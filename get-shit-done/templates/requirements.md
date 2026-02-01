# Requirements Template

Template for `.planning/REQUIREMENTS.md` — checkable requirements that define "done."

<template>

```markdown
# Requirements: [Project Name]

**Defined:** [date]
**Core Value:** [from PROJECT.md]

## v1 Requirements

Requirements for initial release. Each maps to roadmap phases.

### Authentication

- [ ] **AUTH-01**: User can sign up with email and password
- [ ] **AUTH-02**: User receives email verification after signup
- [ ] **AUTH-03**: User can reset password via email link
- [ ] **AUTH-04**: User session persists across browser refresh

### [Category 2]

- [ ] **[CAT]-01**: [Requirement description]
- [ ] **[CAT]-02**: [Requirement description]
- [ ] **[CAT]-03**: [Requirement description]

### [Category 3]

- [ ] **[CAT]-01**: [Requirement description]
- [ ] **[CAT]-02**: [Requirement description]

## v2 Requirements

Deferred to future release. Tracked but not in current roadmap.

### [Category]

- **[CAT]-01**: [Requirement description]
- **[CAT]-02**: [Requirement description]

## Out of Scope

Explicitly excluded. Documented to prevent scope creep.

| Feature | Reason |
|---------|--------|
| [Feature] | [Why excluded] |
| [Feature] | [Why excluded] |

## Traceability

Which phases cover which requirements. Updated during roadmap creation.

| Requirement | Phase | Status |
|-------------|-------|--------|
| AUTH-01 | Phase 1 | Pending |
| AUTH-02 | Phase 1 | Pending |
| AUTH-03 | Phase 1 | Pending |
| AUTH-04 | Phase 1 | Pending |
| [REQ-ID] | Phase [N] | Pending |

**Coverage:**
- v1 requirements: [X] total
- Mapped to phases: [Y]
- Unmapped: [Z] ⚠️

---
*Requirements defined: [date]*
*Last updated: [date] after [trigger]*
```

</template>

<guidelines>

**Requirement Format:**
- ID: `[CATEGORY]-[NUMBER]` (AUTH-01, CONTENT-02, SOCIAL-03)
- Description: User-centric, testable, atomic
- Checkbox: Only for v1 requirements (v2 are not yet actionable)

**Behavioral Patterns (use when conditions matter):**

Most requirements are simple: "User can [action]". But some behaviors need conditions to be clear. Use these patterns when the trigger, state, or failure case matters:

| Pattern | When to Use | Example |
|---------|-------------|---------|
| `User can [action]` | Default — unconditional capability | User can create a post |
| `When [event], [outcome]` | Response to trigger | When signup succeeds, user receives verification email |
| `While [state], [behavior]` | During a mode/condition | While offline, app queues changes for sync |
| `If [condition], [response]` | Error/edge handling | If login fails 3 times, account locks for 15 minutes |

**When to use behavioral patterns:**
- The requirement involves a trigger (login, timeout, payment, network change)
- The requirement depends on state (offline, loading, premium, unverified)
- The requirement handles failure (retry, notify, degrade, block)
- The requirement has boundaries (limits, empty states, max capacity)

**Don't over-apply:** Simple capabilities don't need conditions. "User can edit profile" is fine — you don't need "When user clicks edit, system shows edit form."

**Categories:**
- Derive from research FEATURES.md categories
- Keep consistent with domain conventions
- Typical: Authentication, Content, Social, Notifications, Moderation, Payments, Admin

**v1 vs v2:**
- v1: Committed scope, will be in roadmap phases
- v2: Acknowledged but deferred, not in current roadmap
- Moving v2 → v1 requires roadmap update

**Out of Scope:**
- Explicit exclusions with reasoning
- Prevents "why didn't you include X?" later
- Anti-features from research belong here with warnings

**Traceability:**
- Empty initially, populated during roadmap creation
- Each requirement maps to exactly one phase
- Unmapped requirements = roadmap gap

**Status Values:**
- Pending: Not started
- In Progress: Phase is active
- Complete: Requirement verified
- Blocked: Waiting on external factor

</guidelines>

<evolution>

**After each phase completes:**
1. Mark covered requirements as Complete
2. Update traceability status
3. Note any requirements that changed scope

