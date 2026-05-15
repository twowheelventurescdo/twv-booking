# TWO WHEEL VENTURES CDO — COMPLETE SYSTEM ARCHITECTURE & DEVELOPMENT PLAN

**Version:** 1.0 | **Prepared:** May 2026 | **Author:** Systems Architecture Review  
**Business:** Two Wheel Ventures CDO — Scooter & Car Rentals, Cagayan de Oro City, Philippines

---

## TABLE OF CONTENTS

1. [Existing System Audit](#1-existing-system-audit)
2. [Website Structure & UI/UX](#2-website-structure--uiux)
3. [Tech Stack Decision](#3-tech-stack-decision)
4. [Google Sheets Database Design](#4-google-sheets-database-design)
5. [Reservation System Logic](#5-reservation-system-logic)
6. [Blacklist Verification System](#6-blacklist-verification-system)
7. [Admin / Staff Side](#7-admin--staff-side)
8. [Google Sheets Integration & Apps Script](#8-google-sheets-integration--apps-script)
9. [Automation Plan](#9-automation-plan)
10. [Security & Risk Management](#10-security--risk-management)
11. [Development Roadmap](#11-development-roadmap)
12. [Codebase & Folder Structure](#12-codebase--folder-structure)
13. [System Architecture Diagram](#13-system-architecture-diagram)
14. [Key Workflows](#14-key-workflows)
15. [Tech Stack Summary](#15-tech-stack-summary)
16. [Sample Booking Lifecycle](#16-sample-booking-lifecycle)
17. [Sample Blacklist Verification Flow](#17-sample-blacklist-verification-flow)
18. [MVP Build Order](#18-mvp-build-order)

---

## 1. EXISTING SYSTEM AUDIT

### What You Have (Good Foundation)

| Page | Status | What It Does Well | Gaps |
|------|--------|-------------------|------|
| Vacant Page | ✅ Functional | Live availability from Sheets, color-coded statuses | Requires Messenger to book — no self-service |
| Rates Page | ✅ Functional | Live rates from Sheets, destination filtering | Standalone, not linked to booking flow |
| Reservation Form | ✅ Functional | Comprehensive fields, email confirmation | No blacklist check, no doc upload, no staff dashboard |

### Current Fleet (12 Units)

| Tier | Units | Base Rate |
|------|-------|-----------|
| Economy | Beat Black, Beat White, Mio Gear 125 | ₱500+/day |
| Standard | Click Blue, Click Black, Click Red, Fazzio Red | ₱600+/day |
| Sport | PCX 160 White, PCX 160 Black, Aerox 155 x2 | ₱800+/day |
| ADV | ADV 160 Red | ₱1,000+/day |
| Premium | Spresso AT 2025 | ₱1,300+/day |

### Key Observation
Your current stack (GitHub Pages + Google Sheets) is **exactly right** for your scale. Do NOT switch to a paid backend or database yet. The goal is to formalize, integrate, and secure what you already have.

---

## 2. WEBSITE STRUCTURE & UI/UX

### Navigation Structure

```
TWO WHEEL VENTURES CDO
├── 🏠 Home                  → Hero, quick availability check, CTA
├── 🛵 Available Units       → Live calendar availability checker
├── 💰 Rates                 → Fleet tiers + destination rate table
├── 📋 Reserve Now           → Full reservation form
├── 📄 Requirements          → What to bring, verification process
└── 📞 Contact Us            → Facebook, email, location map
```

### Page Structure

#### Home Page
- Hero banner with brand name, tagline, CTA button ("Check Availability" + "Book Now")
- Quick 3-step visual: 1) Check Availability → 2) Fill Form → 3) Submit Requirements
- Fleet preview cards (Economy / Standard / Sport / ADV / Premium) with rates
- Social proof strip (Facebook reviews widget or static testimonials)
- Sticky "Book Now" floating button on mobile

#### Available Units Page
- Month selector + date picker (same as current system, integrated)
- Unit grid with status badges (Vacant / Reserved / Rented / Extended / Returned)
- Click any unit to see details + direct link to reservation form pre-filled with that unit
- Legend + "As of" timestamp
- Mobile: cards stacked vertically; Desktop: grid layout

#### Rates Page
- Tier cards at top (Economy / Standard / Sport / ADV / Premium) with photos
- Destination filter dropdown + search
- Rate table from Google Sheets (live)
- Add-on pricing section (top box, delivery, camping chairs, driver)
- Note on motorwash/carwash charges

#### Reserve Now Page
- Same fields as current form (well-designed, keep them)
- Add: driver's license number field (for blacklist pre-check)
- Add: government ID type + number field
- Add: "I agree to terms" checkbox with link to requirements
- Progress bar (Step 1: Details → Step 2: Unit & Dates → Step 3: Submit)
- Real-time unit availability check before submission

#### Requirements Page
- What to bring: Driver's License, Government ID, Reservation Payment
- Rental Agreement overview (downloadable PDF)
- Document upload instructions (post-booking Google Form link)
- FAQ section (cancellation policy, deposit, fuel policy)

#### Contact Us Page
- Facebook Messenger link
- Email address
- Business hours
- Embedded Google Map (location pin)
- Instagram / TikTok links

### Mobile-First Layout Principles

- Minimum touch target: 44px
- Font size: minimum 16px on forms (prevents iOS zoom)
- Sticky nav bar collapses to hamburger on mobile
- Reservation form: single-column on mobile, two-column on tablet+
- Rates table: horizontal scroll on mobile, full-width on desktop
- "Book Now" sticky CTA visible at all times on mobile
- Color scheme: consistent with current branding (suggest keeping your existing palette — clean, professional)

### Customer Journey Flow

```
DISCOVER → CHECK AVAILABILITY → VIEW RATES → FILL RESERVATION FORM
    ↓
RECEIVE EMAIL (booking ID + requirements checklist)
    ↓
UPLOAD DOCUMENTS (Google Form with file upload)
    ↓
STAFF: BLACKLIST CHECK → VERIFY DOCS → APPROVE / REJECT
    ↓
CUSTOMER: CONFIRMATION EMAIL (pickup instructions + agreement link)
    ↓
PICKUP DAY → RETURN → COMPLETE
```

---

## 3. TECH STACK DECISION

### Frontend

| Layer | Choice | Reason |
|-------|--------|--------|
| HTML/CSS/JS | Vanilla + Alpine.js (CDN) | No build tools, GitHub Pages friendly, reactive |
| Styling | Tailwind CSS (CDN) | Utility-first, mobile-first, no config needed |
| Icons | Font Awesome or Heroicons (CDN) | Free, lightweight |
| Calendar | Flatpickr (CDN) | Lightweight date picker, no npm needed |
| Fonts | Google Fonts (Poppins or Inter) | Clean, modern, professional |

**Why Alpine.js over React/Vue:** No build step, no npm, no Node.js. Just a `<script>` tag. Works perfectly on GitHub Pages. Handles the reactivity you need (toggling tabs, form validation, API calls).

### Backend (Serverless via Google Apps Script)

| Layer | Choice | Reason |
|-------|--------|--------|
| API | Google Apps Script Web App | Free, integrates natively with Sheets, handles POST |
| Database | Google Sheets | Already using it, familiar, free |
| File Storage | Google Drive (via Forms) | Free, handles document uploads |
| Email | Gmail via Apps Script | Free, no SMTP setup needed |
| Auth (Admin) | Google OAuth via Apps Script | Staff login via Google account |

### Hosting

| Component | Host | Cost |
|-----------|------|------|
| Main website | GitHub Pages | Free |
| API | Google Apps Script | Free |
| Database | Google Sheets | Free |
| Document storage | Google Drive | Free (15GB) |
| Custom domain (optional) | Namecheap ~$10/yr | Optional |

**Total monthly cost: ₱0 to ₱50/month** (custom domain only if desired)

### What to Avoid at This Stage

- ❌ Firebase / Supabase — overkill, adds cost and complexity
- ❌ Next.js / React — requires build pipeline, not GitHub Pages friendly
- ❌ Node.js backend — requires paid hosting
- ❌ MySQL / PostgreSQL — unnecessary when Google Sheets covers your volume
- ❌ Stripe / payment gateway — handle payments manually (GCash/bank) for now

---

## 4. GOOGLE SHEETS DATABASE DESIGN

### Recommended Workbook Structure

One master Google Spreadsheet with multiple tabs:

#### Tab 1: RESERVATIONS
```
Booking ID | Timestamp | Status | First Name | Last Name | Email | Phone
License No | Gov ID Type | Gov ID No | Unit | Pickup Date | Return Date
Duration | Destination | Add-ons | Delivery Option | Payment Method
Total Estimate | Reservation Fee Paid | Blacklist Status | Verified By
Approved Date | Notes | Source | IP Address (optional)
```

**Status Values:** PENDING → DOCS_SUBMITTED → UNDER_REVIEW → APPROVED → CONFIRMED → ACTIVE → COMPLETED → CANCELLED → REJECTED → BLACKLISTED

#### Tab 2: UNITS
```
Unit ID | Unit Name | Category | Plate No | Year | Rate/Day | Status | Notes
Current Booking ID | Last Service Date | Insurance Expiry
```

#### Tab 3: BLACKLIST
```
Entry ID | Date Added | Full Name | Phone | Email | License No | Gov ID No
Reason | Added By | Risk Level (HIGH/MEDIUM/LOW) | Notes | Active (Y/N)
```

#### Tab 4: RATES
```
Destination | Area | Economy | Standard | Sport | ADV | Premium
(This is your existing rates sheet — keep as-is)
```

#### Tab 5: AVAILABILITY_CALENDAR
```
Date | Unit ID | Status | Booking ID | Notes
(Auto-populated by Apps Script from RESERVATIONS tab)
```

#### Tab 6: AUDIT_LOG
```
Timestamp | Action | Booking ID | Done By | Old Value | New Value | Notes
```

#### Tab 7: STAFF_QUEUE
```
Booking ID | Priority | Assigned To | Action Required | Due By | Completed
```

### Google Sheets Access Control

- Main spreadsheet: Owner = you (twowheelventurescdo@gmail.com)
- Staff: Share with edit access (only trusted emails)
- Apps Script: Runs as owner — never expose spreadsheet ID publicly
- Blacklist sheet: Restrict to named range or separate sheet with column-level protection

---

## 5. RESERVATION SYSTEM LOGIC

### How Booking Form Submissions Work

```
Customer fills form → JS validates client-side → 
POST to Apps Script Web App URL → 
Apps Script:
  1. Generates Booking ID (e.g., TWV-2026-00142)
  2. Runs blacklist check (see Section 6)
  3. If BLACKLISTED → log + notify staff, return rejection
  4. If CLEAR → write to RESERVATIONS sheet (Status: PENDING)
  5. Check for date/unit conflicts
  6. If conflict → return "unit unavailable" error
  7. If available → hold unit for 48 hours (pending expiry timer)
  8. Send confirmation email to customer
  9. Send notification email to staff
  10. Return success response to frontend
```

### How Data Gets Stored

All reservation data is written by Apps Script (never directly from the browser to Sheets). The browser only talks to the Apps Script web app endpoint. The Sheets URL and structure are never exposed to the public.

### Booking Statuses Explained

| Status | Meaning | Who Sets It |
|--------|---------|-------------|
| PENDING | Form submitted, awaiting docs | Auto (system) |
| DOCS_SUBMITTED | Customer uploaded documents | Auto (Google Form trigger) |
| UNDER_REVIEW | Staff is verifying | Staff |
| APPROVED | Docs verified, awaiting final payment | Staff |
| CONFIRMED | Payment received, booking locked in | Staff |
| ACTIVE | Unit is out with customer | Staff |
| COMPLETED | Unit returned, no issues | Staff |
| CANCELLED | Customer or staff cancelled | Staff or Auto (expiry) |
| REJECTED | Failed verification | Staff |
| BLACKLISTED | Customer is on blacklist | Auto (system) |
| EXPIRED | Customer didn't submit docs in time | Auto (timer) |

### Preventing Double Bookings

Apps Script checks for conflicts on EVERY submission:

```javascript
function checkConflict(unitId, pickupDate, returnDate) {
  // Query RESERVATIONS sheet
  // Filter by unitId where Status NOT IN ['CANCELLED','REJECTED','EXPIRED','BLACKLISTED']
  // Check if requested date range overlaps any existing booking
  // Overlap condition: pickup < existingReturn AND return > existingPickup
  // Return: { conflict: true/false, conflictingBookingId: "..." }
}
```

**Rule:** Pending bookings hold the unit for **48 hours**. After 48 hours without document submission, the status auto-changes to EXPIRED and the unit is released.

### Handling Overlapping Dates

If a customer requests dates that overlap an existing active/confirmed booking:
- Return an error immediately: "This unit is unavailable for your selected dates."
- Display alternative available units for the same period
- Do NOT allow the booking to proceed

### Pending Reservation Expiry

Apps Script time-driven trigger runs every 6 hours:

```javascript
function expirePendingReservations() {
  // Get all PENDING reservations older than 48 hours
  // Change status to EXPIRED
  // Release unit in AVAILABILITY_CALENDAR
  // Send "Your booking has expired" email to customer
  // Log in AUDIT_LOG
}
```

Set via Apps Script → Triggers → Time-driven → Every 6 hours.

---

## 6. BLACKLIST VERIFICATION SYSTEM

### This Is Your Most Critical Security Feature

The blacklist check runs **automatically on every form submission** before any booking is created. It is not optional and cannot be skipped.

### What Gets Checked

| Field | Check Type | Logic |
|-------|-----------|-------|
| Full Name | Fuzzy match | Normalize (lowercase, trim) + check for similar strings |
| Phone Number | Exact match | Normalize format (remove +63, spaces, dashes) |
| Email | Exact match | Lowercase comparison |
| License Number | Exact match | Uppercase, strip spaces |
| Gov ID Number | Exact match | Uppercase, strip spaces |

### Blacklist Check Algorithm

```javascript
function checkBlacklist(formData) {
  const blacklist = getBlacklistSheet().getDataRange().getValues();
  
  // Normalize inputs
  const name = normalizeName(formData.firstName + ' ' + formData.lastName);
  const phone = normalizePhone(formData.phone);
  const email = formData.email.toLowerCase().trim();
  const license = formData.licenseNo.toUpperCase().trim();
  const govId = formData.govIdNo.toUpperCase().trim();
  
  let flags = [];
  
  for (let row of blacklist) {
    if (!row[ACTIVE]) continue; // Skip inactive entries
    
    // Exact matches
    if (normalizePhone(row[PHONE]) === phone) flags.push({ field: 'phone', entry: row });
    if (row[EMAIL].toLowerCase() === email) flags.push({ field: 'email', entry: row });
    if (row[LICENSE].toUpperCase() === license) flags.push({ field: 'license', entry: row });
    if (row[GOV_ID].toUpperCase() === govId) flags.push({ field: 'govId', entry: row });
    
    // Fuzzy name match (similarity score)
    if (nameSimilarity(normalizeName(row[NAME]), name) > 0.85) {
      flags.push({ field: 'name', entry: row, score: similarity });
    }
  }
  
  return {
    isBlacklisted: flags.length > 0,
    flags: flags,
    riskLevel: determineRisk(flags)
  };
}
```

### Risk Level Logic

| Risk | Condition | Action |
|------|-----------|--------|
| HIGH | License OR Phone OR ID match | Auto-reject + notify staff immediately |
| MEDIUM | Email match OR name similarity > 90% | Flag for manual review + notify staff |
| LOW | Name similarity 85–90% | Flag in system + staff reviews manually |
| CLEAR | No matches | Proceed normally |

### What Happens on Each Risk Level

**HIGH RISK (Auto-Reject):**
- Booking is logged with status BLACKLISTED
- Customer receives generic "We cannot process your booking at this time" email (no blacklist mention)
- Staff receives immediate email/SMS: "⚠️ BLACKLIST HIT — [Name] — [Booking ID] — Match: License"
- Entry is added to AUDIT_LOG

**MEDIUM RISK (Flag + Manual Review):**
- Booking is created with status FLAGGED
- Staff receives notification: "🔴 POSSIBLE BLACKLIST MATCH — Review Required"
- Staff queue shows the booking with flag details
- Staff approves or rejects manually within 24 hours

**LOW RISK (Soft Flag):**
- Booking proceeds to PENDING status
- Staff queue shows a yellow warning
- Staff reviews alongside normal document verification

**CLEAR:**
- Booking proceeds normally to PENDING

### Why You Should NOT Tell the Customer They're Blacklisted

This is important for safety: blacklisted customers who know they're detected may create new identities or become aggressive. The generic rejection protects you and your staff.

### Blacklist Management (Staff Side)

Staff can add new entries to the blacklist via:
- A protected Google Form linked only in the staff admin panel
- Or direct entry in the Blacklist sheet tab (restricted to editors)

Each entry must include: Name, Phone, Email, License, Reason, Risk Level.

---

## 7. ADMIN / STAFF SIDE

### Option A: Google Sheets as Staff Dashboard (MVP — Recommended First)

Your Google Sheets is already a functional admin tool. Enhance it:

- Use conditional formatting to color-code statuses (PENDING = yellow, APPROVED = green, FLAGGED = red)
- Use data validation dropdowns for status changes
- Add a STAFF_NOTES column for remarks
- Add a filter view per status for easy triage
- Protect the BLACKLIST and AUDIT_LOG tabs so only owners can edit

**Pros:** Zero development time, works immediately, familiar interface  
**Cons:** Not a real dashboard, requires Sheets access, not customer-friendly

### Option B: Simple HTML Admin Panel on GitHub Pages (Phase 2)

A separate `/admin/index.html` page protected by a Google OAuth login:

**Features:**
- Booking queue with filters (Pending / Flagged / Under Review / Approved)
- Blacklist check results displayed per booking
- Approve / Reject buttons (POST to Apps Script)
- Document viewer (links to Google Drive uploads)
- Status change with audit log
- Quick stats: bookings this week, revenue estimate, units out

**Access:** Staff logs in via Google account. Apps Script checks if their email is in an approved staff list before allowing any actions.

### Pending Verification Queue

The most important staff workflow:

```
QUEUE VIEW (sorted by priority):
1. 🔴 FLAGGED bookings (blacklist hits) — review immediately
2. 🟡 DOCS_SUBMITTED — verify documents
3. ⚪ PENDING — awaiting documents (show time remaining until expiry)
4. 🟢 APPROVED — awaiting confirmation actions
```

Each queue item shows: Booking ID, Customer Name, Unit, Dates, Blacklist Status, Time in Queue, Action buttons.

### Approval / Rejection Workflow

**Approve:**
1. Staff clicks Approve in admin panel or updates status in Sheets
2. Apps Script sends customer confirmation email with:
   - Booking confirmed
   - Pickup instructions
   - Rental agreement PDF link
   - Payment completion instructions
3. AUDIT_LOG records: who approved, when, what booking

**Reject:**
1. Staff selects rejection reason from dropdown
2. Apps Script sends customer email: "We were unable to confirm your booking" (no specific reason for fraud prevention)
3. If rejection is fraud-related, staff adds customer to BLACKLIST

### Audit Log

Every action is logged automatically:

```
2026-05-15 14:32:01 | STATUS_CHANGE | TWV-2026-00142 | joyce@twv.com | PENDING → APPROVED | "Docs verified, DL matches"
2026-05-15 14:35:12 | BLACKLIST_CHECK | TWV-2026-00143 | SYSTEM | CLEAR | "No matches found"
2026-05-15 15:01:00 | EXPIRY | TWV-2026-00140 | SYSTEM | PENDING → EXPIRED | "48hr timeout"
```

---

## 8. GOOGLE SHEETS INTEGRATION & APPS SCRIPT

### Apps Script Web App Architecture

Your Apps Script acts as a mini-REST API with two endpoints:

```javascript
// GET — for frontend data reads (availability, rates)
function doGet(e) {
  const action = e.parameter.action;
  switch(action) {
    case 'availability': return getAvailability(e.parameter);
    case 'rates': return getRates(e.parameter);
    case 'units': return getUnits();
    default: return error('Unknown action');
  }
}

// POST — for form submissions and admin actions
function doPost(e) {
  const data = JSON.parse(e.postData.contents);
  switch(data.action) {
    case 'submitReservation': return submitReservation(data);
    case 'updateStatus': return updateStatus(data); // admin only
    case 'addBlacklist': return addToBlacklist(data); // admin only
    default: return error('Unknown action');
  }
}
```

### Syncing Live Availability

```javascript
function getAvailability(params) {
  // Read RESERVATIONS sheet
  // Filter by unit (if specified) and active statuses
  // Return array of { unitId, date, status, bookingId }
  // Frontend renders calendar from this data
}
```

### Syncing Reservations

All reservation writes go through `submitReservation()`. Never allow direct Sheet writes from the browser.

### API Limitations to Know

| Limitation | Detail | Mitigation |
|-----------|--------|-----------|
| Rate limit | 20,000 read/writes per day (free Google account) | More than enough for your volume |
| Execution time | 6 minutes max per script run | Fine for your use case |
| Concurrent users | Scripts queue if hammered | Add a simple cache for GET requests |
| CORS | Apps Script handles this natively | No issue |
| Cold start | First request may take 2–3 seconds | Show loading spinner on frontend |

### GET Request Caching (Important)

Cache availability and rates data in Google Apps Script's CacheService:

```javascript
function getCachedAvailability() {
  const cache = CacheService.getScriptCache();
  let data = cache.get('availability');
  if (!data) {
    data = JSON.stringify(fetchAvailabilityFromSheets());
    cache.put('availability', data, 300); // Cache for 5 minutes
  }
  return JSON.parse(data);
}
```

This prevents hammering the Sheets API and speeds up page loads significantly.

---

## 9. AUTOMATION PLAN

### Email Automations via Apps Script

| Trigger | Email Sent To | Content |
|---------|--------------|---------|
| Reservation submitted | Customer | Booking ID, requirements checklist, doc upload link, 48hr warning |
| Reservation submitted | Staff | New booking alert, blacklist status, review link |
| Docs submitted | Customer | "Docs received, under review" |
| Docs submitted | Staff | Queue notification |
| Booking approved | Customer | Confirmation, pickup instructions, agreement PDF |
| Booking rejected | Customer | Generic rejection (no specific reason) |
| Booking expired (48hr) | Customer | "Your booking has expired — rebook?" |
| Day before pickup | Customer | Reminder: bring license + ID + payment |
| Day of return | Customer | Return reminder: time, location, motorwash fee |
| Unit 2 days overdue | Staff | Override alert: customer has not returned unit |
| Blacklist hit | Staff | Immediate alert with match details |

### Automated Timers (Apps Script Time Triggers)

| Trigger | Frequency | Action |
|---------|-----------|--------|
| Expiry check | Every 6 hours | Cancel PENDING bookings older than 48hrs |
| Pickup reminder | Daily 8AM | Email customers with pickup tomorrow |
| Return reminder | Daily 8AM | Email customers with return today |
| Overdue check | Daily 9AM | Flag units not returned on time |
| Rate sync | Weekly | Update rates cache |

### Document Upload Flow

Use a Google Form with file upload enabled:

1. After booking confirmed email, customer receives link: "Upload your documents here: [Google Form URL]"
2. Google Form collects: Booking ID, Driver's License photo, Government ID photo, Payment proof
3. Apps Script trigger on Form submission:
   - Matches Booking ID to reservation
   - Updates status to DOCS_SUBMITTED
   - Moves files to a structured Drive folder: `/TWV Docs/[Year]/[BookingID]/`
   - Notifies staff
4. Staff views docs via Drive link in admin panel

This keeps document uploads off GitHub Pages (which can't handle file uploads) and in secure Google Drive.

---

## 10. SECURITY & RISK MANAGEMENT

### Protecting Google Sheets

- Never expose your Spreadsheet ID in frontend code
- All data access goes through Apps Script (acts as a proxy)
- Protect sensitive tabs (Blacklist, AuditLog) with sheet-level passwords
- Use Apps Script to validate every incoming request

### Spam / Abuse Prevention

| Threat | Mitigation |
|--------|-----------|
| Spam form submissions | Google reCAPTCHA v3 on reservation form (free) |
| Fake bookings | Require email verification (send confirmation link before booking is activated) |
| Rate limiting | Apps Script checks submission frequency by IP/email |
| Brute force | No password-protected pages on GitHub Pages to attack |
| Social engineering | Staff policy: never give status info over Messenger without Booking ID |

### Preventing Unauthorized Edits

- Apps Script `doPost()` for status changes requires a staff_token parameter
- Staff token = a secret string known only to staff, rotated monthly
- OR use Google OAuth: check `Session.getActiveUser().getEmail()` against approved staff list

### Protecting Customer Data

- Reservation data stored only in Google Sheets (your Google account)
- Document photos stored in private Google Drive folder (not publicly accessible)
- Never display full license/ID numbers in the public-facing admin panel (mask to last 4 digits)
- Add Google Sheets activity monitoring: turn on notification rules for any changes

### Backup Strategy

- Google Sheets has built-in version history (90 days) — use it
- Monthly: manually export RESERVATIONS sheet as CSV and save to Drive archive folder
- Apps Script: export to a backup spreadsheet weekly via trigger

---

## 11. DEVELOPMENT ROADMAP

### Phase 0 — Prep (1 week, no coding)

- [ ] Consolidate all existing Sheets into one master workbook with proper tab structure
- [ ] Design your BLACKLIST sheet properly (add all known bad actors)
- [ ] Set up a dedicated Google Account for TWV CDO operations (separate from personal)
- [ ] Register a custom domain (optional but professional): e.g., `twvcdo.com`
- [ ] Create a single GitHub repository: `twowheelventurescdo/platform`
- [ ] Sketch your brand color palette and gather unit photos

### Phase 1 — MVP (2–3 weeks, core functionality)

**Goal:** One unified website that replaces all three separate pages

- [ ] Static site shell: navigation, all 6 pages, mobile-first layout
- [ ] Port existing Vacant/Rates pages into the new site (same functionality, new design)
- [ ] Port existing Reservation Form (add license/ID fields)
- [ ] Apps Script: `/submitReservation` endpoint with blacklist check
- [ ] Apps Script: basic confirmation emails (customer + staff)
- [ ] Blacklist verification (auto-check on submission)
- [ ] Deploy to GitHub Pages

**Deliverable:** Live site at `twowheelventurescdo.github.io/platform` (or custom domain)

### Phase 2 — Verification System (2–3 weeks)

**Goal:** Full document submission and staff review workflow

- [ ] Google Form for document uploads (Drive integration)
- [ ] Apps Script: status updates on doc submission
- [ ] Email automation: all templates from Section 9
- [ ] Staff admin panel (HTML page, Google OAuth protected)
- [ ] Approval/rejection workflow with audit log
- [ ] Time-driven triggers: expiry, reminders

**Deliverable:** Staff can manage entire booking lifecycle from admin panel

### Phase 3 — Operations Polish (2–4 weeks)

**Goal:** Reduce manual work further

- [ ] Availability calendar with real-time unit blocking
- [ ] Automated rental agreement (pre-filled PDF via Apps Script)
- [ ] Return tracking (staff marks units as returned)
- [ ] Blacklist management interface (add/deactivate entries)
- [ ] Analytics dashboard (bookings per week, revenue, popular units)
- [ ] Mobile-optimized staff panel

### Phase 4 — Future Scalability (when volume justifies)

**Delay these until you have 50+ bookings/month:**

- [ ] GCash payment integration (via Dragonpay or Xendit API — ~₱5,000 setup)
- [ ] SMS notifications (Semaphore API — ~₱500/month)
- [ ] Online rental agreement signing (DocuSign or free alternative)
- [ ] Customer portal (track booking status without Messenger)
- [ ] Multiple branch support
- [ ] Dynamic pricing based on demand

---

## 12. CODEBASE & FOLDER STRUCTURE

### GitHub Repository Structure

```
twowheelventurescdo/platform/
│
├── index.html                    ← Main SPA entry point
├── 404.html                      ← Custom 404 page
│
├── /pages/
│   ├── home.html                 ← Home section partial
│   ├── available.html            ← Available units
│   ├── rates.html                ← Rates page
│   ├── reserve.html              ← Reservation form
│   ├── requirements.html        ← Booking requirements
│   └── contact.html             ← Contact page
│
├── /admin/
│   └── index.html               ← Staff admin panel (Google OAuth gated)
│
├── /assets/
│   ├── /css/
│   │   ├── main.css             ← Core styles + Tailwind config
│   │   └── admin.css            ← Admin panel styles
│   ├── /js/
│   │   ├── app.js               ← Main app logic (Alpine.js)
│   │   ├── api.js               ← All Apps Script API calls
│   │   ├── calendar.js          ← Availability calendar logic
│   │   ├── form.js              ← Reservation form logic + validation
│   │   └── admin.js             ← Admin panel logic
│   └── /images/
│       ├── /units/              ← Unit photos (Beat, Click, PCX, etc.)
│       ├── logo.png
│       └── hero-bg.jpg
│
├── /apps-script/                ← Version-controlled Apps Script code
│   ├── Code.gs                  ← Main router (doGet, doPost)
│   ├── Reservations.gs          ← Booking logic
│   ├── Blacklist.gs             ← Blacklist check + fuzzy match
│   ├── Availability.gs          ← Calendar + conflict detection
│   ├── Email.gs                 ← All email templates
│   ├── Admin.gs                 ← Staff-side actions
│   ├── Automation.gs            ← Time-trigger automations
│   └── Utils.gs                 ← Helpers (normalize, format, ID gen)
│
└── README.md
```

### Apps Script Structure

```javascript
// Code.gs — main router
function doGet(e) { ... }
function doPost(e) { ... }

// Blacklist.gs
function checkBlacklist(data) { ... }
function addToBlacklist(data) { ... }
function normalizeName(str) { ... }
function normalizePhone(str) { ... }
function nameSimilarity(a, b) { ... }

// Reservations.gs
function submitReservation(data) { ... }
function generateBookingId() { ... }
function checkConflict(unit, start, end) { ... }
function updateStatus(bookingId, newStatus, staffEmail) { ... }

// Email.gs
function sendCustomerConfirmation(booking) { ... }
function sendStaffAlert(booking, blacklistResult) { ... }
function sendExpiryNotice(booking) { ... }
function sendApprovalEmail(booking) { ... }
function sendReminderEmail(booking, type) { ... }
```

---

## 13. SYSTEM ARCHITECTURE DIAGRAM

```
┌─────────────────────────────────────────────────────────────────┐
│                    CUSTOMER (Browser)                           │
│              GitHub Pages: twvcdo.github.io/platform            │
│                                                                 │
│  [Home] [Available] [Rates] [Reserve] [Requirements] [Contact]  │
└───────────────────────────┬─────────────────────────────────────┘
                            │ HTTPS POST/GET
                            ▼
┌───────────────────────────────────────────────────────────────┐
│              GOOGLE APPS SCRIPT WEB APP (API)                  │
│           script.google.com/macros/s/[ID]/exec                │
│                                                               │
│  doGet()                          doPost()                    │
│  ├── ?action=availability         ├── submitReservation       │
│  ├── ?action=rates                ├── updateStatus (admin)    │
│  └── ?action=units                └── addBlacklist (admin)   │
│                                                               │
│  ┌─────────────────┐  ┌─────────────────────────────────────┐ │
│  │  checkBlacklist │  │  generateBookingId + conflictCheck  │ │
│  │  normalizeName  │  │  writeToReservations               │ │
│  │  nameSimilarity │  │  updateAvailability                │ │
│  └────────┬────────┘  └─────────────────────────────────────┘ │
└───────────┼───────────────────────────────────────────────────┘
            │ Read/Write
            ▼
┌─────────────────────────────────────────────────────────────┐
│              GOOGLE SHEETS (Master Workbook)                 │
│                                                             │
│  📋 RESERVATIONS    🚫 BLACKLIST     🛵 UNITS              │
│  💰 RATES           📅 AVAILABILITY  📝 AUDIT_LOG          │
│  👥 STAFF_QUEUE                                            │
└─────────────────────────────────────────────────────────────┘
            │                      │
            │ Email triggers        │ File storage
            ▼                      ▼
┌─────────────────┐    ┌──────────────────────────┐
│   GMAIL API     │    │   GOOGLE DRIVE           │
│ (via Apps Script│    │  /TWV Docs/[Year]/[ID]/   │
│  GmailApp)      │    │  - DL photo              │
│                 │    │  - Gov ID photo           │
└─────────────────┘    │  - Payment proof         │
                       └──────────────────────────┘
                                   ▲
                            Google Form
                         (Document Upload)
                                   │
                            Customer uploads
                            after booking

┌─────────────────────────────────────────────────────────────┐
│              STAFF ADMIN PANEL                               │
│         /admin/index.html (GitHub Pages)                    │
│         Protected by: Apps Script staff token check         │
│                                                             │
│  [Booking Queue] [Flagged] [Approve/Reject] [Blacklist]    │
│  [Audit Log]    [Analytics] [Unit Status]                  │
└─────────────────────────────────────────────────────────────┘
```

---

## 14. KEY WORKFLOWS

### Workflow A: Standard Clean Booking

```
1. Customer visits platform → checks availability → views rates
2. Fills reservation form (name, contact, unit, dates, license #, Gov ID #)
3. Submits → reCAPTCHA passes → POST to Apps Script
4. Apps Script:
   a. Normalizes all fields
   b. Checks blacklist → CLEAR ✅
   c. Checks date conflicts → AVAILABLE ✅
   d. Generates Booking ID: TWV-2026-00142
   e. Writes row to RESERVATIONS (status: PENDING)
   f. Sends customer email: "Submit documents within 48 hours"
   g. Sends staff email: "New booking — TWV-2026-00142 — CLEAR ✅"
5. Customer uploads docs via Google Form
6. Trigger fires: status → DOCS_SUBMITTED, staff notified
7. Staff verifies docs in 24 hours → clicks Approve
8. Customer receives: "Your booking is CONFIRMED — pickup instructions"
9. Unit goes ACTIVE on pickup day → COMPLETED on return
```

### Workflow B: Blacklisted Customer (HIGH RISK)

```
1. Customer submits form
2. Apps Script runs blacklist check
3. LICENSE NUMBER matches existing blacklist entry (risk: HIGH)
4. Apps Script:
   a. Logs booking with status: BLACKLISTED
   b. Does NOT confirm or hold any unit
   c. Sends customer: "We're unable to process your booking at this time."
   d. Sends staff: "⚠️ BLACKLIST HIT — Phone + License match — [Name] — review: [link]"
5. Staff reviews in admin panel, optionally adds additional details to blacklist
6. No unit is ever released to this customer
```

### Workflow C: Flagged Customer (MEDIUM RISK)

```
1. Customer submits form
2. Apps Script: email similarity to blacklist entry (85%) → risk: MEDIUM
3. Booking created with status: FLAGGED (unit held for 24hrs only)
4. Staff notification: "🔴 POSSIBLE MATCH — Manual review required"
5. Staff compares name, reviews in queue
6. If confirmed same person → REJECT + add to blacklist
7. If different person → mark as CLEAR, proceeds normally
```

### Workflow D: Booking Expiry

```
1. Customer submits form → PENDING status → 48hr timer starts
2. Customer does NOT upload documents within 48 hours
3. Apps Script time trigger (every 6hrs) detects expired booking
4. Status changes: PENDING → EXPIRED
5. Unit is released back to VACANT
6. Customer email: "Your reservation has expired. You may rebook when ready."
7. Logged in AUDIT_LOG
```

---

## 15. TECH STACK SUMMARY

| Component | Technology | Why |
|-----------|-----------|-----|
| Frontend | HTML + Alpine.js + Tailwind CSS | No build tools, GitHub Pages compatible |
| Hosting | GitHub Pages | Free, reliable, custom domain ready |
| API / Backend | Google Apps Script Web App | Free, native Sheets/Gmail integration |
| Database | Google Sheets | Already in use, sufficient for your volume |
| Document Upload | Google Forms + Google Drive | Free file storage, familiar to customers |
| Email | Gmail via GmailApp (Apps Script) | Free, no SMTP config |
| Security | reCAPTCHA v3 + staff tokens + Sheet protection | Lightweight but effective |
| Date Picker | Flatpickr.js (CDN) | Lightweight, mobile-friendly |
| Admin Auth | Google OAuth (Apps Script) | Staff use Google accounts |
| Version Control | GitHub | Free, enables GitHub Pages |
| Custom Domain | Namecheap or Cloudflare (~₱500/yr) | Optional but professional |

**Estimated Monthly Cost: ₱0** (₱500/year if using custom domain)

---

## 16. SAMPLE BOOKING LIFECYCLE

```
DATE        TIME     EVENT                                STATUS
──────────────────────────────────────────────────────────────────
May 15      10:32    Customer fills form, submits         ——
May 15      10:32    reCAPTCHA passes                     ——
May 15      10:32    Blacklist check: CLEAR               ——
May 15      10:32    Conflict check: AVAILABLE            ——
May 15      10:32    Booking ID generated: TWV-2026-00142 PENDING
May 15      10:32    Confirmation email sent to customer  PENDING
May 15      10:32    Staff alert email sent               PENDING
May 15      11:15    Customer uploads docs via Form       DOCS_SUBMITTED
May 15      11:15    Staff notified: docs ready           DOCS_SUBMITTED
May 15      14:30    Staff verifies: DL ✅ Gov ID ✅     UNDER_REVIEW
May 15      14:35    Staff clicks Approve                 APPROVED
May 15      14:35    Customer receives confirmation email CONFIRMED
May 17      09:00    Pickup reminder email sent           CONFIRMED
May 18      09:00    Customer picks up scooter            ACTIVE
May 18      09:05    Staff marks unit as ACTIVE           ACTIVE
May 20      09:00    Return reminder email sent           ACTIVE
May 20      17:30    Customer returns unit                ——
May 20      17:35    Staff marks unit: COMPLETED          COMPLETED
```

---

## 17. SAMPLE BLACKLIST VERIFICATION FLOW

```
INPUT (from reservation form):
  Name:    "Juan dela Cruz"
  Phone:   "09171234567"
  Email:   "juandc@gmail.com"
  License: "N12-34-567890"
  Gov ID:  "1234-5678-9012"

STEP 1: Normalize inputs
  name  → "juan dela cruz"
  phone → "09171234567"
  email → "juandc@gmail.com"
  lic   → "N1234567890"
  id    → "123456789012"

STEP 2: Iterate BLACKLIST sheet (15 entries)

  Row 3: "juan delacruz" | 09171234567 | juandc1@gmail.com | N1234567890
    → Phone match ✅ (EXACT) → Risk: HIGH
    → License match ✅ (EXACT) → Risk: HIGH
    → Name similarity: 94% → Risk: HIGH

STEP 3: Risk assessment
  → Multiple HIGH matches → Risk Level: HIGH
  → Auto-reject triggered

STEP 4: Actions
  → Booking logged: status = BLACKLISTED, booking ID = TWV-2026-00143
  → Customer email: "We're unable to process your booking at this time."
  → Staff alert:
    Subject: ⚠️ BLACKLIST HIT — TWV-2026-00143
    Body: "Juan dela Cruz (09171234567) matched Row 3 of blacklist.
           Match fields: Phone (exact), License (exact), Name (94%).
           Action: Booking auto-rejected. No unit held.
           Review: [admin panel link]"
  → AUDIT_LOG: "BLACKLIST_HIT | TWV-2026-00143 | SYSTEM | HIGH | Phone+License+Name"

RESULT: Customer rejected. No unit allocated. Staff informed. Transaction logged.
```

---

## 18. MVP BUILD ORDER

Build in this exact sequence. Each step is independently deployable.

```
WEEK 1: Foundation
──────────────────
[ ] 1. Set up GitHub repo: twowheelventurescdo/platform
[ ] 2. Create index.html with full navigation (6 tabs, mobile-responsive)
[ ] 3. Port Available Units page into new design (same Google Sheets data source)
[ ] 4. Port Rates page into new design (same data source)
[ ] 5. Static Home page (hero, fleet cards, CTA buttons)
[ ] 6. Static Requirements page (doc list, FAQ)
[ ] 7. Static Contact page
[ ] 8. Deploy to GitHub Pages — verify all pages load

WEEK 2: Reservation Form + Blacklist Core
──────────────────────────────────────────
[ ] 9.  Port Reservation Form with added fields (license #, Gov ID)
[ ] 10. Set up master Google Spreadsheet (all tabs)
[ ] 11. Populate BLACKLIST sheet with known bad actors
[ ] 12. Build Apps Script: doPost, submitReservation, generateBookingId
[ ] 13. Build Apps Script: checkBlacklist (exact match first, fuzzy later)
[ ] 14. Build Apps Script: checkConflict (date overlap detection)
[ ] 15. Build Apps Script: confirmation emails (customer + staff)
[ ] 16. Test end-to-end: submit form → Sheets → email received

WEEK 3: Verification Workflow
──────────────────────────────
[ ] 17. Set up Google Form for document uploads (DL, Gov ID, payment proof)
[ ] 18. Apps Script: trigger on form submit → update status → notify staff
[ ] 19. Build time-driven trigger: expiry check every 6 hours
[ ] 20. Build email templates: expiry notice, reminder, approval, rejection
[ ] 21. Staff workflow: train staff to use Sheets queue + email notifications
[ ] 22. Soft launch: announce new booking process to Facebook followers

POST-LAUNCH (Week 4+)
──────────────────────
[ ] 23. Add reCAPTCHA v3 to reservation form
[ ] 24. Build simple admin HTML panel (read-only first, then approve/reject)
[ ] 25. Add name similarity (fuzzy) to blacklist check
[ ] 26. Add Google OAuth to admin panel
[ ] 27. Analytics dashboard in Sheets (pivot tables, weekly summary)
[ ] 28. Custom domain (optional)
```

---

## CRITICAL DECISIONS SUMMARY

| Decision | Recommendation | Reason |
|----------|---------------|--------|
| Framework | Vanilla JS + Alpine.js | No build pipeline, GitHub Pages works |
| Backend | Google Apps Script only | Free, already integrated, sufficient |
| Database | Google Sheets | Already in use, scale is appropriate |
| Blacklist method | Server-side Apps Script | Never expose blacklist to browser |
| Document uploads | Google Forms + Drive | Free, secure, familiar |
| Admin panel | Start with Sheets, build HTML panel in Phase 2 | Ship faster |
| Payment | Manual (GCash/bank) | Avoid payment gateway complexity for now |
| Auth | Staff token + Google OAuth | Simple, no password DB needed |
| Notifications | Email first, SMS later | Email is free; SMS costs money |
| Custom domain | Optional but recommended | Looks more professional to customers |

---

*Document prepared by Claude (Anthropic) for Two Wheel Ventures CDO operations review.*  
*Architecture designed for GitHub Pages + Google Apps Script + Google Sheets stack.*  
*Optimized for: small rental business, Philippines market, low-cost operations, fraud prevention.*
