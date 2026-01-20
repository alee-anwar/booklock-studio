# BookLock Studio - Setup Guide

## ✅ Completed Setup

### 1. Homepage ✅
- Updated with hero banner
- Services overview section
- How It Works section
- Call-to-action section

### 2. Page Templates Created ✅
- `/pages/book-now` - Booking widget page
- `/pages/services` - Services listing
- `/pages/how-it-works` - Step-by-step guide
- `/pages/pricing` - Pricing information
- `/pages/faq` - Frequently asked questions
- `/pages/rooms-spaces` - Rooms/Spaces booking page
- `/pages/contact` - Contact form (already exists)

## 📋 Next Steps (To Complete in Shopify Admin)

### Step 1: Create Pages in Shopify Admin

1. Go to **Online Store > Pages** in Shopify admin
2. Create the following pages (use the templates we created):

| Page Title | Handle | Template |
|------------|--------|----------|
| Book Now | `book-now` | `page.booking` |
| Services | `services` | `page.services` |
| How It Works | `how-it-works` | `page.how-it-works` |
| Pricing | `pricing` | `page.pricing` |
| FAQ | `faq` | `page.faq` |
| Rooms / Spaces | `rooms-spaces` | `page.rooms` |

**Instructions:**
- Click "Add page"
- Enter the page title
- Set the handle (URL) as shown above
- In the template section, select the corresponding template
- Add placeholder content if needed (templates have default content)
- Save and publish

### Step 2: Set Up Navigation Menu

1. Go to **Online Store > Navigation** in Shopify admin
2. Edit **Main menu**:

```
Main Menu Structure:
├── Home (link to /)
├── Services (link to /pages/services)
│   ├── Rooms / Spaces (link to /pages/rooms-spaces)
│   ├── Bundles / Packages (link to /collections/bundles - create collection first)
│   ├── Coaching (link to /pages/services#coaching)
│   └── Add-Ons (link to /collections/add-ons - create collection first)
├── Pricing (link to /pages/pricing)
├── Book Now (link to /pages/book-now)
├── How It Works (link to /pages/how-it-works)
└── Contact (link to /pages/contact)
```

3. Edit **Footer menu**:

```
Footer Menu Structure:
├── Policies
│   ├── Privacy Policy (link to /pages/privacy-policy)
│   ├── Terms of Service (link to /pages/terms-of-service)
│   └── Refund Policy (link to /pages/refund-policy)
└── FAQ (link to /pages/faq)
```

### Step 3: Create Product Collections

1. Go to **Products > Collections** in Shopify admin
2. Create **Bundles** collection:
   - Title: "Bundles"
   - Handle: `bundles`
   - Description: "Credit bundles for booking studio time"
   - Collection type: Manual or Automated (your choice)

3. Create **Add-Ons** collection:
   - Title: "Add-Ons"
   - Handle: `add-ons`
   - Description: "Additional services and add-ons"
   - Collection type: Manual or Automated

### Step 4: Create Placeholder Products

1. Go to **Products > All products** in Shopify admin
2. Create bundle products (at least 2-3 for demo):

**Example Bundle 1: Podcast Starter**
- Title: "Podcast Starter Bundle"
- Price: $XX.XX (set your price)
- SKU: `BUNDLE-POD-001`
- Product type: Bundle
- Collection: Bundles
- Description: "Includes 1 credit for Podcast Room booking"
- **Metafields to add:**
  - `custom.credit_type` = `podcast`
  - `custom.credit_qty` = `1`

**Example Bundle 2: Creator Combo**
- Title: "Creator Combo Bundle"
- Price: $XX.XX (set your price)
- SKU: `BUNDLE-CC-001`
- Product type: Bundle
- Collection: Bundles
- Description: "Includes 1 Podcast Room credit + 1 White Wall Studio credit"
- **Metafields to add:**
  - `custom.credit_type` = `mixed`
  - `custom.credit_qty` = `2`

**To Add Metafields:**
1. In product edit page, scroll to **Metafields** section
2. Click "Add metafield"
3. Create custom metafield definitions:
   - Namespace: `custom`
   - Key: `credit_type` (Type: Single line text)
   - Key: `credit_qty` (Type: Integer)

### Step 5: Configure SimplyBook.me Widget

1. Get your SimplyBook.me company name/URL
2. Go to each page template that uses the widget:
   - `/pages/book-now`
   - `/pages/rooms-spaces`
3. In Shopify theme editor, edit the page
4. Find the "Booking Widget" section
5. Replace `[your-company]` with your actual SimplyBook.me company name

**Example:**
If your SimplyBook.me URL is `https://booklockstudio.simplybook.me`, replace:
```
"url": "https://[your-company].simplybook.me/v2/"
```
with:
```
"url": "https://booklockstudio.simplybook.me/v2/"
```

### Step 6: Theme Customization (Optional)

1. Go to **Online Store > Themes > Customize**
2. **Logo:**
   - Upload your logo in Header section
   - Adjust logo width if needed

3. **Colors:**
   - Adjust color scheme if needed
   - Current scheme uses default Dawn colors

4. **Homepage:**
   - Add hero image to the banner section
   - Adjust text and button colors if needed

### Step 7: Test the Store

1. **Homepage:**
   - Check hero banner displays correctly
   - Verify services section shows 3 services
   - Test "Book Now" and "View Services" buttons

2. **Pages:**
   - Visit each page and verify content displays
   - Check booking widget loads (once SimplyBook.me is configured)
   - Test FAQ accordion functionality

3. **Navigation:**
   - Test main menu links
   - Test footer menu links
   - Verify mobile menu works

4. **Products:**
   - View bundle products
   - Test product pages
   - Verify collections display correctly

## 🎯 What's Ready to Show Client

### ✅ Fully Functional:
- Homepage with hero, services, and CTA
- All key pages created with content
- FAQ page with accordion
- Page templates ready for SimplyBook.me integration

### ⚠️ Needs Configuration:
- SimplyBook.me widget URL (replace placeholder)
- Actual product creation in Shopify admin
- Navigation menu setup in Shopify admin
- Logo and branding customization

### 📝 Client Presentation Checklist:

1. **Show Homepage:**
   - Professional hero section
   - Services overview
   - Clear call-to-action

2. **Demonstrate Pages:**
   - Book Now page (with widget placeholder)
   - Services page (detailed service listings)
   - How It Works (step-by-step guide)
   - FAQ (interactive accordion)

3. **Explain Next Steps:**
   - SimplyBook.me integration (once account is set up)
   - Product creation (bundle products)
   - Navigation setup
   - Final content review

## 🔧 Technical Notes

- All templates use Dawn theme sections (no custom code)
- SimplyBook.me widget uses custom-liquid section
- FAQ uses collapsible-content section
- All pages are responsive and mobile-friendly
- Templates follow Shopify best practices

## 📞 Support

If you need help with:
- Shopify admin setup → Refer to Shopify documentation
- SimplyBook.me integration → Refer to SimplyBook.me docs
- Theme customization → Use Shopify theme editor
- Product metafields → Shopify metafields documentation

---

**Last Updated:** [Current Date]
**Status:** Ready for client presentation (pending SimplyBook.me URL and product setup)
