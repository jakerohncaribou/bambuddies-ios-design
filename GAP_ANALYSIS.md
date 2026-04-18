# BamBuddies Design Gap Analysis

**Running list of features in staging that are NOT yet mocked up in the designs.**
Last updated: 2026-04-18 (Sprints 1-6 complete)

## ✅ Status: 60+ P0/P1 gap items designed across 6 sprints

**Designed in sprint files (see index-full.html):**
- Sprint 1 (`prototype-flows.html`) — D1-D7, A1, A4, M1-M10
- Sprint 2 (`prototype-profile-settings.html`) — P1-P2, S1-S4, E1-E8
- Sprint 3 (`prototype-moderation-permissions.html`) — MOD1-3, PR1-5
- Sprint 4 (`prototype-public-site.html`) — W1-W10 (public marketing site)
- Sprint 5 (`prototype-billing.html`) — B1-B7 (subscription + Apple IAP)
- Sprint 6 (`prototype-advanced.html`) — G1, G4-G6, G9-G10, GR2, MS1

**Remaining gaps** (lower priority, can be added post-launch):
- G2, G3 (bring items picker/claim UI)
- G7 (player cancellation with reason)
- G8 (in-progress/started visual states)
- G11, G12 (cover image upload, image gallery)
- G13 (invite candidate management)
- GR1, GR3, GR4, GR5 (apply with message, org dashboard, invite candidates, transfer)
- MS2, MS3, MS4 (chat photo, mute, quiet hours)
- AD1-AD5 (admin: recurring series, billing, waitlist, DB reset)
- X1-X16 (P2 nice-to-haves)

---

This doc compares the full feature inventory of the staging codebase against screens in:
- `prototype-ios-pink.html` (30 screens)
- `prototype-web-pink.html` (26 screens)
- `prototype-admin-pink.html` (20 screens)

---

## ✅ Answered Questions (Jake's direction)

1. **Apple Sign-In** → YES, design it
2. **Subscription to remove ads** → YES, mock up the process. Added to go-live deps.
3. **Report / Block users** → Required for Apple Store approval → design + go-live dep
4. **Waitlist / Public website** → Waitlist page is LIVE on production ("Good things are coming"). Entire public website also needs redesign as part of this scope.
5. **Recurring games** → YES, design it (fully functional in staging)
6. **Delete account** → YES, required by Apple → design + go-live dep
7. **Language** → English only at launch. Multi-language in a future build.

---

## How to use this doc

- **[P0]** = Critical for launch — block release or represent common user paths
- **[P1]** = Important — significant usage but not launch-blocking
- **[P2]** = Nice-to-have — edge cases, low usage, or future features
- **Status:** `Gap` = not designed yet · `Designed` = covered · `Partial` = some coverage but incomplete
- **Platforms:** iOS / Web / Admin / Public (marketing site)

---

## 🔴 P0 — Critical Gaps (Launch Blockers)

### 🌐 Public Website (NEW SCOPE — entire site to be redesigned)

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| W1 | **Pre-launch waitlist page** ("Good things are coming") | Public | Current live version. Single email capture. Redesign in pink v9. |
| W2 | **Homepage / hero** | Public | Post-launch homepage with hero, features overview, CTA. |
| W3 | **Features / How it works** | Public | Section explaining app features with visuals. |
| W4 | **About page** | Public | Story, team, mission. |
| W5 | **Pricing page** | Public | Free tier + ad-free subscription tier. (Depends on B1.) |
| W6 | **FAQ / Help** | Public | Common questions, contact. |
| W7 | **Contact support** | Public | Form → POST `/contact`. |
| W8 | **Terms of Service** | Public | Full page, not in-app. |
| W9 | **Privacy Policy** | Public | Full page, not in-app. |
| W10 | **Download app** | Public | iOS link, coming soon for Android. |
| W11 | **Blog** (future) | Public | P2 — not for launch. |

