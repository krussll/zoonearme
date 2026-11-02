# Zoo Near Me (`zoonear.me`) — Full SEO Audit & Strategic Action Plan

**Repository**: `krussll/zoonearme`  
**Live Site**: [https://www.zoonear.me](https://www.zoonear.me)  
**Date of Audit**: November 2026  
**Auditor**: Antigravity Assistant  

---

## Executive Summary & Audit Scorecard

**Zoo Near Me** ([zoonear.me](https://www.zoonear.me)) has a strong content foundation with **148 in-depth articles** averaging **1,520 words per post** (reaching up to 4,241 words). The site is architected around high-intent, long-tail search queries (pairwise zoo comparisons, state guides, accessibility, dog/service animal rules, and species spotting).

However, severe technical blockages, duplicate keyword cannibalization, invalid sitemap formatting, and template bugs are currently capping organic search performance, preventing crawl efficiency, and depressing SERP rankings.

| SEO Pillar | Health Rating | Current Status & Impact Summary |
| :--- | :---: | :--- |
| **Content Depth & Intent** | **Strong (8.5/10)** | Substantial editorial coverage, clear user search intent, long-form articles, excellent comparison data points. |
| **URL Architecture** | **Strong (8.0/10)** | Flat, keyword-focused permalinks (`/:title`), no ugly parameters or date stamps. |
| **Crawlability & Indexability** | **Critical (3.5/10)** | 7 posts completely fail to build due to YAML syntax errors (causing 404s); `sitemap.xml` outputs relative URLs (`<loc>/slug</loc>`) which Google rejects; missing sitemap directive in `robots.txt`. |
| **Keyword Architecture** | **Poor (4.0/10)** | 70 out of 148 posts are locked in direct keyword cannibalization (20 pairs of identical singular vs plural posts; 15 pairs of duplicate animal guides). |
| **On-Page & Meta Tags** | **Moderate (5.0/10)** | Title tags suffer extreme length bloat (165+ characters, truncated in SERPs); homepage title is duplicated; zero Open Graph or Twitter Card tags. |
| **Structured Data (Schema)** | **Poor (4.0/10)** | Uses outdated Microdata with insecure `http://` schema URLs; missing required Google Rich Snippet properties (`mainEntityOfPage`, `logo`); no modern JSON-LD; no `BreadcrumbList` or `FAQPage` schema. |
| **E-E-A-T & Trust Signals** | **Moderate (5.0/10)** | Author bio link `/about-us` produces a 404 on 138 pages; author bio description and Gravatar hash are empty in config; residual template cruft from unrelated sites exists. |
| **Internal Linking & Navigation** | **Moderate (5.5/10)** | State pillar hubs link to 404s; sidebar only links to Michigan; comparison pages lack contextual cross-linking; search bar is non-functional. |

---

## Part 1: Positive Elements of the Site (What Is Working Well)

### 1. High Content Volume & Genuine Editorial Depth
- **Long-Form Coverage**: Across 148 content posts, the average length is **1,520 words**, with flagship posts exceeding **4,200 words**. This gives search engines substantial semantic context and topical relevance.
- **Data-Rich At-a-Glance Comparisons**: Comparison articles feature structured data tables detailing zoo acreage, dwell times, adult/child ticket pricing, family-of-four total costs, parking rates, and outside food policies.
- **Specific Practical Value**: Rather than generic promotional blurbs, the articles answer real visitor questions: ADA wheelchair grading, woodchip vs. paved terrain, stroller rentals, and service-dog regulations under ADA Title III.

### 2. High-Intent, Long-Tail Query Mapping
The site targets high-intent commercial and informational search clusters that major travel sites often neglect:
- **Head-to-Head Comparisons** (68 posts): Captures users in the final decision-making phase (e.g., `Detroit Zoo vs Binder Park Zoo`, `Como Park Zoo vs Minnesota Zoo`).
- **Niche Demographic Guides** (40 posts): Captures high-converting queries for families with toddlers, disabled visitors, and pet owners.
- **Species Spotting** (35 posts): Captures animal-specific interest (e.g., *where to see capybaras in Michigan*).
- **State Pillars** (5 posts): Serves as comprehensive regional directory hubs.

### 3. Clean, Modern URL Architecture
- Configured with `permalink: /:title` in `_config.yml`, producing clean, human-readable URLs without folder clutter or date stamps (e.g., `/detroit-zoo-vs-binder-park-zoo`).
- Follows Google’s URL structure recommendations: hyphen-delimited, lowercase, keyword-rich.

### 4. Clean Heading Hierarchy on Posts
- In `_layouts/post.html`, the article title is rendered inside a single `<h1>`.
- The Markdown bodies avoid duplicate `#` tags and use clean `##` and `###` heading increments throughout all 148 posts.

### 5. Static Site Speed & Zero Server Latency
- Built with Jekyll on **GitHub Pages**, providing instantaneous Time to First Byte (TTFB), static HTML serving from GitHub’s global Fastly CDN, and zero server-side database bottlenecks.
- Responsive design powered by Bootstrap with mobile viewport meta tag configured.
- Image lazy loading is already implemented via `lazysizes.min.js`.

### 6. Baseline E-E-A-T Foundation & Legal Pages
- Required trust pages are published: `about-me.md`, `contact.md`, `privacy.md`, `terms.md`, and `affiliate-disclaimer.md`.
- Google Tag Manager (GA4) and Google Search Console verification tokens are properly embedded.

---

## Part 2: Detailed SEO Audit (Deficiencies & Bottlenecks)

### 1. Technical SEO & Crawlability Deficiencies

#### 🔴 Critical: 7 Posts Fail to Build (YAML Parsing Exceptions)
Jekyll throws unhandled YAML parsing exceptions during `jekyll build` on **7 posts** due to unescaped double quotes inside frontmatter strings:
- `_posts/2023-06-30-best-zoos-in-mississippi-to-see-capybaras.md` (unescaped `"water pig,"`)
- `_posts/2023-06-30-best-zoos-in-missouri-for-dogs.md` (unescaped `"Show-Me"`)
- `_posts/2023-06-30-como-park-vs-lake-superior-zoo.md` (unescaped `"behind the scenes"`)
- `_posts/2023-06-30-deyoung-family-zoo-vs-indian-creek-zoo.md` (double-double-quotes `""Embark...`)
- `_posts/2023-06-30-garlyn-zoo-wildlife-park-vs-roscommon-zoo.md` (unescaped `"no public transport"`)
- `_posts/2023-06-30-garlyn-zoo-wildlife-park-vs-wilderness-trails-zoo.md` (double-double-quotes `""Explore...`)
- `_posts/2023-06-30-john-ball-zoo-vs-roscommon-zoo.md` (double-double-quotes `""Explore...`)

> [!CAUTION]
> **SEO Impact**: Because Jekyll encounters a fatal YAML exception on these files, **it completely skips compiling them into HTML**. They do not exist on the live site, producing **404 errors** whenever linked from state pillar guides.

#### 🔴 Critical: Invalid Relative URLs in `sitemap.xml`
In `sitemap.xml`:
```liquid
<loc>{{ site.url }}{{ post.url }}</loc>
```
Because `site.url` is **not defined** in `_config.yml`, `site.url` evaluates to an empty string. The generated sitemap outputs:
```xml
<loc>/zoos-in-montana-to-see-tigers</loc>
```
> [!WARNING]
> According to the official [sitemaps.org protocol](https://www.sitemaps.org/protocol.html) and Google Search Central requirements, all `<loc>` entries **must be fully qualified absolute URLs** beginning with `https://`. Search engines cannot parse relative URLs in sitemaps and flag the sitemap as invalid. Furthermore, key landing pages (`/`, `/zoos-by-states`, `/about-me`, etc.) are completely missing from the sitemap.

#### 🔴 Critical: Broken Robots Directives & Duplicate File
- In `robots.txt`, there is **no `Sitemap:` directive**.
- A duplicate file, `Robots.txt` (capital R), exists in the root directory. On case-sensitive web servers and git environments, this can lead to unexpected crawler behavior.

#### 🟡 Configuration Conflict: `baseurl` vs `url`
In `_config.yml`:
```yaml
baseurl: "https://www.zoonear.me"
```
In Jekyll:
- `url` is meant for the protocol and domain (`https://www.zoonear.me`).
- `baseurl` is meant for the subdirectory path (which should be `""` for a custom domain).
Misconfiguring `baseurl` as the domain causes Liquid URL filters (`prepend: site.baseurl | prepend: site.url` in `feed.xml`) to duplicate or break paths.

#### 🟡 Duplicate Permalinks & Collision Warnings
Three post pairs exist with both single and double hyphens in the filename:
1. `hattiesburg-zoo-vs-tupelo-buffalo-park--zoo.md` vs `...-tupelo-buffalo-park-zoo.md`
2. `the-jackson-zoo-vs-tupelo-buffalo-park--zoo.md` vs `...-tupelo-buffalo-park-zoo.md`
3. `zoomontana-vs-grizzly--wolf-discovery-center.md` vs `...-grizzly-wolf-discovery-center.md`

Jekyll generates identical permalinks for both, triggering build collision warnings where one file overwrites the other.

---

### 2. Keyword Architecture & Cannibalization

#### 🔴 Severe Self-Cannibalization: 35 Duplicate Pairs (70 Articles)
Nearly **half of the site’s posts** are in direct keyword competition with each other:

1. **Singular vs. Plural Duplication (20 pairs)**:
   - Example: `best-zoo-in-michigan-for-dogs.md` vs `best-zoos-in-michigan-for-dogs.md`.
   - Both pages have the exact same title tag: `"The Best Zoo In Michigan, USA For Dogs"`.
   - Both rank for the exact same query, splitting backlinks, crawl budget, and organic rank signals.
2. **Animal Guide Duplication (15 pairs)**:
   - Example: `zoos-in-michigan-to-see-capybaras.md` vs `best-zoos-in-michigan-to-see-capybaras.md`.
   - Both have identical titles: `"The Best Zoos In Michigan, USA To See Capybaras"`.
3. **Geographic Limitation**:
   - The site only covers **5 states** (Michigan, Minnesota, Mississippi, Missouri, Montana). A visitor or crawler searching for the remaining 45 US states hits dead ends.

---

### 3. On-Page SEO & Metadata Flaws

#### 🟡 Severe Title Tag Bloat
In `_layouts/default.html`:
```liquid
<title>{{ page.title }} | {{site.title_suffix}}</title>
```
Where `site.title_suffix` in `_config.yml` is:
`"Zoo Near Me | Online Guides To The Best Zoos In The US State-By-State"` (69 characters alone!)

When combined on post pages, the title tag reaches **165+ characters**:
> `Which Is Best: Detroit Zoo Or Binder Park Zoo. A Guide To Which Is The Best Zoo In Michigan, USA | Zoo Near Me | Online Guides To The Best Zoos In The US State-By-State`

- Google truncates desktop titles at ~600px (~60 characters).
- The brand name `"Zoo Near Me"` and key terms get cut off entirely.
- On the homepage, the phrase `"Online Guides To The Best Zoos In The US State-By-State"` repeats twice in the same `<title>` tag.

#### 🟡 Meta Description Inconsistencies & Empty Meta Tags
- On `index.html`, the meta description is just the generic title string: `"Online Guides To The Best Zoos In The US State-By-State"`.
- `_layouts/default.html` outputs an empty `<meta name="keyword" content="">` on all posts without tags. (Meta keywords are ignored by Google and should be removed).

#### 🟡 Missing `<h1>` on Pages Using `layout: page`
In `_layouts/page.html`:
```html
<h2><span>{{ page.title }}</span></h2>
```
Static pages like `about-me.md` have **no `<h1>` tag at all**, violating heading hierarchy standards.

#### 🟡 Missing Open Graph & Twitter Cards
There are **zero Open Graph (`og:*`) and Twitter Card (`twitter:*`) tags** in `_layouts/default.html`. When articles are shared on social media, Pinterest, or messaging apps, no rich snippet, title, or thumbnail image appears.

#### 🟡 Broken Image References & CLS Vulnerabilities
1. **Missing Extensions (9 posts)**: In Michigan demographic and animal posts, Indian Creek Zoo is referenced as `assets/images/zoos/indian-creek-zoo` without `.jpg`, rendering broken images.
2. **Relative Pathing without Leading Slash**: Markdown images referenced as `assets/images/...` instead of `/assets/images/...` fail when loaded from subpath URLs.
3. **Missing Width/Height Attributes**: In `_includes/postbox.html`, images lack `width` and `height`, causing layout shifts (Cumulative Layout Shift - CLS) while pages load.

---

### 4. Structured Data & Semantic SEO

#### 🟡 Outdated Microdata & Incomplete Schemas
`_layouts/post.html` implements inline Schema.org Microdata:
- Uses `http://schema.org/BlogPosting` instead of the secure `https://schema.org/BlogPosting`.
- Fails Google's Rich Result validation requirements:
  - Missing `mainEntityOfPage`.
  - Incomplete `publisher` (missing required `logo` object).
  - Empty `author.description`.
  - In line 40, `<time date="..." itemprop="dateModified">` uses the non-standard attribute `date` instead of `datetime`.
- **Missing JSON-LD**: Google officially recommends **JSON-LD** over Microdata for search indexing and rich snippets.
- **Missing High-Value Schema Types**:
  - `BreadcrumbList`: Enhances search snippet navigation paths in Google SERPs.
  - `FAQPage`: Generates rich expandable FAQ accordions directly in search results.
  - `TouristAttraction` / `Zoo`: Could semantically mark up zoo addresses, opening hours, ticket pricing, and geo-coordinates.

---

### 5. E-E-A-T & Trust Signal Deficiencies

#### 🔴 Broken Author Link (Sitewide 404)
In `_layouts/post.html`:
```html
<a class="link-dark" href="/about-us"><span itemprop="name">{{ author.display_name }} </span></a>
```
The site only has `/about-me`. This creates a **broken link on 138 pages** directly from the author byline, which negatively impacts user experience and trust raters.

#### 🟡 Empty Author Config
In `_config.yml`:
```yaml
authors:
  reece:
    name: Reece
    display_name: Reece Williams 
    gravatar:
    email: ""
    description: ""
```
Because `description` and `gravatar` are empty:
- The author box renders an empty description.
- The avatar defaults to a generic mystery-man placeholder icon via Gravatar.
- Under Google's Search Quality Rater Guidelines, content reviewing safety, disability access, and pet policies requires demonstrable first-hand experience and author transparency.

#### 🟡 Unrelated Template Boilerplate Cruft
- `contact.md` lists the email `reece@jackhighbowls.com` (from a lawn bowls site).
- `_includes/useful.html` links to lawn bowls guides.
- `_includes/newsletter.html` advertises a guide to "selling digital downloads."

---

### 6. Internal Linking Architecture & Usability

#### 🟡 Broken Internal Links on Flagship Hubs
In `best-zoos-in-michigan.md`, `best-zoos-in-minnesota.md`, etc., links pointing to the 7 failed YAML posts lead to 404s.

#### 🟡 Underutilized Sidebar Navigation
In `_includes/recommended.html`, the sidebar only contains 2 links: "Best Zoos In Michigan" and "All States". The other 4 covered states are missing, and no related comparison or animal guides are linked.

#### 🟡 Non-Functional Search Feature
`assets/js/search.js` expects `/search_data.json` and `lunr.js`. However, no search page exists (`/search`), Lunr is not loaded, and the JSON index is not generated.

---

## Part 3: Prioritized Action Plan to Boost Search Performance

```
+-------------------------------------------------------------------------------+
|                        SEO ROADMAP TO SEARCH PERFORMANCE                      |
+-------------------------------------------------------------------------------+
|  PHASE 1 (Immediate - Day 1): Build Integrity, Indexation & Crawlability      |
|  - Fix 7 YAML frontmatter syntax errors (restore missing pages & stop 404s)   |
|  - Fix site.url & baseurl in _config.yml                                      |
|  - Generate fully qualified absolute XML sitemap & link in robots.txt         |
|  - Fix broken /about-us author link & delete duplicate Robots.txt             |
+-------------------------------------------------------------------------------+
                                        |
                                        v
+-------------------------------------------------------------------------------+
|  PHASE 2 (Week 1): On-Page, Metadata & Rich Snippets (JSON-LD)               |
|  - Revamp <title> structure (60 char max; eliminate title bloat)              |
|  - Implement Open Graph & Twitter Cards                                       |
|  - Migrate schema to modern JSON-LD (Article, Breadcrumbs, Organization)      |
|  - Fix broken image paths and append width/height to eliminate CLS            |
|  - Fix <h1> on static pages                                                   |
+-------------------------------------------------------------------------------+
                                        |
                                        v
+-------------------------------------------------------------------------------+
|  PHASE 3 (Weeks 2-3): Cannibalization Consolidation & Topic Clusters         |
|  - Consolidate 20 Singular vs Plural pairs (canonical/redirect strategy)     |
|  - Consolidate 15 duplicate Animal guide pairs                                |
|  - Build rich state hub navigation on zoos-by-states.md                       |
|  - Expand sidebar with dynamic category / state-specific contextual links     |
+-------------------------------------------------------------------------------+
                                        |
                                        v
+-------------------------------------------------------------------------------+
|  PHASE 4 (Week 4): E-E-A-T, Trust & Conversion                               |
|  - Update Reece Williams author profile with real bio and experience          |
|  - Purge legacy template cruft (lawn bowls email, irrelevant newsletters)     |
|  - Add FAQ sections with FAQPage schema to top 20 comparison posts            |
+-------------------------------------------------------------------------------+
                                        |
                                        v
+-------------------------------------------------------------------------------+
|  PHASE 5 (Ongoing): Content Scaling to All 50 US States                       |
|  - Expand state coverage beyond "M" states (Texas, California, Florida, etc.) |
|  - Programmatic zoo directory pages with Zoo / Attraction schema              |
+-------------------------------------------------------------------------------+
```

---

### Phase 1: Immediate Critical Fixes (Day 1 - Quick Wins)

#### Step 1.1: Fix the 7 YAML Frontmatter Syntax Errors
Replace unescaped internal double quotes with single quotes or escape them with `\"` in:
- `_posts/2023-06-30-best-zoos-in-mississippi-to-see-capybaras.md`
- `_posts/2023-06-30-best-zoos-in-missouri-for-dogs.md`
- `_posts/2023-06-30-como-park-vs-lake-superior-zoo.md`
- `_posts/2023-06-30-deyoung-family-zoo-vs-indian-creek-zoo.md`
- `_posts/2023-06-30-garlyn-zoo-wildlife-park-vs-roscommon-zoo.md`
- `_posts/2023-06-30-garlyn-zoo-wildlife-park-vs-wilderness-trails-zoo.md`
- `_posts/2023-06-30-john-ball-zoo-vs-roscommon-zoo.md`

*Result: Immediately generates 7 missing URLs and resolves 10 internal 404 errors.*

#### Step 1.2: Correct Jekyll Site URL Configuration
In `_config.yml`:
```yaml
# Update URL and BaseURL configuration
url: "https://www.zoonear.me"
baseurl: ""
```

#### Step 1.3: Generate a Valid XML Sitemap with All Site Pages
Replace `sitemap.xml` with a standard, fully qualified sitemap indexing both pages and posts:
```xml
---
layout: null
---
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <!-- Core Static Pages -->
  <url>
    <loc>{{ site.url }}/</loc>
    <changefreq>daily</changefreq>
    <priority>1.0</priority>
  </url>
  <url>
    <loc>{{ site.url }}/zoos-by-states</loc>
    <changefreq>weekly</changefreq>
    <priority>0.9</priority>
  </url>
  <url>
    <loc>{{ site.url }}/about-me</loc>
    <changefreq>monthly</changefreq>
    <priority>0.5</priority>
  </url>
  <url>
    <loc>{{ site.url }}/contact</loc>
    <changefreq>monthly</changefreq>
    <priority>0.4</priority>
  </url>
  <!-- Content Posts -->
  {% for post in site.posts %}
  <url>
    <loc>{{ site.url }}{{ post.url }}</loc>
    <lastmod>{{ post.last_modified_at | default: post.date | date_to_xmlschema }}</lastmod>
    <changefreq>monthly</changefreq>
    <priority>{% if post.categories contains "state" %}0.9{% else %}0.8{% endif %}</priority>
  </url>
  {% endfor %}
</urlset>
```

#### Step 1.4: Update `robots.txt` & Delete Duplicate `Robots.txt`
In `robots.txt`:
```txt
User-agent: *
Allow: /

Sitemap: https://www.zoonear.me/sitemap.xml
```
Delete the duplicate file `Robots.txt`.

#### Step 1.5: Fix Author Link 404
In `_layouts/post.html`:
```diff
-<a class="link-dark" href="/about-us"><span itemprop="name">{{ author.display_name }} </span></a>
+<a class="link-dark" href="{{ site.baseurl }}/about-me"><span itemprop="name">{{ author.display_name }} </span></a>
```

#### Step 1.6: Remove Double-Hyphen Duplicate Files
Delete the 3 duplicate files with `--` in `_posts/`:
- `2023-06-30-hattiesburg-zoo-vs-tupelo-buffalo-park--zoo.md`
- `2023-06-30-the-jackson-zoo-vs-tupelo-buffalo-park--zoo.md`
- `2023-06-30-zoomontana-vs-grizzly--wolf-discovery-center.md`

---

### Phase 2: On-Page, Metadata & Structured Data (Week 1)

#### Step 2.1: Optimize Title Tags (Under 60 Characters)
Update `_layouts/default.html` to avoid suffix repetition and title bloat:
```liquid
{% if page.url == "/" %}
<title>Zoo Near Me | Best US Zoos & Wildlife Guides State-by-State</title>
{% else %}
<title>{{ page.title | truncate: 50 }} | Zoo Near Me</title>
{% endif %}
```

#### Step 2.2: Add Open Graph & Twitter Cards
In `_layouts/default.html` inside `<head>`:
```html
<!-- Open Graph / Facebook -->
<meta property="og:type" content="{% if page.layout == 'post' %}article{% else %}website{% endif %}">
<meta property="og:url" content="{{ site.url }}{{ page.url }}">
<meta property="og:title" content="{% if page.title %}{{ page.title }} | Zoo Near Me{% else %}{{ site.name }}{% endif %}">
<meta property="og:description" content="{% if page.description %}{{ page.description }}{% else %}{{ site.description }}{% endif %}">
<meta property="og:image" content="{{ site.url }}/{% if page.image %}{{ page.image }}{% else %}assets/images/zoonearme.png{% endif %}">

<!-- Twitter Cards -->
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="{% if page.title %}{{ page.title }} | Zoo Near Me{% else %}{{ site.name }}{% endif %}">
<meta name="twitter:description" content="{% if page.description %}{{ page.description }}{% else %}{{ site.description }}{% endif %}">
<meta name="twitter:image" content="{{ site.url }}/{% if page.image %}{{ page.image }}{% else %}assets/images/zoonearme.png{% endif %}">
```

#### Step 2.3: Modernize Schema Markup to JSON-LD
In `_layouts/post.html`, remove obsolete inline Microdata and add a rich JSON-LD script block:
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@graph": [
    {
      "@type": "Article",
      "@id": "{{ site.url }}{{ page.url }}#article",
      "isPartOf": {
        "@type": "WebPage",
        "@id": "{{ site.url }}{{ page.url }}"
      },
      "headline": {{ page.title | jsonify }},
      "description": {{ page.description | jsonify }},
      "mainEntityOfPage": "{{ site.url }}{{ page.url }}",
      "datePublished": "{{ page.date | date_to_xmlschema }}",
      "dateModified": "{{ page.last_modified_at | default: page.date | date_to_xmlschema }}",
      "image": "{{ site.url }}/{{ page.image }}",
      "author": {
        "@type": "Person",
        "name": "{{ author.display_name }}",
        "url": "{{ site.url }}/about-me"
      },
      "publisher": {
        "@type": "Organization",
        "name": "Zoo Near Me",
        "url": "{{ site.url }}",
        "logo": {
          "@type": "ImageObject",
          "url": "{{ site.url }}/assets/images/zoonearme.png"
        }
      }
    },
    {
      "@type": "BreadcrumbList",
      "@id": "{{ site.url }}{{ page.url }}#breadcrumb",
      "itemListElement": [
        {
          "@type": "ListItem",
          "position": 1,
          "name": "Home",
          "item": "{{ site.url }}/"
        },
        {
          "@type": "ListItem",
          "position": 2,
          "name": "Zoos By State",
          "item": "{{ site.url }}/zoos-by-states"
        },
        {
          "@type": "ListItem",
          "position": 3,
          "name": {{ page.title | jsonify }},
          "item": "{{ site.url }}{{ page.url }}"
        }
      ]
    }
  ]
}
</script>
```

#### Step 2.4: Fix Broken Image References & CLS
1. Fix Indian Creek Zoo image links in all 9 posts:
   `assets/images/zoos/indian-creek-zoo` $\rightarrow$ `/assets/images/zoos/indian-creek-zoo.jpg`
2. Add explicit `width="600" height="400"` to image tags in `_includes/postbox.html` to eliminate Cumulative Layout Shift (CLS).

#### Step 2.5: Fix `<h1>` on `_layouts/page.html`
```diff
-<div class="section-title">
-    <h2><span>{{ page.title }}</span></h2>
+<div class="section-title">
+    <h1><span>{{ page.title }}</span></h1>
```

---

### Phase 3: Cannibalization Consolidation & Cluster Architecture (Weeks 2-3)

#### Step 3.1: Consolidate the 20 Singular vs. Plural Duplicate Pairs
Choose the plural version (e.g., `best-zoos-in-michigan-for-dogs`) as the primary canonical URL because user search intent for directories favors the plural.
- **Merge Content**: Migrate any unique paragraphs, zoo reviews, or visitor tips from the singular post into the plural post.
- **Redirect / Canonicalize**: Use the `jekyll-redirect-from` plugin or add frontmatter canonical redirects so the singular URL 301 redirects to the plural master URL.
- **Repeat across all 20 pairs**:
  - `best-zoo-in-michigan-for-disability-access` $\rightarrow$ `best-zoos-in-michigan-for-disability-access`
  - `best-zoo-in-michigan-for-families` $\rightarrow$ `best-zoos-in-michigan-for-families`
  - `best-zoo-in-michigan-for-toddlers` $\rightarrow$ `best-zoos-in-michigan-for-toddlers`
  - (and the corresponding pairs across Minnesota, Mississippi, Missouri, and Montana).

#### Step 3.2: Consolidate the 15 Duplicate Animal Guide Pairs
- Standardize on `best-zoos-in-<state>-to-see-<animal>`.
- 301 redirect `zoos-in-<state>-to-see-<animal>` to the `best-zoos-in-...` version.

#### Step 3.3: Upgrade `zoos-by-states.md` into a Grand Hub
Currently, this page only contains an empty loop over 5 posts.
- Add an intro section explaining the directory mission.
- Provide a clean A–Z state list linking to existing state pillars (Michigan, Minnesota, Mississippi, Missouri, Montana) and marking future states as "Coming Soon".
- Add links to sub-pillars (Family Guides, Toddler Guides, Accessibility Guides).

#### Step 3.4: Revamp `_includes/recommended.html`
Expand the sidebar links to include:
- All 5 active state pillars (Michigan, Minnesota, Mississippi, Missouri, Montana).
- Popular zoo comparisons.
- Accessibility & family guides.

---

### Phase 4: E-E-A-T & Trust Signal Fortification (Week 4)

#### Step 4.1: Populate Author Metadata in `_config.yml`
```yaml
authors:
  reece:
    name: Reece
    display_name: Reece Williams
    gravatar: "d41d8cd98f00b204e9800998ecf8427e" # Or local image asset
    email: "contact@zoonear.me"
    description: "Wildlife enthusiast and travel writer dedicated to helping families discover accessible, budget-friendly zoological parks and wildlife sanctuaries across the United States."
```

#### Step 4.2: Purge Legacy Template Cruft
- Update `contact.md` with a branded email address (e.g., `contact@zoonear.me`) instead of the lawn bowls email `reece@jackhighbowls.com`.
- Delete `_includes/useful.html` (unreferenced bowls guide).
- Remove or redesign `_includes/newsletter.html` to offer a zoo-focused newsletter rather than a digital downloads guide.

#### Step 4.3: Implement FAQ Sections on Top Comparison Posts
Add 3–5 FAQs to high-traffic comparison posts with `FAQPage` JSON-LD schema:
- *Which zoo is cheaper for families?*
- *Which zoo has more animals?*
- *Can you bring outside food into Binder Park Zoo or Detroit Zoo?*
*Benefit: FAQPage structured data enables Google to award expandable accordion rich snippets directly on search result pages.*

---

### Phase 5: Long-Term Scaling (All 50 States)

Once the core technical debt and cannibalization issues are resolved:
1. **Target High-Volume Zoo States Next**:
   - Ohio (Columbus Zoo, Cincinnati Zoo, Toledo Zoo, Cleveland Metroparks Zoo).
   - California (San Diego Zoo, San Diego Safari Park, LA Zoo, Oakland Zoo).
   - Florida (Zoo Miami, Tampa Zoo at Lowry Park, Disney Animal Kingdom).
   - Texas (Fort Worth Zoo, Dallas Zoo, Houston Zoo, San Antonio Zoo).
2. **Implement a Dedicated Zoo Listing Layout**:
   - Create individual zoo profile pages (e.g. `/zoos/detroit-zoo`) using `TouristAttraction` schema.
   - Aggregate reviews, ticket prices, opening hours, and direct links to comparisons.

---

## Action Plan Checklist Summary

```markdown
### Phase 1: Immediate Critical Fixes (Day 1)
- [x] Fix 7 unescaped quotes in `_posts/` frontmatter (resolves 404s and build failures)
- [x] Set `url: "https://www.zoonear.me"` and `baseurl: ""` in `_config.yml`
- [x] Generate fully qualified absolute XML sitemap in `sitemap.xml`
- [x] Add `Sitemap:` directive to `robots.txt` and delete duplicate `Robots.txt`
- [x] Update author link from `/about-us` to `/about-me` in `_layouts/post.html`
- [x] Remove 3 duplicate double-hyphen files in `_posts/`

### Phase 2: On-Page & Schema (Week 1)
- [x] Truncate and optimize title tags in `_layouts/default.html` (under 60 chars)
- [x] Add Open Graph and Twitter Card tags to `<head>`
- [x] Implement Article and BreadcrumbList JSON-LD in `_layouts/post.html`
- [x] Fix Indian Creek Zoo missing `.jpg` extensions and add image dimensions in `postbox.html`
- [x] Change `<h2>` to `<h1>` in `_layouts/page.html`

### Phase 3: Cannibalization & Internal Linking (Weeks 2-3)
- [x] Merge and 301-redirect 20 singular posts into plural equivalents
- [x] Merge and 301-redirect 15 duplicate animal guides
- [x] Redesign `zoos-by-states.md` with complete state directory navigation
- [x] Expand `_includes/recommended.html` sidebar with multi-state and category links

### Phase 4: E-E-A-T & Trust (Week 4)
- [x] Add real author bio and avatar in `_config.yml`
- [x] Clean up lawn bowls email in `contact.md` and remove obsolete includes
- [x] Add FAQ accordions with FAQPage schema on top comparison articles
```
