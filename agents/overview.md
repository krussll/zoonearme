# Zoo Near Me (`zoonearme`) — Site Overview & Architecture Guide

**Repository**: `krussll/zoonearme`  
**Live Site**: [https://www.zoonear.me](https://www.zoonear.me)  
**Active Production Branch**: `gh-pages`  
**Hosting Platform**: GitHub Pages  
**Engine**: Jekyll 4.4.x (Ruby Static Site Generator)  
**Primary Author/Persona**: Reece Williams (`reece`)  

---

## 1. Executive Summary & Site Concept

**Zoo Near Me** is an editorial and programmatic content website providing state-by-state guides, pairwise comparisons, accessibility breakdowns, and animal spotting directories for zoological parks and wildlife sanctuaries across the United States.

### Core Value Proposition & Mission
- **State-by-State Exploration**: Help travelers and locals discover the best zoological parks in every US state.
- **Audience-Specific Practicality**: Give targeted, practical advice for families with toddlers, visitors needing disability/wheelchair access, and pet owners navigating strict zoo dog and service-animal policies.
- **Animal-Specific Spotting**: Guide wildlife fans to zoos that house specific sought-after animal species (e.g., capybaras, giraffes, elephants, tigers).
- **Direct Side-by-Side Comparisons**: Enable visitors to choose between nearby attractions by comparing ticket pricing, acreage, dwell time, animal varieties, and food policies.

---

## 2. Technical Stack & How It Works

```
                                +---------------------------+
                                |    Markdown Content       |
                                |  148 Posts in _posts/     |
                                |  Static Pages (*.md)      |
                                +-------------+-------------+
                                              |
                                              v
+------------------------+      +-------------+-------------+      +-------------------------+
|      Layouts           | ---> |      Jekyll 4.4.x Engine   | <--- |     Includes & Partials |
|  default, post, page   |      |   (GitHub Pages Builder)  |      |  postbox, sidebar, etc. |
+------------------------+      +-------------+-------------+      +-------------------------+
                                              |
                                              v
                                +-------------+-------------+
                                |  Compiled Static HTML/CSS |
                                |  Served via gh-pages      |
                                |  Domain: zoonear.me       |
                                +---------------------------+
```

### Technical Specifications
| Layer | Technology | Details / Configuration |
| :--- | :--- | :--- |
| **Static Site Generator** | Jekyll 4.4.1 | Liquid templating, SCSS compilation, Markdown parsing via kramdown |
| **Hosting & Deployment** | GitHub Pages | Deployed directly from branch `gh-pages` via `CNAME` (`zoonear.me`) |
| **CSS Framework** | Bootstrap 4.0.0-alpha.6 | Grid system, responsive utilities, card components |
| **Custom Styling** | `assets/css/theme.css`, `custom.scss` | Adapted from WowThemes "Affiliates" theme, palette centered on teal (`#277c80`) and lime (`#6FD350`) |
| **Typography & Icons** | Google Fonts ("Rubik") & FontAwesome 5.8.1 | Weights 400, 500, 700; iconography for social, UI, arrows |
| **Client-Side Scripts** | jQuery 3.x, Masonry, Lazysizes | Masonry dynamic grid layout for post cards, lazysizes image loading, scroll-to-top |
| **Analytics & Verification** | Google Tag Manager / GA4 | Measurement ID `G-HD88E8NJ31`; Google Site Verification `3hy6rng49LmxKJ53UCGkhh56RmYZTdFZoHEpkktdxOY` |
| **Monetization Stack** | AdSense, Ezoic, Amazon Affiliates | `ads.txt` (publisher `pub-8543572621618524`), Ezoic placeholders, Amazon Associates disclaimer |

---

## 3. Repository File Structure & Organization

```
zoosnearme/
├── .git/                      # Git repository tracking origin/gh-pages
├── agents/                    # Agent instructions and architectural documentation
│   └── overview.md            # This overview document
├── _layouts/                  # Master page wrappers & templates
│   ├── default.html           # Root layout: <head>, navbar, footer, scripts, Ezoic hooks
│   ├── post.html              # Standard 2-column post template (article + right sidebar)
│   ├── post-left-sidebar.html # Alternate post template with left-sidebar layout
│   ├── page.html              # Single-column layout for static informational pages
│   └── archive.html           # Category/tag archive listing layout
├── _includes/                 # Reusable template components
│   ├── postbox.html           # Masonry card component (image, title, 15-word excerpt)
│   ├── featuredbox.html       # Hero featured post card
│   ├── sidebar.html           # Right sidebar container
│   ├── recommended.html       # Sidebar links ("Best Zoos By State", "All States")
│   ├── all-posts.html         # Unordered list loop of all site posts
│   ├── search-form.html       # GET search form targeting /search
│   ├── pagination.html        # Multi-page pagination widget
│   ├── newsletter.html        # MailChimp subscription embed (template artifact)
│   ├── newsletter-signup.html # Sendinblue subscription embed (template artifact)
│   └── useful.html            # Unreferenced legacy include (lawn bowls artifact)
├── _posts/                    # 148 Markdown articles (dated 2023-06-30)
├── _sass/                     # SASS partials (_syntax.scss, config.scss)
├── assets/
│   ├── css/                   # Compiled CSS (theme.css, custom.scss)
│   ├── js/                    # jQuery, theme.js, search.js, masonry, lazysizes
│   ├── fonts/                 # Webfonts
│   └── images/                # 845+ zoo images, header graphics, zoonearme.png logo
├── _config.yml                # Jekyll global site configuration
├── CNAME                      # Custom apex domain: zoonear.me
├── index.html                 # Homepage with hero section, featured posts, and latest articles
├── zoos-by-states.md          # State index page filtering posts with category 'state'
├── about-me.md                # Author biography & mission statement
├── all-posts.md               # Masonry listing of all published articles
├── contact.md                 # Contact details page
├── affiliate-disclaimer.md    # Amazon Associates disclosure page
├── privacy.md                 # Privacy policy
├── terms.md                   # Terms and conditions
├── ads.txt                    # Google AdSense authorization record
├── sitemap.xml                # XML sitemap generator
├── feed.xml                   # RSS 2.0 feed generator
├── favicon.ico                # Root site favicon
└── robots.txt                 # Search engine crawler directives
```

---

## 4. Content Taxonomy & Post Archetypes

The site currently contains **148 content posts**, focused on **5 US states** (all beginning with the letter "M"):
- **Michigan**
- **Minnesota**
- **Mississippi**
- **Missouri**
- **Montana**

### The Four Post Archetypes

```
+---------------------------------------------------------------------------------+
|                                 1. State Pillars                                |
| Comprehensive roundups of all major zoos within a state                         |
| Slug: best-zoos-in-<state>.md | Category: [ state ]                             |
+---------------------------------------------------------------------------------+
                                         |
                                         +---> 2. Head-to-Head Comparisons (68 posts)
                                         |     Pairwise zoo comparisons (<zoo-a>-vs-<zoo-b>)
                                         |     Category: [ zoo ]
                                         |
                                         +---> 3. Animal-Specific Guides (35 posts)
                                         |     Targeting: Capybaras, Giraffes, Elephants, Tigers
                                         |     Category: [ zoo ]
                                         |
                                         +---> 4. Audience / Need Guides (40 posts)
                                               Disability access, Dog policies, Families, Toddlers
                                               Category: [ zoo ]
```

#### 1. State Pillar Guides (`categories: [ state ]`)
- **Quantity**: 5 posts (`best-zoos-in-michigan.md`, `best-zoos-in-minnesota.md`, `best-zoos-in-mississippi.md`, `best-zoos-in-missouri.md`, `best-zoos-in-montana.md`).
- **Structure**: Comprehensive long-form articles (15,000–31,000 bytes) reviewing each zoo in the state in numbered order with addresses, contact details, official websites, ticket pricing, acreages, and highlights.

#### 2. Head-to-Head Comparisons (`categories: [ zoo ]`)
- **Quantity**: 68 posts.
- **Structure**: Formatted with `<div class="overview">` containing an "At A Glance" comparison, followed by deep dives into:
  - Zoo Size and Dwell Times (e.g. 433 acres vs 79 acres)
  - Ticket Pricing & Family Cost of Four
  - Target Audience Suitability (toddlers vs older kids)
  - Animal Diversity & Signature Species
  - Accessibility & Wheelchair Path Conditions
  - Food, Beverage, and Pet Policies

#### 3. Animal-Specific Guides (`categories: [ zoo ]`)
- **Quantity**: 35 posts.
- **Target Species**: Capybaras, Elephants, Giraffes, Tigers.
- **Structure**: Reviews which zoos in the given state exhibit the species, including exhibit names, viewing tips, and direct advice when an animal is *not* present in that state (e.g. confirming no Minnesota zoos currently hold elephants).

#### 4. Need & Demographic Guides (`categories: [ zoo ]`)
- **Quantity**: 40 posts.
- **Focus Areas**:
  - **Disability Access**: ADA path grading, paved vs gravel vs woodchip terrain, sensory accommodations, stroller/wheelchair rental fees.
  - **Dogs & Pets**: Clarification that domestic dogs are prohibited for animal welfare and quarantine safety, while specifying service animal policies under the ADA.
  - **Families**: Best exhibits for mixed-age children, interactive zones, budget-friendly ticket bundles, shaded rest areas.
  - **Toddlers**: Gentle animals, petting zoos, splash pads, walking fatigue mitigation.

---

## 5. Frontmatter Data Model & Rendering Flow

Every article in `_posts/` utilizes standard YAML frontmatter:

```yaml
---
layout: post
title: "A Complete Guide To The Best Zoos In Michigan, USA"
author: reece
categories: [ state ]
image: assets/images/best-zoos-in-michigan.jpg
description: "Discover the magic of the animal kingdom in Michigan's top-tier zoos..."
first_paragraph: "<p>Michigan, the Great Lake State, is known for its diverse landscapes...</p>"
featured: true # (Optional, activates featured box on index.html)
---
```

### Template Rendering Pipeline
1. `_layouts/default.html` wraps all pages, providing global `<head>`, SEO meta tags, Google Analytics, responsive navbar, and footer.
2. `_layouts/post.html` renders:
   - Page title (`<h1 class="posttitle">`)
   - `first_paragraph` highlighted in `.intro-block`
   - Featured image (`<img>` with `itemprop="image"`)
   - Main content body (`{{ content }}`)
   - Published & Modified dates (`time` tags)
   - Previous / Next article navigation
   - Author profile box using `site.authors[page.author]`
   - Right sidebar (`{% include sidebar.html %}`)

---

## 6. Site Health Audit & Identified Issues

A rigorous codebase and build audit revealed several technical defects, template artifacts, and SEO issues that require remediation.

### 🔴 Critical Build Errors: 7 YAML Exceptions
During `jekyll build`, Jekyll throws fatal YAML parsing exceptions on 7 post files due to **unescaped double quotes** inside frontmatter fields:

| File | Error Cause | Location |
| :--- | :--- | :--- |
| `_posts/2023-06-30-best-zoos-in-mississippi-to-see-capybaras.md` | Unescaped `"water pig,"` inside double-quoted string | `first_paragraph` line 8 |
| `_posts/2023-06-30-best-zoos-in-missouri-for-dogs.md` | Unescaped `"Show-Me"` inside double-quoted string | `first_paragraph` line 8 |
| `_posts/2023-06-30-como-park-vs-lake-superior-zoo.md` | Unescaped `"behind the scenes"` inside double-quoted string | `first_paragraph` line 8 |
| `_posts/2023-06-30-deyoung-family-zoo-vs-indian-creek-zoo.md` | Accidental double-double-quotes `""Embark...` | `description` line 7 |
| `_posts/2023-06-30-garlyn-zoo-wildlife-park-vs-roscommon-zoo.md` | Unescaped `"no public transport"` inside double-quoted string | `first_paragraph` line 8 |
| `_posts/2023-06-30-garlyn-zoo-wildlife-park-vs-wilderness-trails-zoo.md` | Accidental double-double-quotes `""Explore...` | `description` line 7 |
| `_posts/2023-06-30-john-ball-zoo-vs-roscommon-zoo.md` | Accidental double-double-quotes `""Explore...` | `description` line 7 |

> [!WARNING]
> Because of these YAML errors, **Jekyll completely skips generating HTML pages for these 7 posts**, resulting in missing URLs and 404s on the live site!

---

### 🟡 Destination Conflicts & Duplicate Filenames
1. **Double-Hyphen Filename Collisions**:
   Three comparison articles exist twice with both single-hyphen and double-hyphen filenames:
   - `hattiesburg-zoo-vs-tupelo-buffalo-park--zoo.md` vs `...-tupelo-buffalo-park-zoo.md`
   - `the-jackson-zoo-vs-tupelo-buffalo-park--zoo.md` vs `...-tupelo-buffalo-park-zoo.md`
   - `zoomontana-vs-grizzly--wolf-discovery-center.md` vs `...-grizzly-wolf-discovery-center.md`  
   Jekyll generates the exact same permalink for both, triggering build warnings: `"The written file may end up with unexpected contents."`
2. **Singular vs Plural Cannibalization (20 Pairs)**:
   Every state has duplicate articles targeting both singular and plural forms (e.g. `best-zoo-in-michigan-for-dogs.md` and `best-zoos-in-michigan-for-dogs.md`). They share near-identical titles and headers, creating internal keyword cannibalization.
3. **Animal Guide Duplication (15 Pairs)**:
   Articles exist as both `best-zoos-in-<state>-to-see-<animal>.md` and `zoos-in-<state>-to-see-<animal>.md`.
4. **Case-Insensitive Duplicate File**:
   Both `robots.txt` and `Robots.txt` exist in the root directory.

---

### 🟡 Broken Image References (12 Instances)
1. **Missing Image File Extension (9 posts)**:
   In 9 Michigan articles, the Indian Creek Zoo image is linked as `assets/images/zoos/indian-creek-zoo` without `.jpg`, rendering broken images.
2. **Mismatched Image Filenames (3 posts)**:
   The comparison posts reference files with hyphens (e.g. `hattiesburg-zoo-vs-tupelo-buffalo-park--zoo.jpg`), but the actual files in `assets/images/` contain ampersands (e.g. `hattiesburg-zoo-vs-tupelo-buffalo-park-&-zoo.jpg`).

---

### 🟡 Broken Internal Links & Search
1. **Author Link 404**:
   In `_layouts/post.html` (line 75), the author bio links to `<a href="/about-us">`. The site only has `/about-me`, resulting in a broken link on every post.
2. **Search Engine Non-Functional**:
   `assets/js/search.js` searches `/search_data.json` using `lunr.js`. However, no search page exists (`/search`), no `search_data.json` is generated, and `lunr.js` is not loaded.
3. **Empty Post in `_posts`**:
   `_posts/README.md` is a 1-byte placeholder file in the posts directory.

---

### 🟡 Template Boilerplate Cruft (Remnants from Previous Sites)
The repository was built from a WowThemes template originally configured for other projects:
- **Lawn Bowls Remnants**:
  - `contact.md` gives the contact email `reece@jackhighbowls.com`.
  - `_includes/useful.html` links to "Second Hand Woods Buyers Guide" and "Lawn Bowls Rules for Beginners".
  - `README.md` references `https://github.com/krussll/spencer-bowls`.
- **E-Commerce & Digital Downloads Remnants**:
  - `_includes/newsletter.html` form title is *"Sign up to get our weekly free guide to selling digital downloads"* with a placeholder WowThemes MailChimp action URL.
  - `_includes/newsletter-signup.html` submits to Sendinblue with a return URL pointing to `http://www.bfea.co.uk/success`.

---

### 🟡 SEO & Configuration Discrepancies
1. **Sitemap Domain Incomplete**:
   `_config.yml` defines `baseurl: "https://www.zoonear.me"` instead of `url: "https://www.zoonear.me"`. Because `sitemap.xml` generates `<loc>{{ site.url }}{{ post.url }}</loc>`, `<loc>` outputs relative URLs (`/<post-name>`) without the domain name.
2. **Missing Pages in Sitemap**:
   `sitemap.xml` only iterates through `site.posts`. All key landing pages (`/`, `/zoos-by-states`, `/about-me`, `/contact`, `/terms`, `/privacy`) are missing from the sitemap.
3. **Author Metadata Incomplete**:
   `_config.yml` has empty fields for author email, gravatar, and description, leading to empty schema attributes.

---

## 7. Recommended Action Plan for Agents

When implementing fixes or enhancements to this repository, follow this priority sequence:

### Phase 1: High Priority (Build & Rendering Fixes)
- [ ] **Fix 7 YAML Syntax Errors**: Escape internal quotes or switch to single-quoted strings in `_posts/` frontmatter so all 148 posts compile.
- [ ] **Fix 12 Broken Image Links**: Append `.jpg` to Indian Creek Zoo image paths and align comparison post image filenames with existing assets.
- [ ] **Fix Broken `/about-us` Link**: Update `_layouts/post.html` to point to `/about-me`.
- [ ] **Resolve Duplicate Double-Hyphen Posts**: Consolidate or delete the 3 duplicate files (`--zoo.md` and `--wolf-discovery-center.md`).

### Phase 2: Medium Priority (SEO, Canonical & Metadata Integrity)
- [ ] **Configure `url` and `baseurl` in `_config.yml`**: Set `url: "https://www.zoonear.me"` and `baseurl: ""`.
- [ ] **Update `sitemap.xml`**: Include static pages (`index.html`, `about-me.md`, `zoos-by-states.md`, legal pages) and ensure absolute URLs.
- [ ] **Consolidate Singular vs Plural Keyword Cannibalization**: Decide on canonical slugs for the 20 singular/plural pairs to strengthen page authority.
- [ ] **Delete Case-Insensitive Duplicate**: Remove duplicate `Robots.txt`.
- [ ] **Populate Author Info**: Add description and Gravatar hash in `_config.yml` to enhance E-E-A-T signals.

### Phase 3: Cleanup & Future Features
- [ ] **Purge Template Cruft**: Replace `reece@jackhighbowls.com` with a domain email, remove `_includes/useful.html` bowls links, and clean up the newsletter forms.
- [ ] **Functional Search or Removal**: Either implement Lunr.js with a search layout and JSON index, or remove the unused search form and script.
- [ ] **State Expansion**: Expand directory beyond the 5 "M" states to cover all 50 US states.
