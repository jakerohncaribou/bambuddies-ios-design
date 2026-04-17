# BamBuddies v2 Build Plan

**Status:** Planning · not yet started
**Decision date:** 2026-04-17
**Owner:** Jake Rohn

---

## TL;DR

Ship the new designs as **v2**, running in parallel with current v1. Users log into either with the same credentials, data is shared via a single backend, and v1 is sunset once v2 reaches feature parity and user migration completes.

---

## Why parallel v1 / v2 (not in-place migration)

- Zero risk to current UAT — v1 keeps running unchanged
- Users can compare functionality side-by-side and report gaps
- Zero-downtime rollback if v2 has issues (users just use v1)
- iOS app launches directly on v2 design — no legacy mobile experience
- Sunsetting v1 becomes trivial: delete package + add a redirect

---

## Monorepo structure

```
packages/
  web/              ← v1 (current) — unchanged, Vite + React SPA
  web-v2/           ← NEW — new emerald responsive web experience
  mobile/           ← NEW — iOS React Native app (mauve theme)
  api/              ← shared Node/Express backend — no changes needed
  shared/           ← shared Zod schemas/types (existing)
  design-system/    ← NEW — design tokens + components shared by web-v2 AND mobile
```

### Why the shared `design-system` package

Both `web-v2` and `mobile` need the same look and feel (per Jake's design direction decision). A single tokens + primitives package prevents drift:
- Color tokens (emerald for web, mauve for iOS, shared structure)
- Spacing scale (4, 8, 12, 16, 24, 32, 48)
- Typography ramp (DM Serif Display, Outfit, Inter, Caveat)
- Component primitives (Button, Card, Pill, Avatar, etc.)
- Icon wrapper (Phosphor Icons, same names)
- Status color definitions (locked: PENDING amber, CANCELLED rose, etc.)

---

## Deployment topology

| Environment | URL | What runs there | AWS resources |
|---|---|---|---|
| **v1** | `bambuddies.com` | Current production | Existing EB environment |
| **v2** | `v2.bambuddies.com` | New emerald web | **NEW** EB environment |
| **API** | `api.bambuddies.com` | Backend (shared) | Existing EB environment |
| **iOS** | App Store | React Native app | iOS App Store Connect |

**Key constraint:** Same API backend serves all three surfaces. Same JWT auth works cross-surface. Zero data migration risk.

### DNS / infra setup

1. Register `v2.bambuddies.com` subdomain
2. Create new Elastic Beanstalk environment (same AWS app, new env)
3. Deploy `packages/web-v2` build artifacts
4. CloudFront distribution for static assets if needed
5. Point `v2.bambuddies.com` at the new EB environment

---

## Auth strategy (cross-version)

**Shared JWT tokens.** A token issued by v1 login works identically on v2 because both hit the same `packages/api`.

**Single sign-on options:**

**Option A (preferred):** Root-domain cookie
- Store token as cookie on `.bambuddies.com` (not `bambuddies.com`)
- Both v1 and v2 subdomains can read it
- User logs in once, both versions see the session

**Option B (fallback):** User logs in twice
- Same credentials, different localStorage on each subdomain
- Slightly less smooth but still functional
- Safer if cookie domain configuration is tricky

---

## Build sequence (when we start coding)

### Phase 0: Foundation (1-2 weeks)
1. Create `packages/design-system` with tokens + primitives
2. Set up Storybook or similar for design system review
3. Audit v1 component inventory so v2 knows what to match

### Phase 1: Scaffolding (1 week)
1. Create `packages/web-v2` (Vite + React + TypeScript)
2. Copy auth + API client utilities from `packages/web`
3. Set up routing skeleton matching v1 routes
4. Wire up `packages/design-system` tokens

### Phase 2: Feature-by-feature port (4-6 weeks)
Build screens in priority order, matching v1 functionality:

**Week 1-2 (high traffic):**
- Login / Register / Forgot Password / Reset / Verify Phone
- Dashboard (including empty state for first-time users)
- My Games (with Needs Your Attention + NEXT UP highlight)

**Week 3-4 (core flows):**
- Game Detail (with table graphic, inline chat composer)
- Game Creation (3-step wizard, interactive game type, expanded invites)
- Game Edit
- Applicants + Applicant Profile (PII-safe)
- Game Review (skill chips, not stars)

**Week 5-6 (social features):**
- Discover (real Leaflet map, POI interaction, view toggle)
- My Groups / Group Detail / Group Creation / Group Invite Members hub
- Inbox / Chat Thread (no DM per current spec)
- Notifications
- Profile & Settings (with theme picker)

### Phase 3: Parity QA (2 weeks)
- Feature parity test matrix (every v1 feature verified in v2)
- UAT from internal testers comparing v1 ↔ v2
- Performance + accessibility audit
- Bug fix pass

### Phase 4: Soft launch (2-3 weeks)
- Deploy v2 to `v2.bambuddies.com`
- Banner on v1: "Try the new BamBuddies" → links to v2
- Monitor opt-in rate, engagement, error rate
- Collect feedback

### Phase 5: Default switch (2-4 weeks)
- New signups default to v2
- Existing users prompted to upgrade
- Feature flag for targeted rollout (e.g., 10% → 50% → 100%)

### Phase 6: Migration complete (2-4 weeks)
- Force migrate remaining v1 users
- v1 becomes read-only
- Verify no traffic on v1 API endpoints beyond staff

### Phase 7: Sunset (1 week)
- `bambuddies.com` redirects to v2
- `packages/web` archived to a branch
- Delete from active monorepo after 90-day backup period
- v2 becomes the singular web experience

---

## iOS app build (parallel track)

The iOS app is independent of the v1/v2 split — it launches directly on v2 design.

**Prerequisites:**
- `packages/design-system` exists (tokens shared with web-v2)
- `packages/mobile` scaffolded with React Native + Expo

**Sequencing:**
- Can start in parallel with web-v2 once design system exists
- Launches when App Store review passes + core features work
- Probably ships around Phase 4 / 5 of the web migration (when v2 is stable)

---

## Risks and mitigations

| Risk | Mitigation |
|---|---|
| Feature parity gap between v1 and v2 | Strict test matrix, UAT comparison, parity gate blocks v1 sunset |
| v2 has show-stopping bug | Users can switch back to v1 via subdomain (zero rollback cost) |
| Auth cookie doesn't sync between subdomains | Fallback to log in twice (same credentials) |
| Monorepo complexity | Nothing new — already have `packages/web`, `packages/api`, `packages/shared` |
| Design drift between web-v2 and iOS | Enforce via `packages/design-system` — both consume same tokens |
| Users confused by two versions | Clear banner + one-click switch + honest communication ("we're rebuilding, preview it here") |

---

## Open decisions (to revisit before build starts)

- [ ] Subdomain: `v2.` or `app.` or something else?
- [ ] Cookie domain strategy: root-domain SSO or log-in-twice fallback?
- [ ] Feature flag system: ship with or roll our own?
- [ ] When does v1 sunset? Set a target date (e.g., 6 months post-v2-launch).
- [ ] What's the UAT testing process during parallel run? Discord? Slack? TestFlight-equivalent for web?

---

## Definitions

- **v1** — current BamBuddies web app (Vite + React SPA on Elastic Beanstalk)
- **v2** — the new emerald responsive web experience
- **Feature parity** — every user-facing feature in v1 works identically in v2 (same inputs → same outputs)
- **Sunset** — v1 is fully retired, no longer accessible

---

## Related docs

- `/docs/ios/mockups/` — iOS and web prototypes (design reference)
- `/docs/ios/ENHANCEMENTS.md` — features deferred from initial launch
- `prototype-ios-full.html` — iOS clickable prototype (mauve)
- `prototype-web-full.html` — responsive web prototype (emerald)
- `prototype-admin-full.html` — admin + org admin mockups (in-progress)