**After roadmap updates:**
1. Verify all v1 requirements still mapped
2. Add new requirements if scope expanded
3. Move requirements to v2/out of scope if descoped

**Requirement completion criteria:**
- Requirement is "Complete" when:
  - Feature is implemented
  - Feature is verified (tests pass, manual check done)
  - Feature is committed

</evolution>

<example>

```markdown
# Requirements: CommunityApp

**Defined:** 2025-01-14
**Core Value:** Users can share and discuss content with people who share their interests

## v1 Requirements

### Authentication

- [ ] **AUTH-01**: User can sign up with email and password
- [ ] **AUTH-02**: When signup succeeds, user receives verification email
- [ ] **AUTH-03**: User can reset password via email link
- [ ] **AUTH-04**: User session persists across browser refresh
- [ ] **AUTH-05**: If login fails 3 times, account locks for 15 minutes
- [ ] **AUTH-06**: While unverified, user can browse but not post

### Profiles

- [ ] **PROF-01**: User can create profile with display name
- [ ] **PROF-02**: User can upload avatar image (max 5MB)
- [ ] **PROF-03**: User can write bio (max 500 chars)
- [ ] **PROF-04**: User can view other users' profiles
- [ ] **PROF-05**: If avatar upload fails, user sees error with retry option

### Content

- [ ] **CONT-01**: User can create text post
- [ ] **CONT-02**: User can upload image with post (max 10MB)
- [ ] **CONT-03**: User can edit own posts within 24 hours
- [ ] **CONT-04**: User can delete own posts
- [ ] **CONT-05**: User can view feed of posts
- [ ] **CONT-06**: While feed is loading, user sees skeleton placeholders
- [ ] **CONT-07**: If feed is empty, user sees prompt to follow users

### Social

- [ ] **SOCL-01**: User can follow other users
- [ ] **SOCL-02**: User can unfollow users
- [ ] **SOCL-03**: User can like posts
- [ ] **SOCL-04**: User can comment on posts
- [ ] **SOCL-05**: User can view activity feed (followed users' posts)

## v2 Requirements

### Notifications

- **NOTF-01**: User receives in-app notifications
- **NOTF-02**: User receives email for new followers
- **NOTF-03**: User receives email for comments on own posts
- **NOTF-04**: User can configure notification preferences

### Moderation

- **MODR-01**: User can report content
- **MODR-02**: User can block other users
- **MODR-03**: Admin can view reported content
- **MODR-04**: Admin can remove content
- **MODR-05**: Admin can ban users

## Out of Scope

| Feature | Reason |
|---------|--------|
| Real-time chat | High complexity, not core to community value |
| Video posts | Storage/bandwidth costs, defer to v2+ |
| OAuth login | Email/password sufficient for v1 |
| Mobile app | Web-first, mobile later |

## Traceability

| Requirement | Phase | Status |
|-------------|-------|--------|
| AUTH-01 | Phase 1 | Pending |
| AUTH-02 | Phase 1 | Pending |
| AUTH-03 | Phase 1 | Pending |
| AUTH-04 | Phase 1 | Pending |
| AUTH-05 | Phase 1 | Pending |
| AUTH-06 | Phase 1 | Pending |
| PROF-01 | Phase 2 | Pending |
| PROF-02 | Phase 2 | Pending |
| PROF-03 | Phase 2 | Pending |
| PROF-04 | Phase 2 | Pending |
| PROF-05 | Phase 2 | Pending |
| CONT-01 | Phase 3 | Pending |
| CONT-02 | Phase 3 | Pending |
| CONT-03 | Phase 3 | Pending |
| CONT-04 | Phase 3 | Pending |
| CONT-05 | Phase 3 | Pending |
| CONT-06 | Phase 3 | Pending |
| CONT-07 | Phase 3 | Pending |
| SOCL-01 | Phase 4 | Pending |
| SOCL-02 | Phase 4 | Pending |
| SOCL-03 | Phase 4 | Pending |
| SOCL-04 | Phase 4 | Pending |
| SOCL-05 | Phase 4 | Pending |

**Coverage:**
- v1 requirements: 23 total
- Mapped to phases: 23
- Unmapped: 0 ✓

---
*Requirements defined: 2025-01-14*
*Last updated: 2025-01-14 after initial definition*
```

</example>
