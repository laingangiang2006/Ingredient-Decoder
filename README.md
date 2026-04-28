# Ingredient-Decoder

## Inspiration

If you look at the ingredients list of a skincare product, you'll most probably find chemical names
such as _Butylene Glycol_, _Phenoxyethanol_, and _Sodium Hyaluronate_, which are the names that are completely foreign to the majority of people, written in INCI (International Nomenclature of Cosmetic Ingredients), a standardised format which hardly anyone understands except for a scientist.

One will often decide to Google the ingredients individually, but even then one can easily drift
into an ocean of contradictory beauty blogs, sponsored articles, and forum debates. A person with
sensitive skin, a known allergy, or a specific concern like acne or ageing might have to spend up to
**45 minutes per product** and in the end, will probably still have some doubts.

That is where we intervened. **Ingredient Decoder** was initiated from a very simple question: what
if, after pasting an ingredient list, you could instantly learn what is inside, what each ingredient
is for, whether these are backed by scientific proof, and most notably, whether the product is right
for _your_ skin?

---

## What it does

Ingredient Decoder is a web app where you paste any cosmetic product's ingredient list and receive a
full, personalised breakdown in plain English — in about 10 seconds.

**Key features:**

- **Ingredient Parser**: paste a raw INCI list and have it automatically split, cleaned, and identified
  ingredient by ingredient
- **Plain-English Explanations**: each ingredient gets a one-sentence summary of its role (moisturiser,
  exfoliant, preservative, fragrance, etc.)
- **Evidence Ratings**: a simple indicator of whether the ingredient is well-researched, has mixed
  evidence, or is mostly marketing
- **Red Flag Alerts**: automatic highlights for known irritants, common allergens, comedogenic
  ingredients, and potentially harmful preservatives
- **Ingredient Conflict Detector**: flags combinations known to cause irritation or cancel each other
  out (e.g. retinol + AHAs)
- **Skin Type Personaliser**: select your skin type and concerns upfront; the suitability score adjusts
  accordingly
- **Top Ingredients Summary**: the most impactful ingredients are surfaced first
- **Save & Compare** — save multiple products and compare their profiles side by side, no account
  required

The suitability score is computed as a weighted deduction from a perfect baseline:

$$S = 100 - \sum_{i=1}^{n} w_i \cdot f_i$$

where \\( w_i \\) is the severity weight of flagged ingredient \\( i \\) (scaled by concern type and
skin type match), and \\( f_i \in \{0, 1\} \\) indicates whether the ingredient was detected. A score
below 70 triggers a warning.

---

## How we built it

The project is built as a single-page frontend application with no backend or server required:

- **Frontend:** React (via CDN), Tailwind CSS, and Babel standalone for a clean, responsive UI
  written entirely in a single HTML file
- **Logic:** A curated ingredient dictionary maps known INCI names to their function, evidence rating,
  and warning flags. When a user pastes an ingredient list, it is split, cleaned, and looked up
  against this dictionary ingredient by ingredient
- **Scoring:** Each flagged ingredient carries a severity weight; the suitability score is computed
  client-side as a weighted deduction from 100
- **Storage:** `localStorage` persists saved products and user skin profiles entirely in the browser —
  no account, no login, no friction
- **Hosting:** Vercel for fast, free deployment

The entire app runs client-side. There is no API call, no server, and no database — just a single
HTML file that works instantly in any browser.

---

## Challenges we ran into

**1. Real-world ingredient lists are messy.** INCI format has a standard, but brands don't always
follow it. Lists come with inconsistent separators, spelling mistakes, trademarked ingredient names,
and mixed languages. Parsing them reliably into clean individual ingredient tokens required careful
string handling and normalisation logic.

**2. Building a meaningful ingredient dictionary.** Without an AI layer, the quality of the analysis
depends entirely on the curated data. Sourcing accurate function descriptions, evidence ratings, and
warning flags for hundreds of common INCI ingredients — cross-referenced against dermatological
literature and the EU's CosIng database — was the most time-consuming part of the build.

**3. Conflict detection without false positives.** Ingredient interaction data isn't neatly organised
anywhere publicly. We curated a conflict list manually and erred on the side of softer warnings
rather than hard blocks to avoid alarming users unnecessarily.

**4. Suitability scoring is inherently subjective.** Skin compatibility is not a solved science. We
framed the score as a _rough guide_, not a medical verdict, and surfaced the reasoning behind every
deduction so users can make their own call.

---

## Accomplishments that we're proud of

- Shipping a **fully functional, zero-infrastructure app** in a single HTML file
- Building a clean, intuitive UI that makes ingredient analysis feel effortless
- A save-and-compare system that works entirely in the browser with no account required
- A conflict detection system that catches ingredient combinations most users would never know to
  look for

---

## What we learned

- **Data quality is everything.** Without AI, the ingredient dictionary is the product. Investing time
  in accurate, well-sourced ingredient data made the difference between a toy and a useful tool.
- **Transparency builds trust.** Showing users _why_ an ingredient was flagged, not just _that_ it
  was, changed how the tool felt. It went from a black box to a knowledgeable friend.
- **Constraints force clarity.** Building without a backend or API pushed us to think carefully about
  what the app truly needed to do, and cut everything else.

---

## What's next for Ingredient Decoder

- **AI integration**: connect the Claude API to deliver dynamic, real-time analysis beyond the
  curated dictionary, handling any ingredient including obscure or newly introduced ones
- **Browser extension**: decode ingredients directly on retailer pages (Sephora, iHerb) without
  copying and pasting
- **Barcode / photo scan**: point your camera at a product label and have it parsed automatically
- **Personalised ingredient history**: track which ingredients appear across everything you've used
  and spot recurring irritants
- **Community flagging**: let users upvote or correct ingredient descriptions over time
- **Formulator mode**: a reverse tool for cosmetic formulators to check a formula for conflicts and
  compliance before production

---

## Target Users

- **Skincare beginners** overwhelmed by ingredient lists and unsure what to look for
- **People with sensitive skin or allergies** who need to quickly screen products for specific irritants
- **Skincare enthusiasts** who want to go
