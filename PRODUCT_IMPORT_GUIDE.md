# Product Import Guide - BookLock Studio

## 📦 Products Created

The CSV file `booklock_studio_products.csv` contains **11 products** ready for import:

### Bundle Products (3)
1. **Podcast Starter Bundle** - $79.99
   - 1 Podcast Room credit
   - SKU: BUNDLE-POD-001

2. **Creator Combo Bundle** - $149.99
   - 1 Podcast Room + 1 White Wall Studio credit
   - SKU: BUNDLE-CC-001

3. **Content Pro Bundle** - $299.99 (Compare at: $399.99)
   - 5 credits (mix and match)
   - SKU: BUNDLE-CP-001

### Coaching Services (2)
4. **Beginner Coaching Session** - $99.99
   - 60-minute one-on-one session
   - SKU: COACH-BEG-001

5. **Advanced Coaching Session** - $149.99
   - 60-minute advanced session
   - SKU: COACH-ADV-001

### Add-On Services (6)
6. **Camera Operator Add-On** - $199.99
   - SKU: ADDON-CAM-001

7. **Photographer Add-On** - $179.99
   - SKU: ADDON-PHOTO-001

8. **On-Site Assistant Add-On** - $99.99
   - SKU: ADDON-ASSIST-001

9. **Video Editing Add-On** - $249.99
   - SKU: ADDON-EDIT-001

10. **Clips / Short-Form Content Add-On** - $149.99
    - SKU: ADDON-CLIPS-001

11. **Social Cuts Add-On** - $179.99
    - SKU: ADDON-SOCIAL-001

## 📸 Image Requirements Summary

Each product description includes detailed image requirements. Here's a quick reference:

| Product | Image Type | Key Elements |
|---------|-----------|--------------|
| **Podcast Starter** | Podcast studio | Microphones, headphones, soundproofing, professional equipment |
| **Creator Combo** | Split/collage | Both podcast studio + white wall studio |
| **Content Pro** | Premium montage | Multiple studios or creator in action |
| **Beginner Coaching** | Coaching session | Two people, collaborative, approachable |
| **Advanced Coaching** | Business consultation | Analytics, strategy, professional |
| **Camera Operator** | Camera operator | Person operating professional camera |
| **Photographer** | Photographer | Person with camera, adjusting lighting |
| **On-Site Assistant** | Production assistant | Setting up equipment, helping |
| **Video Editing** | Editing workspace | Computer screen with editing software |
| **Clips/Short-Form** | Social media | Phone with TikTok/Reels, short clips |
| **Social Cuts** | Multi-platform | Multiple social media platforms grid |

**All Images:**
- Minimum: 1200x1200px
- Format: Square preferred
- Style: Professional, clean, modern
- Quality: High resolution

## 🚀 How to Import

### Step 1: Prepare Images
1. Generate images using Shopify's image generator (or your preferred tool)
2. Follow the image requirements in each product description
3. Save images with descriptive names (e.g., `podcast-starter-bundle.jpg`)

### Step 2: Import CSV to Shopify
1. Go to **Products > All products** in Shopify admin
2. Click **Import** button
3. Select `booklock_studio_products.csv`
4. Map the columns (Shopify will auto-detect most)
5. Click **Import products**

### Step 3: Add Images
1. After import, go to each product
2. Click **Add image** in the product editor
3. Upload the corresponding image
4. Set as primary image

### Step 4: Set Up Collections
1. Create **Bundles** collection
   - Add: Podcast Starter, Creator Combo, Content Pro
2. Create **Coaching** collection
   - Add: Beginner Coaching, Advanced Coaching
3. Create **Add-Ons** collection
   - Add: All 6 add-on products

### Step 5: Add Metafields (Important!)

For **Bundle products only**, add these metafields:

**Podcast Starter Bundle:**
- `custom.credit_type` = `podcast`
- `custom.credit_qty` = `1`

**Creator Combo Bundle:**
- `custom.credit_type` = `mixed`
- `custom.credit_qty` = `2`

**Content Pro Bundle:**
- `custom.credit_type` = `mixed`
- `custom.credit_qty` = `5`

**How to Add Metafields:**
1. Go to product edit page
2. Scroll to **Metafields** section
3. Click **Add metafield**
4. Create definition:
   - Namespace: `custom`
   - Key: `credit_type` (Type: Single line text)
   - Key: `credit_qty` (Type: Integer)
5. Add values for each bundle product

## ✅ Product Settings

All products are configured with:
- ✅ **Requires Shipping:** FALSE (services/credits)
- ✅ **Taxable:** TRUE
- ✅ **Inventory:** Tracked (999 qty set)
- ✅ **Status:** Active
- ✅ **Published:** TRUE

## 📋 Post-Import Checklist

- [ ] All 11 products imported successfully
- [ ] Images added to each product
- [ ] Metafields added to bundle products
- [ ] Collections created and products assigned
- [ ] Prices verified
- [ ] Descriptions reviewed
- [ ] SEO titles and descriptions checked
- [ ] Product tags verified

## 💡 Notes

1. **Pricing:** All prices are placeholder values. Adjust as needed.
2. **Images:** Use Shopify's image generator or professional stock photos
3. **Metafields:** Critical for credit system automation - don't skip!
4. **Collections:** Organize products for better navigation
5. **Descriptions:** Include image requirements for reference when generating images

## 🎯 Next Steps After Import

1. Review all product pages
2. Customize descriptions if needed
3. Add additional product images (gallery)
4. Set up product variants if needed
5. Configure related products
6. Test product pages on storefront

---

**File Location:** `c:\Users\mrmal\Downloads\booklock_studio_products.csv`
**Total Products:** 11
**Status:** Ready for import
