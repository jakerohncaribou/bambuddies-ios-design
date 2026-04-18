# BamBuddies Go-Live Dependencies

**Tracking launch-blocking items** — cannot ship to production without completing these.

Last updated: 2026-04-18

---

## 🔴 Launch-Blocking Items (Required Before Production)

### Apple App Store Approval

| # | Item | Status | Source |
|---|------|--------|--------|
| L1 | **Apple Sign-In** on all auth screens | Design: pending / Code: native token schema exists | App Store Guideline 4.8 — required if offering other SSO like Google |
| L2 | **In-app account deletion** flow | Design: pending / Code: pending | App Store Guideline 5.1.1(v) — iOS apps with accounts must allow deletion |
| L3 | **Report content / user** | Design: pending / Code: pending | App Store Guideline 1.2 — required for user-generated content apps |
| L4 | **Block user** | Design: pending / Code: pending | App Store Guideline 1.2 — moderation tools |
| L5 | **Content moderation response** | Design: pending / Code: pending | Acknowledge reports within 24 hours |

### Legal / Compliance

| # | Item | Status | Source |
|---|------|--------|--------|
| L6 | **Terms of Service — public page** | Design: pending / Code: page exists | Standard legal requirement |
| L7 | **Privacy Policy — public page** | Design: pending / Code: page exists | GDPR / CCPA / App Store requirement |
| L8 | **Privacy disclosures** in App Store Connect | N/A (process) | Required for app submission |
| L9 | **Data deletion confirmation** email/notification | Pending | Some jurisdictions require |

### Core User Product

| # | Item | Status | Source |
|---|------|--------|--------|
| L10 | **All auth deeplink flows** (game invite + new user) | Design: pending / Code: working | Critical user acquisition path |
| L11 | **Production login/signup pages** restored | Restored from .bak | Memory note: project_golive_deps |
| L12 | **Pre-launch waitlist page redesigned** (if we keep it) | Pending | Currently live on bambuddies.com |
| L13 | **Permission primers** (notifications, location, contacts, photos, calendar) | Design: pending | Apple recommends primers before OS prompts |
| L14 | **Error states** (offline, 500, 404) | Design: pending | Crash/error resilience |
| L15 | **Session expiration handling** | Design: pending / Code: auto-refresh exists | Smooth re-auth experience |

### Monetization (Launch with Subscription for Ad-Free)

| # | Item | Status | Source |
|---|------|--------|--------|
| L16 | **Subscription picker (Free vs Ad-Free)** | Design: pending / Code: Stripe integrated | Jake confirmed launching with ad-free subscription tier |
| L17 | **Stripe checkout flow** | Design: pending / Code: `/subscriptions/checkout` exists | Core monetization |
| L18 | **Cancel subscription flow** | Design: pending / Code: portal exists | Required for fair dealing |
| L19 | **Billing history / invoices** | Design: pending / Code: portal handles | User transparency |
| L20 | **Ad removal confirmation** | Design: pending | Post-subscription UX |
| L21 | **Apple In-App Purchase** (iOS) | Pending | App Store requires IAP for digital goods — NOT Stripe |

**⚠️ IMPORTANT NOTE ON L21:** Apple requires digital subscriptions (ad-free tier) to use In-App Purchase on iOS, not Stripe. Stripe only works on web. This means:
- Web: Stripe checkout ✅
- iOS: Apple IAP — needs separate implementation + App Store Connect setup

### Go-Live Site Restoration

| # | Item | Status | Source |
|---|------|--------|--------|
| L22 | **Restore prod login page from .bak** | Memory: outstanding | project_golive_deps memory |
| L23 | **Restore prod signup page from .bak** | Memory: outstanding | project_golive_deps memory |

---

## 🟡 Soft Launch Items (Ship Without, Fix Later)

| # | Item | Acceptable post-launch? |
|---|------|-------------------------|
| S1 | Recurring games UI polish | Yes — backend works |
| S2 | Multi-table assignment UI | Yes — fallback to auto-assign |
| S3 | Advanced notification preferences | Yes — 3 global toggles work |
| S4 | Bring items polish | Yes — basic flow works |
| S5 | Direct message screen | Yes — can be added post-launch |
| S6 | Granular search filters | Yes — basic filters sufficient |
| S7 | Organizer transfer flow | Yes — can be support-handled |
| S8 | App Store rating prompt | No rush — don't ask until users have value |

---

## 🟢 Launch Verifications (Pre-Release Checklist)

- [ ] All auth flows work end-to-end (register, login, SSO, password reset)
- [ ] Apple Sign-In works and returns to correct screen
- [ ] Game invite deeplink works for new and existing users
- [ ] Group invite deeplink works for new and existing users
- [ ] Game creation wizard completes successfully
- [ ] Game edit/cancel works
- [ ] Invite players: manual, favorites, contacts, group members all work
- [ ] Applicant approval/rejection works
- [ ] Game review submission works
- [ ] Group CRUD works
- [ ] Group discovery + application works
- [ ] Messaging: thread list, chat, image upload all work
- [ ] Notifications list + read/unread state works
- [ ] Push notifications deliver
- [ ] Subscription checkout completes
- [ ] Subscription cancellation works
- [ ] Ad removal activates immediately after subscription
- [ ] Apple IAP alternative works on iOS
- [ ] Account deletion works end-to-end (data removed)
- [ ] Report user/message workflow completes
- [ ] Block user workflow completes
- [ ] All empty states show correct messaging
- [ ] Offline banner displays on network loss
- [ ] 500/404 errors show friendly pages
- [ ] All permission prompts have primers
- [ ] Terms of Service and Privacy Policy accessible
- [ ] Waitlist page (or full public site) live
- [ ] App Store Connect submission complete (iOS)
- [ ] Legal review sign-off
- [ ] Privacy/compliance review sign-off
- [ ] Performance testing passed
- [ ] Security audit passed
- [ ] Load testing passed

---

## 📚 Related Docs

- [GAP_ANALYSIS.md](./GAP_ANALYSIS.md) — Full list of design gaps
- [BUILD_PLAN.md](./BUILD_PLAN.md) — v2 parallel build strategy
- [ENHANCEMENTS.md](./ENHANCEMENTS.md) — Design decisions and change log

---

*Updated as items are completed or new dependencies emerge.*
