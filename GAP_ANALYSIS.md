# BamBuddies Design Gap Analysis

**Running list of features in staging that are NOT yet mocked up in the designs.**
Updated: 2026-04-18

This doc compares the full feature inventory of the staging codebase against screens in:
- `prototype-ios-pink.html` (30 screens)
- `prototype-web-pink.html` (26 screens)
- `prototype-admin-pink.html` (20 screens)

---

## How to use this doc

- **[P0]** = Critical for launch — block release or represent common user paths
- **[P1]** = Important — significant usage but not launch-blocking
- **[P2]** = Nice-to-have — edge cases, low usage, or future features
- **Status:** `Gap` = not designed yet · `Designed` = covered · `Partial` = some coverage but incomplete
- **Platforms:** iOS / Web / Admin (where the gap applies)

---

## 🔴 P0 — Critical Gaps (Launch Blockers)

### Deeplink / Entry Flows

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| D1 | **Game invite deeplink → unregistered user → register → Accept/Decline modal** | iOS, Web | User clicks share link, gets routed to register, after signup sees `AcceptFirstGameModal` with Accept/Decline + optional bring-item. Key flow — Jake's specific callout. |
| D2 | **Group invite deeplink → unregistered user → register → JoinGroupModal** | iOS, Web | Same pattern but for groups. After signup, prompt to join group + auto-invite to pending group games. |
| D3 | **Game invite deeplink → unauthenticated user (has account) → login → Accept modal** | iOS, Web | Login variant of D1 — already registered, just not signed in. |
| D4 | **Password reset email link landing** | iOS, Web | What the user sees when they click the link in the reset email. Shows `ResetPasswordScreen` with token prefilled. |
| D5 | **Email verification landing** | iOS, Web | Confirmation page after clicking email verify link. |
| D6 | **Share link preview (fallback for non-users)** | Web | When someone without the app clicks a share link — needs a lightweight preview + CTA to download app / register. |
| D7 | **SSO callback processing screen** | Web | Brief "Signing you in..." screen during OAuth callback (`/auth/callback`). |

### Critical Auth

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| A1 | **Apple Sign-In button** | iOS, Web | Currently we have Google SSO; Apple is required on iOS per App Store rules. Add to login + register screens. |
| A2 | **Pre-launch waitlist gate** | Web | `PreLaunchGate` component shows signup waitlist form if user lacks invite. Important for launch. |
| A3 | **Change password flow** | iOS, Web | For SSO users who want to add email+password, or existing users who want to change. POST `/auth/set-password`. |
| A4 | **Session expired / re-auth prompt** | iOS, Web | When access token expires and refresh fails — force re-login modal. |

### Critical Modals & Confirmations

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| M1 | **Cancel game confirmation** | iOS, Web | `ConfirmDialog` with title, reason textarea, warning that participants will be notified. |
| M2 | **Leave game confirmation** | iOS, Web | For players — confirm cancelling participation, optional reason. |
| M3 | **Decline invite confirmation** | iOS, Web | Quick modal on Decline click — "Are you sure?" |
| M4 | **Delete group confirmation** | iOS, Web | Destructive action — type group name to confirm. |
| M5 | **Leave group confirmation** | iOS, Web | "Leave group? You'll be removed from future games." Toggle: remove me from upcoming games. |
| M6 | **Remove player from game** | iOS, Web | Organizer confirmation. |
| M7 | **Remove member from group** | iOS, Web | Organizer confirmation. |
| M8 | **Logout confirmation** | iOS, Web | Simple confirm on Settings → Sign Out. |
| M9 | **Unsaved changes prompt** | iOS, Web | When leaving Game Edit / Profile / Group Edit with unsaved changes. |
| M10 | **Transfer organizer** | iOS, Web | Pick new organizer from participant list, confirm transfer. |

### Critical Profile / User

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| P1 | **Other user's public profile** | iOS, Web | Shown when tapping avatar anywhere. Public info only: name, avatar, level, games count, public games. |
| P2 | **Delete account flow** | iOS, Web | Multi-step: warning, type email, final confirm, success. |
| P3 | **Deactivate account flow** | iOS, Web | Lighter than delete — can be reactivated. |

