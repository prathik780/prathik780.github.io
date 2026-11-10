# Website Architecture & Editing Guide

This guide explains how your portfolio website ([index.html](file:///c:/Users/pgunreddy3/Documents/GitHub/prathik780.github.io/index.html)) is built and how you can easily edit every part of it (bio, projects, publications, skills, photos, and links).

---

## 1. High-Level Architecture

The website is a **single-file, zero-build static application** designed for **GitHub Pages**.
- **No Node.js or build steps required**: You can edit [index.html](file:///c:/Users/pgunreddy3/Documents/GitHub/prathik780.github.io/index.html) in any text editor, save it, and push to GitHub.
- **Styling**: Uses Tailwind CSS via CDN (`cdn.tailwindcss.com`) with custom color extensions (`gtgold` Georgia Tech Gold and `navy`).
- **Icons**: Font Awesome v6 (`cdnjs.cloudflare.com`).
- **Fonts**: Inter, Plus Jakarta Sans, and JetBrains Mono loaded from Google Fonts.
- **Data-Driven Architecture**: 95% of the content (text, metrics, project case studies, publications, skills, timeline) is driven by a single JavaScript object:
  ```javascript
  const PORTFOLIO_DATA = { ... };
  ```
  This means **you rarely need to touch the HTML layout**. When you update `PORTFOLIO_DATA` in the `<script>` tag near the bottom of [index.html](file:///c:/Users/pgunreddy3/Documents/GitHub/prathik780.github.io/index.html), the page re-renders everything automatically on load.

---

## 2. Where to Edit Content (`PORTFOLIO_DATA`)

Open [index.html](file:///c:/Users/pgunreddy3/Documents/GitHub/prathik780.github.io/index.html) and jump to line ~`715` (search for `const PORTFOLIO_DATA =`).

### A. Personal Info, Bio, and Social Links (`profile`)
Found in `PORTFOLIO_DATA.profile`:
```javascript
profile: {
  name: "Prathik Reddy Gunreddy",
  navSubtitle: "Ph.D. Candidate | Georgia Tech",
  statusBadge: "Available for Industry R&D Roles • May 2027",
  headlineSubtitle: "Ph.D. Candidate in Mechanical Engineering at Georgia Tech | Vibro-Acoustic Metamaterials, Structural Dynamics & EV Systems Architecture",
  bio: "Bridging fundamental wave physics, high-fidelity finite element modeling...",
  location: "Atlanta, GA",
  email: "prathik.gunreddy@gatech.edu",
  cvUrl: "Prathik_Gunreddy_CV.pdf", // Path to your CV file
  profileImage: "", // Leave blank to show the animated technical metamaterial graphic, or set to e.g. "assets/headshot.jpg"
  socials: [
    { label: "Google Scholar", icon: "fa-brands fa-google-scholar", url: "https://scholar.google.com/..." },
    { label: "LinkedIn", icon: "fa-brands fa-linkedin-in", url: "https://www.linkedin.com/in/..." },
    { label: "GitHub", icon: "fa-brands fa-github", url: "https://github.com/prathik780" },
    { label: "Email", icon: "fa-solid fa-envelope", url: "mailto:prathik.gunreddy@gatech.edu" }
  ]
}
```

#### How to Add Your Portrait Photo:
1. Place your image file (e.g. `headshot.jpg`) into this repository folder or create an `assets/` folder.
2. In `PORTFOLIO_DATA.profile`, set:
   ```javascript
   profileImage: "headshot.jpg", // or "assets/headshot.jpg"
   ```
3. When `profileImage` is populated, the site automatically hides the default metamaterial graphic and displays your photo. If you ever want to revert to the graphic, just set `profileImage: ""`.

---

### B. Impact Metric Cards (`metrics`)
These are the 4 summary highlight cards below the hero section:
```javascript
metrics: [
  {
    metric: "1st Place Overall",
    title: "EcoCAR EV Challenge",
    detail: "1st in Technical Presentation • 4 Years as System Design & Integration (SDI) Lead",
    icon: "fa-solid fa-trophy"
  },
  // ...
]
```
- You can change the number/statistic (`metric`), title, subtitle description, and FontAwesome icon.

---

### C. Featured Projects & Case Studies (`projects`)
Each project in `PORTFOLIO_DATA.projects` populates both the **grid card** and the **interactive full case study modal**:
```javascript
{
  id: "proj-hybrid-metamaterial",             // Unique identifier string
  title: "Planar Hybrid Vibro-Acoustic Metamaterials (Patent Pending)",
  category: "Vibro-Acoustics & Metamaterials", // Category matches filter tabs
  badge: "U.S. Patent Filed • Georgia Tech",  // Small badge on top of card
  summary: "Brief 1-2 sentence overview shown directly on the grid card...",
  
  // Case study modal fields:
  problem: "What fundamental engineering problem did this solve?...",
  approach: "How did you model, manufacture, or validate it?...",
  results: [
    "Bullet point 1 with measurable engineering outcome...",
    "Bullet point 2...",
  ],
  tags: ["COMSOL Multiphysics", "Vibro-Acoustics", "FEA"],
  diagramType: "srr-metamaterial",            // Built-in SVG type (see below)
  customImage: ""                             // Optional: set to e.g. "assets/cad_render.png" to replace the SVG
}
```

#### Categories Available:
- `"Vibro-Acoustics & Metamaterials"`
- `"EV Systems & Controls"`
- `"Analytical & ML Modeling"`
*(If you want to add a new category, add it here and in `categories` array in [`renderProjectFilterTabs`](file:///c:/Users/pgunreddy3/Documents/GitHub/prathik780.github.io/index.html#L1219)).*

#### Built-in SVG Diagrams (`diagramType`):
- `"srr-metamaterial"`: Split-ring resonator and wave vector schematic.
- `"lyriq-awd"`: Cadillac LYRIQ electric AWD chassis architecture.
- `"impedance-tube"`: 4-microphone impedance tube acoustic transfer function schematic.
- `"ml-inverse"`: Neural network surrogate model & bandgap optimization diagram.
- `""` (default): Powertrain & Simscape thermal plant block diagram.

---

### D. Publications & Intellectual Property (`publications`)
Each entry in `PORTFOLIO_DATA.publications` renders under the Publications section:
```javascript
{
  id: "pub-patent-1",
  type: "Patent",                     // "Patent", "Journal", or "Manuscript"
  badge: "U.S. Patent Application • Filed",
  title: "Metamaterial Structure Engineered for...",
  authors: "Prathik Reddy Gunreddy, Karim Sabra, Michael Leamy, et al.",
  venue: "United States Patent Application (Georgia Tech Research Corporation)",
  year: "2025",
  abstract: "Summary of patent or publication...",
  doiUrl: "https://doi.org/...",      // Link to paper, patent, or "#contact"
  pdfUrl: "papers/my_paper.pdf",      // Link to PDF or "#contact"
  bibtex: `@misc{gunreddy_patent_metamaterial,
  author       = {Gunreddy, Prathik Reddy and Sabra, Karim and Leamy, Michael},
  title        = {...},
  ...
}`
}
```
- Clicking **"Cite (BibTeX)"** automatically opens the formatted citation modal and lets visitors copy it with 1 click.

---

### E. Skills Matrix (`skills`)
Organized by domains:
```javascript
skills: [
  {
    domain: "Simulation & Numerical Modeling",
    subtitle: "Multiphysics FEA, Wave Dispersion & System Plant Modeling",
    icon: "fa-solid fa-cubes",
    items: [
      { name: "COMSOL Multiphysics (...)", level: "Expert" }, // level: "Expert" or "Advanced"
      { name: "MATLAB (...)", level: "Expert" }
    ]
  },
  // ...
]
```

---

### F. Experience & Education Timeline (`timeline`)
```javascript
timeline: [
  {
    role: "Ph.D. Candidate in Mechanical Engineering",
    organization: "Georgia Institute of Technology",
    location: "Atlanta, GA",
    period: "Expected May 2027",
    badge: "Graduate Research",
    highlights: [
      "Advised by Dr. Karim Sabra and Dr. Michael Leamy...",
      "Research focus: Hybrid vibro-acoustic and elastic metamaterials..."
    ]
  },
  // ...
]
```

---

## 3. How to Update Your CV PDF

1. Compile or export your CV as `Prathik_Gunreddy_CV.pdf` (or any file name).
2. Save it directly in the root of this repository.
3. In `PORTFOLIO_DATA.profile`:
   ```javascript
   cvUrl: "Prathik_Gunreddy_CV.pdf",
   ```
4. In [index.html](file:///c:/Users/pgunreddy3/Documents/GitHub/prathik780.github.io/index.html), the **"Download CV"** buttons in the navigation bar and modal are already configured to link to your CV.

---

## 4. HTML Page Layout Guide

If you ever need to tweak the HTML structure directly, here is the section breakdown:

| Lines (Approx) | Section ID | Description |
|---|---|---|
| `20-53` | `<script>` | Tailwind configuration (custom colors `gtgold`, `navy`, fonts) |
| `54-88` | `<style>` | Subtle grid pattern, scrollbar styles, `.card-lift` hover physics |
| `96-157` | `<header>` | Sticky navigation bar, brand monogram, theme switch, mobile drawer |
| `162-314` | `<section id="about">` | Hero section, bio, status indicator badge, profile card container |
| `309-313` | `#impact-metrics-grid` | 4 metric cards (populated by JS) |
| `318-360` | `<section id="projects">` | Featured projects filter tabs & 3-column project cards grid |
| `364-394` | `<section id="publications">` | Publications & patents list with filter pills |
| `399-419` | `<section id="skills">` | 2x2 technical arsenal skills matrix |
| `423-441` | `<section id="experience">` | Experience & education vertical timeline |
| `444-486` | `<footer id="contact">` | Executive contact card, copy email button, copyright |
| `489-565` | `#case-study-modal` | Full-screen interactive popup for project details |
| `568-587` | `#bibtex-modal` | BibTeX citation modal |
| `590-623` | `#cv-modal` | Curriculum Vitae modal & print button |
| `632-1490` | `<script>` | `PORTFOLIO_DATA`, rendering logic, theme toggle |

---

## 5. Previewing & Deploying Your Changes

### Previewing Locally
Because this is pure static HTML/JS, you can preview changes by:
- Simply double-clicking [index.html](file:///c:/Users/pgunreddy3/Documents/GitHub/prathik780.github.io/index.html) to open it in Chrome, Edge, or Safari, OR
- Running `python -m http.server 8000` in terminal and visiting `http://localhost:8000`.

### Deploying to GitHub Pages
Whenever you edit [index.html](file:///c:/Users/pgunreddy3/Documents/GitHub/prathik780.github.io/index.html) or add images/PDFs:
```bash
git add .
git commit -m "Update portfolio content"
git push origin main
```
GitHub Pages will automatically rebuild and deploy the site in ~60 seconds to:
**https://prathik780.github.io**