### 🔗 Deeplink / Entry Flows

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| D1 | **Game invite deeplink → unregistered user → register → Accept/Decline modal** | iOS, Web | User clicks share link, routed to register, after signup sees `AcceptFirstGameModal`. Key flow — Jake's specific callout. |
| D2 | **Group invite deeplink → unregistered user → register → JoinGroupModal** | iOS, Web | Same pattern for groups. Auto-invite to pending group games. |
| D3 | **Game invite deeplink → unauthenticated user → login → Accept modal** | iOS, Web | Login variant of D1 — already registered, just not signed in. |
| D4 | **Password reset email link landing** | iOS, Web | What user sees when clicking reset email link. |
| D5 | **Email verification landing** | iOS, Web | Post-verify confirmation. |
| D6 | **Share link preview (non-users)** | Public | Open Graph / preview when sharing — lightweight landing with Download CTA. |
| D7 | **SSO callback processing** | Web | "Signing you in..." during OAuth callback. |

### 🔐 Auth

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| A1 | **Apple Sign-In button** ✅ CONFIRMED | iOS, Web | Required per App Store. Add to login + register. |
| A3 | **Change password flow** | iOS, Web | For SSO users adding password or existing users changing. |
| A4 | **Session expired / re-auth prompt** | iOS, Web | When refresh fails. |

### 💬 Modals & Confirmations

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| M1 | **Cancel game confirmation** | iOS, Web | With optional reason. |
| M2 | **Leave game confirmation** | iOS, Web | For players cancelling participation. |
| M3 | **Decline invite confirmation** | iOS, Web | Quick confirm on Decline. |
| M4 | **Delete group confirmation** | iOS, Web | Type group name to confirm. |
| M5 | **Leave group confirmation** | iOS, Web | Toggle: remove from upcoming games. |
| M6 | **Remove player from game** | iOS, Web | Organizer action. |
| M7 | **Remove member from group** | iOS, Web | Organizer action. |
| M8 | **Logout confirmation** | iOS, Web | Simple confirm. |
| M9 | **Unsaved changes prompt** | iOS, Web | Prevent data loss on navigate-away. |
| M10 | **Transfer organizer** | iOS, Web | Pick new organizer, confirm. |

### 👤 Profile / User

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| P1 | **Other user's public profile** | iOS, Web | Tap avatar → public info (name, level, games count). |
| P2 | **Delete account flow** ✅ CONFIRMED | iOS, Web | Multi-step required by Apple. Warning → type email → final confirm. GO-LIVE DEP. |
| P3 | **Deactivate account flow** | iOS, Web | Lighter than delete — reactivatable. |

