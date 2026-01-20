# TECHNICAL SPECIFICATION DOCUMENT
## Shopify Booking & Smart Lock Integration System

**Project:** Room Booking System with Smart Lock Integration  
**Theme:** Shopify Dawn (Free Theme)  
**Budget:** $1,000 USD  
**Timeline:** 3-4.5 weeks  

---

## TABLE OF CONTENTS

1. [System Architecture](#system-architecture)
2. [Technical Stack](#technical-stack)
3. [Module Specifications](#module-specifications)
4. [Integration Points](#integration-points)
5. [Data Flow Diagrams](#data-flow-diagrams)
6. [API Specifications](#api-specifications)
7. [Implementation Steps](#implementation-steps)
8. [Testing Requirements](#testing-requirements)
9. [Risk Assessment](#risk-assessment)

---

## 1. SYSTEM ARCHITECTURE

### 1.1 High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    SHOPIFY STOREFRONT                        │
│  (Dawn Theme)                                                │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐     │
│  │   Products   │  │    Pages     │  │   Checkout   │     │
│  │  (Bundles)   │  │  (Booking)   │  │  (Stripe)    │     │
│  └──────────────┘  └──────────────┘  └──────────────┘     │
└─────────────────────────────────────────────────────────────┘
                          │
                          │ Webhooks/API
                          ▼
┌─────────────────────────────────────────────────────────────┐
│                  MAKE.COM AUTOMATION PLATFORM                │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  Scenario 1: Bundle Purchase → Credit Issuance     │   │
│  │  Scenario 2: Booking → Lock Code Generation         │   │
│  │  Scenario 3: Reschedule/Cancel → Code Management    │   │
│  │  Scenario 4: CRM Sync & Tagging                     │   │
│  │  Scenario 5: Email Notifications                   │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
         │              │              │              │
         │              │              │              │
         ▼              ▼              ▼              ▼
┌──────────────┐ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐
│ SimplyBook.me│ │ IglooHome API│ │  Zoho CRM    │ │Shopify Email │
│   (Booking)  │ │  (Smart Lock)│ │  (Customer)  │ │  (Notifications)│
└──────────────┘ └──────────────┘ └──────────────┘ └──────────────┘
```

### 1.2 Component Overview

- **Frontend:** Shopify Dawn theme (no custom code)
- **Booking Engine:** SimplyBook.me (embedded widget)
- **Automation:** Make.com workflows
- **Smart Locks:** IglooHome API
- **CRM:** Zoho (or similar)
- **Payments:** Shopify Payments / Stripe
- **Notifications:** Email via Shopify Email Service

---

## 2. TECHNICAL STACK

### 2.1 Core Platforms

| Component | Technology | Purpose |
|-----------|-----------|---------|
| **E-commerce** | Shopify (Dawn Theme) | Storefront, products, checkout |
| **Booking** | SimplyBook.me | Room/space booking calendar |
| **Automation** | Make.com | Workflow orchestration |
| **Smart Locks** | IglooHome API | Access code generation |
| **CRM** | Zoho CRM | Customer management |
| **Notifications** | Shopify Email Service | Email notifications |
| **Payments** | Stripe (via Shopify) | Payment processing |

### 2.2 Required Accounts & Access

- Shopify store (admin access)
- SimplyBook.me account (admin access)
- Make.com account (paid plan recommended)
- IglooHome account (API access)
- Zoho CRM account (or alternative)
- Shopify Email Service (included with Shopify)
- Stripe account (via Shopify)

---

## 3. MODULE SPECIFICATIONS

### 3.1 MODULE 1: Shopify Theme Setup (Dawn)

#### 3.1.1 Pages Required

| Page | Template | Content Type | Notes |
|------|----------|--------------|-------|
| Home | `index` | Sections | Hero, services overview, CTA |
| Services | `page.services` | Custom sections | Service listings |
| Rooms / Spaces | `page.rooms` | Embed + Content | SimplyBook.me widget |
| Bundles / Packages | `collection.bundles` | Product grid | Bundle products |
| Pricing | `page.pricing` | Content | Pricing tables |
| Book Now | `page.booking` | Embed | SimplyBook.me widget |
| How It Works | `page.how-it-works` | Content | Step-by-step guide |
| FAQ | `page.faq` | Accordion | FAQ sections |
| Contact | `page.contact` | Form | Contact form |
| Policies | `page.policies` | Content | Policy links |
| Privacy Policy | `page.privacy` | Standard | Shopify template |
| Terms of Service | `page.terms` | Standard | Shopify template |
| Refund / Credit Policy | `page.refund` | Content | Credits-only policy |

#### 3.1.2 Navigation Structure

```
Main Menu:
├── Home
├── Services
│   ├── Rooms / Spaces
│   ├── Bundles / Packages
│   ├── Coaching
│   └── Add-Ons
├── Pricing
├── Book Now
├── How It Works
└── Contact

Footer:
├── Policies
│   ├── Privacy Policy
│   ├── Terms of Service
│   └── Refund Policy
└── FAQ
```

#### 3.1.3 Theme Customization (Minimal)

- Logo upload
- Color scheme (if needed)
- Font adjustments (if needed)
- Custom sections for service pages
- Embed blocks for SimplyBook.me widgets

**Implementation Method:**
- Use Dawn's built-in theme customizer
- Custom sections via Shopify theme editor (no code)
- HTML embed blocks for booking widgets

---

### 3.2 MODULE 2: Products & Bundles System

#### 3.2.1 Product Structure

**Bundle Products (Shopify Products):**

| Product Type | SKU Format | Price | Credits Issued | Metafields |
|-------------|------------|-------|----------------|------------|
| Podcast Starter | `BUNDLE-POD-001` | Variable | 1 Podcast Room | `credit_type: podcast`, `credit_qty: 1` |
| Creator Combo | `BUNDLE-CC-001` | Variable | 1 Podcast + 1 White Wall | `credit_type: mixed`, `credit_qty: 2` |
| Content Pro | `BUNDLE-CP-001` | Variable | Multiple credits | `credit_type: mixed`, `credit_qty: X` |

**Coaching Services (Shopify Products):**

| Service | SKU Format | Booking Type | Notes |
|---------|------------|--------------|-------|
| Beginner Coaching | `COACH-BEG-001` | Appointment | No lock access |
| Advanced Coaching | `COACH-ADV-001` | Appointment | No lock access |

**Add-On Products:**

| Add-On | SKU Format | Type | Assignment |
|--------|------------|------|------------|
| Camera Operator | `ADDON-CAM-001` | Service | Internal assignment |
| Photographer | `ADDON-PHOTO-001` | Service | Internal assignment |
| On-site Assistant | `ADDON-ASSIST-001` | Service | Internal assignment |
| Video Editing | `ADDON-EDIT-001` | Post-production | Not time-based |
| Clips / Short-form | `ADDON-CLIPS-001` | Post-production | Not time-based |
| Social Cuts | `ADDON-SOCIAL-001` | Post-production | Not time-based |

#### 3.2.2 Credit System Logic

**Credit Storage:**
- **Option 1:** Shopify Customer Metafields
  - `credits_podcast: integer`
  - `credits_whitewall: integer`
  - `credits_total: integer`
- **Option 2:** SimplyBook.me Customer Notes/Custom Fields
- **Option 3:** External database (not recommended for budget)

**Credit Flow:**
1. Customer purchases bundle → Shopify order created
2. Make.com webhook triggers on order creation
3. Extract credit quantity from product metafields
4. Add credits to customer account (metafield or SimplyBook.me)
5. Customer redeems credits when booking

**Implementation:**
- Use Shopify Customer Metafields (recommended)
- Make.com workflow: `Shopify Order Created` → `Update Customer Metafield`

---

### 3.3 MODULE 3: Booking Integration (SimplyBook.me)

#### 3.3.1 SimplyBook.me Configuration

**Services Setup:**

| Service Name | Duration | Price | Lock Required | Notes |
|--------------|----------|-------|---------------|-------|
| Podcast Room | 1 hour | $0 (credit-based) | Yes | Room Lock #1 |
| White Wall Studio Room 1 | 1 hour | $0 (credit-based) | Yes | Room Lock #2 |
| White Wall Studio Room 2 | 1 hour | $0 (credit-based) | Yes | Room Lock #3 |
| White Wall Studio Room 3 | 1 hour | $0 (credit-based) | Yes | Room Lock #4 |
| Beginner Coaching | 60 min | Variable | No | Appointment only |
| Advanced Coaching | 60 min | Variable | No | Appointment only |

**Booking Rules:**
- Credit check before booking (via SimplyBook.me custom field or API)
- Front door lock code always required for room bookings
- Room-specific lock code for selected room
- Buffer time: 10 minutes before/after booking

#### 3.3.2 Widget Embedding

**Implementation:**
```html
<!-- SimplyBook.me Widget Embed -->
<div id="sbw_booking_widget">
  <script src="https://widget.simplybook.me/v2/widget/widget.js"></script>
  <script>
    var widget = new SimplybookWidget({
      "widget_type": "iframe",
      "url": "https://[your-company].simplybook.me/v2/",
      "theme": "default",
      "theme_settings": {
        "timeline_show_end_time": "1",
        "sb_base_color": "#e3f2fd",
        "display_item_mode": "block",
        "body_bg_color": "#f5f5f5",
        "sb_review_image": "",
        "sb_company_label_color": "#ffffff",
        "hide_img_mode": "0",
        "sb_busy": "#dad2ce",
        "sb_available": "#d3e0f1"
      },
      "timeline": "modern_week",
      "datepicker": "top_calendar",
      "is_rtl": false,
      "app_config": {
        "clear_session": 0,
        "allow_switch_to_ada": 0,
        "predefined": {
          "service": ""
        }
      }
    });
  </script>
</div>
```

**Pages to Embed:**
- `/pages/rooms-spaces` - Full booking widget
- `/pages/book-now` - Full booking widget

---

### 3.4 MODULE 4: Smart Lock Integration (IglooHome API)

#### 4.1 IglooHome API Setup

**Required API Credentials:**
- API Key
- API Secret
- Lock IDs (4 locks total):
  - Front Door Lock
  - Room Lock #1 (Podcast Room)
  - Room Lock #2 (White Wall Studio 1)
  - Room Lock #3 (White Wall Studio 2)
  - Room Lock #4 (White Wall Studio 3)

#### 4.2 Code Generation Logic

**Access Window Calculation:**
```
Booking Start: 2024-01-15 10:00:00
Booking End: 2024-01-15 11:00:00

Code Start Time: 2024-01-15 09:50:00 (start - 10 min)
Code End Time: 2024-01-15 11:10:00 (end + 10 min)
```

**API Endpoints Required:**
- Create PIN code: `POST /v2/locks/{lock_id}/pincodes`
- Update PIN code: `PUT /v2/locks/{lock_id}/pincodes/{pincode_id}`
- Delete PIN code: `DELETE /v2/locks/{lock_id}/pincodes/{pincode_id}`

**Code Generation Rules:**
1. **Room Booking:** Generate 2 codes
   - Front door code (all room bookings)
   - Room-specific code
2. **Multiple Rooms:** Generate front door code once + room codes for each
3. **Code Format:** 6-8 digit PIN (IglooHome default)

#### 4.3 Make.com Workflow: Lock Code Generation

**Trigger:** SimplyBook.me booking confirmed

**Steps:**
1. Get booking details (service, time, customer)
2. Determine which locks needed
3. Calculate access window (start -10 min, end +10 min)
4. Generate front door code (if room booking)
5. Generate room-specific code(s)
6. Store codes in booking metadata
7. Trigger email notification

**Error Handling:**
- API failure → Retry 3 times
- Lock unavailable → Notify admin
- Code generation failed → Manual intervention alert

---

### 3.5 MODULE 5: Automation Workflows (Make.com)

#### 5.1 Workflow 1: Bundle Purchase → Credit Issuance

**Trigger:** Shopify Order Created (webhook)

**Conditions:**
- Order contains bundle product
- Order status = "paid"

**Steps:**
1. Get order details
2. Extract bundle product(s)
3. Read product metafields (credit_type, credit_qty)
4. Get customer ID
5. Read current customer credits (metafield)
6. Calculate new credit total
7. Update customer metafield with new credits
8. Send confirmation email to customer

**Data Mapping:**
```
Order → Products → Metafields → Customer Metafields
```

#### 5.2 Workflow 2: Booking Confirmed → Lock Codes

**Trigger:** SimplyBook.me Booking Created (webhook)

**Conditions:**
- Service requires lock access (Podcast Room or White Wall Studio)
- Booking status = "confirmed"

**Steps:**
1. Get booking details (service, time, customer, booking ID)
2. Identify required locks:
   - Front door (always for room bookings)
   - Room lock (based on service)
3. Calculate access window:
   - Start: booking_start - 10 minutes
   - End: booking_end + 10 minutes
4. Generate front door code via IglooHome API
5. Generate room code via IglooHome API
6. Store codes in SimplyBook.me booking notes/custom fields
7. Trigger email notification workflow

**Error Handling:**
- If code generation fails → Alert admin, manual code creation

#### 5.3 Workflow 3: Booking Rescheduled → Update Codes

**Trigger:** SimplyBook.me Booking Updated (webhook)

**Conditions:**
- Booking time changed
- Booking has existing lock codes

**Steps:**
1. Get booking details (new time, booking ID)
2. Retrieve existing codes from booking notes
3. Delete old codes via IglooHome API
4. Calculate new access window
5. Generate new front door code
6. Generate new room code
7. Update booking notes with new codes
8. Send email with updated codes

#### 5.4 Workflow 4: Booking Canceled → Revoke Codes

**Trigger:** SimplyBook.me Booking Deleted/Canceled (webhook)

**Steps:**
1. Get booking details (booking ID)
2. Retrieve codes from booking notes
3. Delete front door code via IglooHome API
4. Delete room code via IglooHome API
5. Update booking status
6. Send cancellation confirmation

#### 5.5 Workflow 5: Credit Redemption Check

**Trigger:** SimplyBook.me Booking Created (webhook)

**Conditions:**
- Service requires credit (room bookings)
- Booking is credit-based (not direct payment)

**Steps:**
1. Get customer ID from booking
2. Check customer credits (Shopify metafield)
3. If credits available:
   - Deduct 1 credit
   - Update customer metafield
   - Allow booking to proceed
4. If no credits:
   - Cancel booking
   - Notify customer to purchase bundle
   - Send email with bundle purchase link

#### 5.6 Workflow 6: Combined Email for Multiple Bookings

**Trigger:** SimplyBook.me Booking Created (webhook)

**Conditions:**
- Customer has multiple room bookings on same day
- All bookings confirmed

**Steps:**
1. Get all customer bookings for the day
2. Collect all access codes
3. Format email message (HTML template):
   ```
   Subject: Your Booking Access Codes - [Date]
   
   Your booking access codes:
   Front Door: [CODE]
   Podcast Room: [CODE]
   White Wall Studio 1: [CODE]
   Access: [START TIME] - [END TIME]
   ```
4. Send single email with all codes via Shopify Email
5. Mark email as sent in booking notes

#### 5.7 Workflow 7: CRM Sync & Tagging

**Trigger:** Multiple (Order Created, Booking Created)

**Steps:**
1. Get customer data (Shopify + SimplyBook.me)
2. Check if customer exists in Zoho
3. If new: Create Zoho contact
4. If existing: Update contact
5. Add tags based on:
   - Services used
   - Bundles purchased
   - Booking frequency
   - Total spend
6. Sync booking history
7. Update last activity date

**Tag Examples:**
- `bundle-podcast-starter`
- `service-podcast-room`
- `service-white-wall`
- `coaching-beginner`
- `frequent-customer`
- `high-value`

---

### 3.6 MODULE 6: CRM Integration (Zoho)

#### 6.1 Data Sync Structure

**Customer Fields:**
- Name, Email, Phone
- Shopify Customer ID
- SimplyBook.me Customer ID
- Total Credits (current)
- Total Bookings
- Last Booking Date
- Preferred Services
- Tags

**Booking Records:**
- Booking ID
- Service Name
- Booking Date/Time
- Status
- Access Codes (encrypted)
- Amount Paid

**Integration Method:**
- Make.com → Zoho CRM API
- Real-time sync on order/booking events

---

### 3.7 MODULE 7: Email Notifications

#### 7.1 Shopify Email Service Setup

**Service:** Shopify Email (Built-in)

**Required:**
- Shopify store with email service enabled
- Email templates configured
- Customer email addresses (from Shopify/SimplyBook.me)

**Benefits:**
- ✅ No additional cost (included with Shopify - up to 10,000 emails/month free)
- ✅ No separate account needed (one less integration)
- ✅ Integrated with customer data automatically
- ✅ Professional email templates
- ✅ Better deliverability (Shopify's email infrastructure)
- ✅ Simpler setup (no API keys for SMS provider)
- ✅ Lower ongoing costs (no per-message charges)

**Trade-offs:**
- ⚠️ Email vs SMS: Customers may not check email as quickly as SMS
- ⚠️ Spam folder risk (though Shopify has good deliverability)
- ⚠️ Less immediate than SMS (but more cost-effective)

**Implementation:**
- Use Make.com's Shopify module to send transactional emails
- Or use Shopify's email API directly
- Email templates can be customized in Shopify admin

#### 7.2 Email Templates

**Booking Confirmation Email:**
```
Subject: Your Booking Confirmation - [Service Name] on [Date]

Hi [Name],

Your booking is confirmed!

Service: [Service Name]
Date: [Date]
Time: [Start] - [End]

ACCESS CODES:
Front Door: [CODE]
[Room]: [CODE]

Access Window: [Start-10min] to [End+10min]

Please arrive 10 minutes before your booking time.

Thank you!
```

**Reschedule Notification Email:**
```
Subject: Your Booking Has Been Rescheduled

Hi [Name],

Your booking has been rescheduled:

New Date: [Date]
New Time: [Start] - [End]

UPDATED ACCESS CODES:
Front Door: [CODE]
[Room]: [CODE]

New Access Window: [Start-10min] to [End+10min]

Thank you!
```

**Cancellation Email:**
```
Subject: Booking Cancellation - [Date]

Hi [Name],

Your booking on [Date] at [Time] has been canceled.

Your credits have been restored to your account.

If you have any questions, please contact us.

Thank you!
```

**Bundle Purchase Confirmation:**
```
Subject: Your Bundle Purchase - Credits Added

Hi [Name],

Thank you for your purchase!

Bundle: [Bundle Name]
Credits Added: [Quantity] [Credit Type]

You can now use these credits to book rooms.

Book Now: [Booking Link]

Thank you!
```

---

## 4. INTEGRATION POINTS

### 4.1 Shopify ↔ Make.com

**Webhooks:**
- `orders/create` - Bundle purchase
- `orders/paid` - Payment confirmation
- `customers/create` - New customer
- `customers/update` - Customer update

**API Calls:**
- Get order details
- Get customer details
- Update customer metafields
- Get product metafields

### 4.2 SimplyBook.me ↔ Make.com

**Webhooks:**
- Booking created
- Booking updated
- Booking canceled/deleted
- Customer created

**API Calls:**
- Get booking details
- Update booking notes/custom fields
- Get customer credits
- Check service availability

### 4.3 IglooHome API ↔ Make.com

**API Endpoints:**
- `POST /v2/locks/{lock_id}/pincodes` - Create code
- `PUT /v2/locks/{lock_id}/pincodes/{id}` - Update code
- `DELETE /v2/locks/{lock_id}/pincodes/{id}` - Delete code
- `GET /v2/locks/{lock_id}/pincodes` - List codes

**Authentication:**
- API Key + Secret
- OAuth (if available)

### 4.4 Zoho CRM ↔ Make.com

**API Calls:**
- Create/Update contact
- Add tags
- Create deal/booking record
- Search contacts

### 4.5 Shopify Email ↔ Make.com

**API Calls:**
- Send email via Shopify Email API
- Use Shopify's email templates
- Track email delivery (via Shopify)

**Alternative Method:**
- Use Make.com's Shopify module to send transactional emails
- Or use Make.com's Email module with SMTP (if Shopify API limited)

---

## 5. DATA FLOW DIAGRAMS

### 5.1 Bundle Purchase Flow

```
Customer → Shopify Checkout → Order Created
    ↓
Make.com Webhook Triggered
    ↓
Extract Bundle Product → Read Metafields
    ↓
Get Customer → Read Current Credits
    ↓
Calculate New Credits → Update Customer Metafield
    ↓
Send Confirmation Email
    ↓
Sync to Zoho CRM
```

### 5.2 Booking & Lock Code Flow

```
Customer → SimplyBook.me Widget → Booking Created
    ↓
Make.com Webhook Triggered
    ↓
Check Customer Credits
    ↓
If Credits Available:
    ↓
Deduct Credit → Update Customer Metafield
    ↓
Calculate Access Window (start-10min, end+10min)
    ↓
Generate Front Door Code (IglooHome API)
    ↓
Generate Room Code (IglooHome API)
    ↓
Store Codes in Booking Notes
    ↓
Send Email with Codes
    ↓
Sync Booking to Zoho CRM
```

### 5.3 Reschedule Flow

```
Customer → SimplyBook.me → Reschedule Booking
    ↓
Make.com Webhook Triggered
    ↓
Retrieve Old Codes from Booking Notes
    ↓
Delete Old Codes (IglooHome API)
    ↓
Calculate New Access Window
    ↓
Generate New Front Door Code
    ↓
Generate New Room Code
    ↓
Update Booking Notes
    ↓
Send Email with Updated Codes
```

---

## 6. API SPECIFICATIONS

### 6.1 IglooHome API Details

**Base URL:** `https://api.igloohome.co/v2`

**Authentication:**
```json
{
  "api_key": "your_api_key",
  "api_secret": "your_api_secret"
}
```

**Create PIN Code:**
```http
POST /locks/{lock_id}/pincodes
Content-Type: application/json

{
  "name": "Booking-{booking_id}",
  "pin": "auto",  // or specific PIN
  "schedule": {
    "start": "2024-01-15T09:50:00Z",
    "end": "2024-01-15T11:10:00Z"
  },
  "max_uses": 1
}
```

**Response:**
```json
{
  "pincode_id": "12345",
  "pin": "123456",
  "schedule": {
    "start": "2024-01-15T09:50:00Z",
    "end": "2024-01-15T11:10:00Z"
  }
}
```

### 6.2 SimplyBook.me API

**Base URL:** `https://[company].simplybook.me/api`

**Get Booking:**
```http
GET /bookings/{booking_id}
Authorization: Bearer {token}
```

**Update Booking Notes:**
```http
PUT /bookings/{booking_id}
{
  "notes": "Front Door: 123456, Room: 789012"
}
```

### 6.3 Shopify API

**Update Customer Metafield:**
```http
PUT /admin/api/2024-01/customers/{customer_id}/metafields/{metafield_id}.json
{
  "metafield": {
    "value": "2",
    "type": "integer"
  }
}
```

---

## 7. IMPLEMENTATION STEPS

### Phase 1: Setup & Discovery (Days 1-2)
- [ ] Review PDF brief
- [ ] Set up all required accounts (Shopify, SimplyBook.me, Make.com, IglooHome, Zoho)
- [ ] Install Dawn theme
- [ ] Gather content, images, copy
- [ ] Define exact service names, pricing, bundle structures

### Phase 2: Theme & Pages (Days 3-5)
- [ ] Configure Dawn theme (logo, colors, fonts)
- [ ] Create all 12+ pages
- [ ] Set up navigation structure
- [ ] Add content to pages
- [ ] Embed SimplyBook.me widgets on booking pages
- [ ] Test page responsiveness

### Phase 3: Products & Bundles (Days 6-7)
- [ ] Create bundle products in Shopify
- [ ] Add product metafields (credit_type, credit_qty)
- [ ] Create coaching service products
- [ ] Create add-on products
- [ ] Set up product collections
- [ ] Configure pricing

### Phase 4: SimplyBook.me Setup (Days 8-10)
- [ ] Create services in SimplyBook.me
- [ ] Configure booking rules
- [ ] Set up custom fields for credits
- [ ] Test booking widget embedding
- [ ] Configure webhooks to Make.com

### Phase 5: IglooHome Integration (Days 11-13)
- [ ] Obtain API credentials
- [ ] Test API connection
- [ ] Map locks to services
- [ ] Build code generation logic in Make.com
- [ ] Test code creation/deletion
- [ ] Verify access window calculations

### Phase 6: Make.com Workflows (Days 14-17)
- [ ] Build Workflow 1: Bundle → Credits
- [ ] Build Workflow 2: Booking → Codes
- [ ] Build Workflow 3: Reschedule → Update Codes
- [ ] Build Workflow 4: Cancel → Revoke Codes
- [ ] Build Workflow 5: Credit Check
- [ ] Build Workflow 6: Combined Email
- [ ] Build Workflow 7: CRM Sync
- [ ] Test each workflow individually

### Phase 7: CRM & Payments (Days 18-19)
- [ ] Set up Zoho CRM integration
- [ ] Configure customer sync
- [ ] Set up tagging rules
- [ ] Configure card-on-file (Stripe)
- [ ] Test payment flows

### Phase 8: Testing & QA (Days 20-21)
- [ ] End-to-end test: Purchase bundle → Receive credits
- [ ] End-to-end test: Book room → Receive codes
- [ ] Test reschedule flow
- [ ] Test cancellation flow
- [ ] Test multiple bookings on same day
- [ ] Test credit redemption
- [ ] Test email notifications
- [ ] Test CRM sync
- [ ] Fix bugs and edge cases

### Phase 9: Launch Prep (Day 22)
- [ ] Final content review
- [ ] Performance testing
- [ ] Documentation
- [ ] Go-live checklist
- [ ] Launch

---

## 8. TESTING REQUIREMENTS

### 8.1 Functional Testing

**Bundle Purchase:**
- [ ] Customer purchases bundle
- [ ] Credits added to account
- [ ] Confirmation email sent
- [ ] Customer synced to CRM

**Room Booking:**
- [ ] Customer with credits books room
- [ ] Credit deducted
- [ ] Lock codes generated
- [ ] Email sent with codes
- [ ] Codes work at correct time window

**Reschedule:**
- [ ] Customer reschedules booking
- [ ] Old codes revoked
- [ ] New codes generated
- [ ] Updated email sent

**Cancellation:**
- [ ] Customer cancels booking
- [ ] Codes revoked
- [ ] Credit restored
- [ ] Cancellation confirmation sent

**Multiple Bookings:**
- [ ] Customer books multiple rooms same day
- [ ] Single front door code generated
- [ ] Room codes generated for each
- [ ] Combined email sent

### 8.2 Edge Cases

- [ ] Customer books without credits (should fail)
- [ ] API failure during code generation (retry logic)
- [ ] Booking rescheduled multiple times
- [ ] Customer books at exact same time (conflict handling)
- [ ] Email delivery failure (check spam, retry logic)

### 8.3 Performance Testing

- [ ] Page load times (< 3 seconds)
- [ ] Widget load times (< 2 seconds)
- [ ] API response times (< 1 second)
- [ ] Workflow execution times (< 5 seconds)

---

## 9. RISK ASSESSMENT

### 9.1 Technical Risks

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| IglooHome API changes | High | Low | Monitor API docs, version pinning |
| SimplyBook.me widget issues | Medium | Medium | Test thoroughly, have fallback |
| Make.com workflow failures | High | Medium | Error handling, retry logic, alerts |
| Credit system logic errors | High | Medium | Extensive testing, manual override |
| Email delivery failures | Low | Low | Spam filtering, retry logic, delivery tracking |

### 9.2 Budget Risks

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| Scope creep | High | Medium | Strict scope control, change requests |
| API subscription costs | Medium | Medium | Research costs upfront |
| Complex credit logic | High | Medium | Use simple metafield approach |
| Testing time overrun | Medium | Medium | Allocate buffer time |

### 9.3 Timeline Risks

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| API access delays | Medium | Low | Request access early |
| Content delays | Low | Medium | Request content upfront |
| Integration issues | High | Medium | Build in buffer time |
| Testing complexity | Medium | Medium | Test incrementally |

---

## 10. SUCCESS CRITERIA

### 10.1 Functional Requirements

✅ All pages created and accessible  
✅ Bundle products functional  
✅ Booking widget embedded and working  
✅ Credit system operational  
✅ Lock codes generated automatically  
✅ Email notifications sent  
✅ CRM sync working  
✅ Reschedule/cancel flows functional  

### 10.2 Performance Requirements

✅ Page load < 3 seconds  
✅ Booking widget load < 2 seconds  
✅ Code generation < 5 seconds  
✅ Email delivery < 60 seconds  

### 10.3 Business Requirements

✅ Customers can purchase bundles  
✅ Customers can redeem credits  
✅ Customers receive access codes  
✅ Codes work at correct times  
✅ System handles reschedules/cancellations  

---

## 11. MAINTENANCE & SUPPORT

### 11.1 Ongoing Requirements

- Monitor Make.com workflows daily
- Check API health (IglooHome, SimplyBook.me)
- Review failed email deliveries
- Monitor credit balance accuracy
- Update CRM tags as needed

### 11.2 Documentation

- Admin guide for managing credits
- Troubleshooting guide for common issues
- API credentials documentation (secure)
- Workflow diagrams

---

## 12. APPENDIX

### 12.1 Required Shopify Apps (Optional)

- **Customer Metafields:** Built-in (no app needed)
- **Booking Widget:** SimplyBook.me (embedded)
- **Email:** Via Shopify Email Service (built-in, no app needed)

### 12.2 Cost Breakdown Estimate

| Item | Cost | Notes |
|------|------|-------|
| Theme (Dawn) | $0 | Free |
| SimplyBook.me | $9-29/mo | Subscription |
| Make.com | $9-29/mo | Subscription |
| IglooHome | $2/lock/mo | $8/mo for 4 locks |
| Shopify Email | $0 | Included with Shopify (up to 10,000 emails/month free) |
| Zoho CRM | $14-23/mo | Subscription |
| **Development** | **$1,000** | **One-time** |
| **Monthly Ongoing** | **~$40-90** | **Subscriptions (reduced - no SMS costs)** |

---

**Document Version:** 1.0  
**Last Updated:** 2024-01-XX  
**Author:** Technical Specification Team