### Critical Settings

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| S1 | **Saved locations management** | iOS, Web | Add / edit / delete saved locations (My Home, Mom's House, etc.). CRUD screens. |
| S2 | **Connected accounts management** | iOS, Web | View linked Google/Apple/Phone, link/unlink actions. |
| S3 | **Notification preferences granular** | iOS, Web | Per-notification-type preferences (game invite, game confirmed, applicants, messages, etc.) — not just 3 global toggles. |
| S4 | **Privacy settings page** | iOS, Web | Who can see my profile, who can invite me, etc. (Currently no page — placeholder.) |

### Critical Empty / Error States

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| E1 | **Empty groups state** | iOS, Web | User has no groups. "Create one" + "Discover groups" CTAs. |
| E2 | **Empty inbox state** | iOS, Web | No message threads yet. |
| E3 | **Empty notifications state** | iOS, Web | All caught up! illustration. |
| E4 | **Empty search results** | iOS, Web | "No results for X" with tips. |
| E5 | **Offline / network error banner** | iOS, Web | Sticky banner at top of screen when offline. |
| E6 | **Global error state (500)** | iOS, Web | Fallback "Something went wrong" page. |
| E7 | **404 / page not found** | iOS, Web | Redirects to dashboard currently — but should have a screen. |
| E8 | **Account suspended / banned** | iOS, Web | What user sees if admin suspends them. |

### Permission Prompts

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| PR1 | **Notification permission prompt** | iOS | iOS push notification permission primer before OS prompt. |
| PR2 | **Location permission prompt** | iOS, Web | "Allow BamBuddies to find games near you?" primer before OS prompt. |
| PR3 | **Contacts permission prompt** | iOS | Primer before Contacts access for Invite Players flow. |
| PR4 | **Calendar permission prompt** | iOS | Primer before calendar write (Add to Cal feature). |
| PR5 | **Photo library permission prompt** | iOS | Primer before picking photos for review / chat images. |

---

## 🟡 P1 — Important Gaps

### Game Lifecycle (Advanced)

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| G1 | **Multi-table layout & assignment** | iOS, Web | `GameTableVisualization` + `PlayerAssignmentPanel` — drag/drop players to tables. |
| G2 | **Bring items catalog picker** | iOS, Web | Select from catalog of bring items (tiles, snacks, drinks, etc.) during game creation. |
| G3 | **Bring items claim/unclaim UI** | iOS, Web | Within game detail — who's bringing what, claim button. |
| G4 | **Recurring game setup** | iOS, Web | RRULE config — "Repeat weekly on Saturdays", end date, exceptions. |
| G5 | **Recurring game series management** | iOS, Web | View all games in series, pause/resume, delete series. |
| G6 | **Replace-me request** | iOS, Web | Player requests replacement. Notifies eligible players. |
| G7 | **Player cancellation with reason** | iOS, Web | Optional "why are you cancelling?" reason. |
| G8 | **Game status transition screens** | iOS, Web | In Progress state, Confirmed state (different from Pending), Started, Completed with review prompt. |
| G9 | **Post-game completion prompt** | iOS, Web | "Did this game happen? Mark complete" push for organizer 24h after scheduled end. |
| G10 | **Share game modal** | iOS, Web | Copy link, Native iOS share sheet, social buttons (WhatsApp, Messages, etc.). |
| G11 | **Cover image upload for game** | iOS, Web | Select from library, upload new, or pick from mahjong stock images. |
| G12 | **Image gallery picker** | iOS, Web | User's previously uploaded game images — reuse for new game/review. |
| G13 | **Invite candidate management** | iOS, Web | View pending candidates (unregistered), resend, revoke invite. |
| G14 | **Game applicants approval/reject reasons** | iOS, Web | Optional reject reason textarea. Partially designed. |

### Messaging (Advanced)

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| MS1 | **Direct message (1-1) screen** | iOS, Web | Separate from game/group threads — dedicated DM UI. |
| MS2 | **Send photo in chat** | iOS, Web | Image picker, preview, send. |
| MS3 | **Mute conversation** | iOS, Web | Long-press thread → mute options (1h, 8h, 1d, forever). |
| MS4 | **Global mute / quiet hours** | iOS, Web | Settings → "Quiet hours" configuration. |
| MS5 | **Read receipts** | iOS | (Not implemented in staging — but may be needed.) |
| MS6 | **Typing indicators** | iOS | (Not implemented in staging — but may be needed.) |

### Groups (Advanced)

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| GR1 | **Apply to group with message** | iOS, Web | Discovery: apply with optional message to organizer. |
| GR2 | **Group application approval (organizer view)** | iOS, Web | List of pending applications with approve/decline. |
| GR3 | **Group organizer dashboard** | iOS, Web | Organizer-specific view of group detail with admin actions. |
| GR4 | **Group invite candidate management** | iOS, Web | Same as G13 but for groups. |
| GR5 | **Transfer group ownership flow** | iOS, Web | Pick new organizer from members, confirm, notify. |

### Discover / Search

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| DS1 | **Global search results** | iOS, Web | Unified search across games, groups, players — tabbed. |
| DS2 | **Filter panel (advanced)** | iOS, Web | Expanded filter sheet with multiple criteria. |

### Billing

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| B1 | **Subscription picker / plans** | iOS, Web | Plan tiers with features comparison. |
| B2 | **Stripe checkout handoff** | Web | "Redirecting to payment..." screen. |
| B3 | **Billing history / invoices** | Web | List of past invoices with download. |
| B4 | **Cancel subscription flow** | Web | Reason survey, confirmation, effective date. |
| B5 | **Payment method management** | Web | Stripe customer portal deep link. |

### Admin (Gaps in existing admin prototype)

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| AD1 | **Admin recurring series management** | Admin | View/manage all recurring series across platform. |
| AD2 | **Admin billing / subscription view** | Admin | Per-org subscription status, overdue alerts. |
| AD3 | **Admin waitlist management** | Admin | Pre-launch waitlist signups management. |
| AD4 | **Admin DB reset screen** (staging only) | Admin | Utility screen for staging environment. |
| AD5 | **Org admin billing** | Admin | Org-level billing view. |
| AD6 | **Feature flag management UI** | Admin | (Currently env vars only — may need UI.) |

---

## 🟢 P2 — Nice-to-Have / Future

### User-Facing

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| X1 | **Report message / user** | iOS, Web | Not in staging yet, but likely needed. |
| X2 | **Block user** | iOS, Web | Not in staging yet. |
| X3 | **Help center / FAQ** | iOS, Web | Static content or embedded. |
| X4 | **Contact support form** | iOS, Web | Already in staging as endpoint (`POST /contact`). |
| X5 | **Language picker** | iOS, Web | Per-user locale (already stored; no UI yet). |
| X6 | **App Store / Play Store rating prompt** | iOS | Post-positive-event nudge. |
| X7 | **Badges / achievements screen** | iOS, Web | Not in staging yet. |
| X8 | **Reviews received section on profile** | iOS, Web | Public reviews from past games. |
| X9 | **Follow / friend system** | iOS, Web | Not in staging yet. |
| X10 | **Full Terms of Service page** | iOS, Web | Currently just a link — may need proper in-app page. |
| X11 | **Full Privacy Policy page** | iOS, Web | Same as X10. |
| X12 | **About / Credits page** | iOS, Web | Version, build number, legal. |
| X13 | **Discovery thread (public game chat)** | iOS, Web | Public discussion about a public game — currently an endpoint exists. |

### System States

| # | Gap | Platform | Notes |
|---|-----|----------|-------|
| X14 | **Rate limited message** | iOS, Web | "Slow down!" banner when hitting rate limits. |
| X15 | **Feature flag off state** | iOS, Web | When a feature is flagged off per-user or per-org. |
| X16 | **Loading state polish (skeleton variants)** | iOS, Web | Screen-specific skeleton designs. |

---

## 📝 Already Designed (For reference)

These are covered — listing them here so we know what's NOT a gap.

### iOS (prototype-ios-pink.html — 30 screens)
- Login, Register, Forgot Password, Reset Password, Verify Phone, Complete Profile
- Dashboard, Dashboard Empty, My Games, Game Detail, Game Edit
- Game Creation (3 steps), Applicants, Applicant Profile, Game Review
- Discover, Groups, Group Detail, Group Creation, Group Discovery
- Group Invite Members, Invite Manual, Invite Favorites
- Inbox, Chat, Notifications, Profile Settings (with theme picker)
- Modal: First Game Accept (AcceptFirstGameModal)

### Web (prototype-web-pink.html — 26 screens)
- Auth: login, register, forgot, reset, verify
- Main: dashboard, myGames, gameDetail, 3-step gameCreate, gameEdit, applicants, gameReview
- Discover: map + list view, groups, groupDetail, groupCreate, groupDiscover
- Messaging: inbox, chat, notifications
- Settings (with theme picker), completeProfile
- Admin landing

### Admin (prototype-admin-pink.html — 20 screens)
- Platform Admin: Login, Dashboard, Users, User Detail, Games, Game Detail, Orgs, Org Detail, Analytics, Reports/Moderation, Settings, Activity Log
- Org Admin: Login, Dashboard, Games, Members, Analytics, Branding, Settings, Admins

---

## 🎯 Recommended Priority Order for Design Sprint

Based on launch criticality and user experience impact:

### Phase 1 (Blocking launch)
1. D1, D2 — Deeplink + registration + accept/join modals
2. A1 — Apple Sign-In
3. M1–M10 — All confirmation modals
4. P1 — Other user's public profile
5. S1 — Saved locations management
6. E1–E4 — Empty states
7. A2 — Pre-launch waitlist gate
8. PR1–PR5 — Permission prompts

### Phase 2 (Important for complete product)
1. G1, G4 — Multi-table + Recurring games
2. G2, G3 — Bring items
3. MS1 — Direct message screen
4. GR1, GR2 — Group apply + approvals
5. P2 — Delete account flow
6. S2, S3 — Connected accounts + granular notifications
7. B1–B5 — Billing (if launching with subscriptions)

### Phase 3 (Polish / future)
1. X1, X2 — Report / Block
2. X3, X4 — Help + contact
3. X7, X8 — Badges + reviews
4. Other P2 items

---

## 📌 Open Questions for Jake

1. **Apple Sign-In** — confirm we need it for iOS launch (App Store guidelines)?
2. **Payments/Subscriptions at launch** — are we launching with free-tier only, or do paid plans need design?
3. **Report/Block** — is this P0 for Apple App Store approval? (Apple requires moderation tools.)
4. **Pre-launch waitlist** — is this still in the product or removed?
5. **Recurring games** — is this P0 or P1 for launch? (Heavily used by regular mahjong groups.)
6. **Delete account** — App Store requires it for iOS. P0.
7. **Language** — English only at launch, or multi-lang from day 1?

---

*This document will be updated as gaps are designed or new features are added to staging.*