### ⚙️ Settings

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| S1 | **Saved locations management** | iOS, Web | CRUD for saved locations (My Home, Mom's House, etc.). |
| S2 | **Connected accounts management** | iOS, Web | View linked Google/Apple/Phone, link/unlink. |
| S3 | **Notification preferences granular** | iOS, Web | Per-type prefs, not just 3 global toggles. |
| S4 | **Privacy settings page** | iOS, Web | Who can see profile, invite me, etc. |

### 🚫 Moderation (Apple requirement)

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| MOD1 | **Report user / message / content** ✅ CONFIRMED | iOS, Web | Required for App Store approval. GO-LIVE DEP. |
| MOD2 | **Block user** ✅ CONFIRMED | iOS, Web | Required for App Store approval. GO-LIVE DEP. |
| MOD3 | **Moderation response (post-report)** | iOS, Web | "Thanks, we'll review" confirmation. |

### 🫙 Empty / Error States

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| E1 | **Empty groups state** | iOS, Web | "Create one" + "Discover groups" CTAs. |
| E2 | **Empty inbox state** | iOS, Web | No message threads yet. |
| E3 | **Empty notifications state** | iOS, Web | All caught up illustration. |
| E4 | **Empty search results** | iOS, Web | "No results for X" + tips. |
| E5 | **Offline / network error banner** | iOS, Web | Sticky banner. |
| E6 | **Global error state (500)** | iOS, Web | "Something went wrong" fallback. |
| E7 | **404 / page not found** | iOS, Web | Dedicated screen. |
| E8 | **Account suspended / banned** | iOS, Web | Post-suspension screen. |

### 📱 Permission Prompts (iOS)

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| PR1 | **Notification permission prompt (primer)** | iOS | Before OS prompt. |
| PR2 | **Location permission prompt (primer)** | iOS, Web | Before OS prompt. |
| PR3 | **Contacts permission prompt** | iOS | For Invite Players flow. |
| PR4 | **Calendar permission prompt** | iOS | For Add to Cal feature. |
| PR5 | **Photo library permission prompt** | iOS | For avatar / review photos. |

---

## 🟡 P1 — Important Gaps

### 🎮 Game Lifecycle (Advanced)

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| G1 | **Multi-table layout & assignment** | iOS, Web | Drag/drop players to tables. |
| G2 | **Bring items catalog picker** | iOS, Web | Select from catalog during creation. |
| G3 | **Bring items claim/unclaim UI** | iOS, Web | Within game detail. |
| G4 | **Recurring game setup** ✅ CONFIRMED P0→P1 | iOS, Web | RRULE config — "Repeat weekly". Fully functional in staging. |
| G5 | **Recurring series management** | iOS, Web | View all games in series, pause/resume. |
| G6 | **Replace-me request flow** | iOS, Web | Player requests replacement. |
| G7 | **Player cancellation with reason** | iOS, Web | Optional reason textarea. |
| G8 | **Game in-progress / confirmed / started states** | iOS, Web | Different visual states than Pending. |
| G9 | **Post-game completion prompt** | iOS, Web | "Did this happen?" 24h after scheduled end. |
| G10 | **Share game modal** | iOS, Web | Copy link + native share sheet. |
| G11 | **Cover image upload for game** | iOS, Web | Pick from library, upload new, or stock. |
| G12 | **Image gallery picker** | iOS, Web | User's previously uploaded images. |
| G13 | **Invite candidate management** | iOS, Web | View pending, resend, revoke. |

### 💰 Subscription / Billing ✅ CONFIRMED

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| B1 | **Subscription picker (plans)** | iOS, Web | Free (with ads) vs Ad-free tiers. GO-LIVE DEP. |
| B2 | **Stripe checkout handoff** | Web | "Redirecting to payment" screen. |
| B3 | **Billing history / invoices** | Web | Past invoices. |
| B4 | **Cancel subscription flow** | Web | Survey + confirmation. |
| B5 | **Payment method management** | Web | Stripe portal deep link. |
| B6 | **Upgrade/downgrade flow** | iOS, Web | Change plan. |
| B7 | **Ad removal confirmation** | iOS, Web | Post-subscription "Ads removed!" confirmation. |

### 💬 Messaging (Advanced)

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| MS1 | **Direct message (1-1) screen** | iOS, Web | Separate from game/group threads. |
| MS2 | **Send photo in chat** | iOS, Web | Image picker, preview, send. |
| MS3 | **Mute conversation** | iOS, Web | 1h / 8h / 1d / forever. |
| MS4 | **Global mute / quiet hours** | iOS, Web | Settings config. |

### 👥 Groups (Advanced)

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| GR1 | **Apply to group with message** | iOS, Web | Optional message to organizer. |
| GR2 | **Group application approvals (organizer view)** | iOS, Web | Approve/decline pending members. |
| GR3 | **Group organizer dashboard** | iOS, Web | Admin view of group. |
| GR4 | **Group invite candidates** | iOS, Web | Same as G13 but for groups. |
| GR5 | **Transfer group ownership** | iOS, Web | Pick new organizer. |

### 🗺️ Discover / Search

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| DS1 | **Global search results** | iOS, Web | Unified search across games / groups / players. |
| DS2 | **Advanced filters panel** | iOS, Web | Expanded filter sheet. |

### 👑 Admin Gaps

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| AD1 | **Recurring series management** | Admin | Platform view. |
| AD2 | **Billing / subscription view** | Admin | Per-org subscription status. |
| AD3 | **Waitlist management** | Admin | Pre-launch signups. |
| AD4 | **DB reset utility** (staging only) | Admin | Dev tool. |
| AD5 | **Org admin billing** | Admin | Org-level. |

---

## 🟢 P2 — Nice-to-Have / Future

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| X1 | **Help center / FAQ** | iOS, Web | Static content. |
| X2 | **Contact support form (in-app)** | iOS, Web | (Public site has it.) |
| X7 | **Badges / achievements** | iOS, Web | Not in staging yet. |
| X8 | **Reviews received section on profile** | iOS, Web | Public reviews. |
| X9 | **Follow / friend system** | iOS, Web | Future. |
| X12 | **About / Credits page (in-app)** | iOS, Web | Version, legal. |
| X13 | **Discovery thread (public game chat)** | iOS, Web | Endpoint exists, no UI yet. |
| X14 | **Rate limited banner** | iOS, Web | "Slow down!" message. |
| X15 | **App Store rating prompt** | iOS | Post-positive-event. |
| X16 | **Multi-language support** | iOS, Web | Confirmed future build. |

---

## 📝 Already Designed (For Reference)

### iOS (30 screens)
Dashboard, Dashboard Empty, My Games, Game Detail, Game Edit, Game Creation (3 steps), Applicants, Applicant Profile, Game Review, Discover, Groups, Group Detail, Group Creation, Group Discovery, Group Invite Members, Invite Manual, Invite Favorites, Inbox, Chat, Notifications, Profile Settings (with theme picker), Login, Register, Forgot Password, Reset Password, Verify Phone, Complete Profile, Modal: First Game Accept

### Web (26 screens)
Login, Register, Forgot, Reset, Verify, Dashboard, My Games, Game Detail, 3-step Game Create, Game Edit, Applicants, Game Review, Discover (map + list), Groups, Group Detail, Group Create, Group Discover, Inbox, Chat, Notifications, Settings (with theme picker), Complete Profile, Admin landing

### Admin (20 screens)
Platform Admin (12): Login, Dashboard, Users, User Detail, Games, Game Detail, Orgs, Org Detail, Analytics, Reports/Moderation, Settings, Activity Log
Org Admin (8): Login, Dashboard, Games, Members, Analytics, Branding, Settings, Admins

---

## 🎯 Recommended Design Sprint Order

### Sprint 1: Deeplinks + Critical Modals
- D1, D2, D3 (deeplink flows)
- M1–M10 (confirmation modals)
- A1 (Apple Sign-In)
- A4 (session expired)

### Sprint 2: Profile, Settings, Empty States
- P1 (public profile)
- P2 (delete account) — GO-LIVE DEP
- S1–S4 (saved locations, connected accounts, granular notifs, privacy)
- E1–E8 (empty states + errors)

### Sprint 3: Moderation + Permissions
- MOD1, MOD2, MOD3 — GO-LIVE DEP
- PR1–PR5 (permission primers)

### Sprint 4: Public Website (all pages)
- W1–W11

### Sprint 5: Subscriptions & Billing
- B1–B7 — GO-LIVE DEP

### Sprint 6: Game/Group advanced flows
- G1–G13
- GR1–GR5

### Sprint 7: Admin gaps + messaging advanced
- AD1–AD5
- MS1–MS4

### Sprint 8: P2 polish
- X1–X16

---

## 📌 Go-Live Dependencies (track separately)

See [GO_LIVE_DEPS.md](./GO_LIVE_DEPS.md) for launch-blocking items.

*This document is updated as gaps are designed or new features are added to staging.*
