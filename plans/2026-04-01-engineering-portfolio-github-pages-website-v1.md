# Engineering Portfolio - GitHub Pages Website

## Objective

Build a polished, responsive personal portfolio website for Arye Mindell, hosted via GitHub Pages from the **same repository** (`engineering-portfolio`). The site will showcase 9 engineering projects currently stored as PDFs, images, and Markdown files, transforming them into an interactive, navigable web experience. The site will be deployed from a `/docs` folder on the `main` branch (simplest GitHub Pages configuration for a co-located repo).

---

## Repository Inventory (Content Available for the Website)

| Project | Files | Content Type | External Links |
|---------|-------|-------------|----------------|
| **Dogfight AIsteroids** | README, Proposal, 2 PDFs, 1 PPTX, 5 PNGs, 1 GIF | AI/Game Dev capstone | [Source repo](https://github.com/amindell11/astronomical-home) |
| **FRC Team 1389** | 1 PDF (Software Overview) | Robotics / FIRST | None |
| **Humatics Internship** | 1 PDF (Assembly Instructions) | Industry internship | None |
| **IDT Coding Contest** | 3 PDFs (Intro, Brief, Feedback) | Competition | None |
| **Junior Design Lab** | README, 2 PDFs (Op-Amp, Prime Counter) | EE coursework | [JLab1](https://github.com/amindell11/JLab1), [JLab-II](https://github.com/amindell11/JLab-II) |
| **Misc Projects** | README, 7 JPG/PNG images, 4 loose images | Hardware/prototyping gallery | None |
| **OSV Autonomous Vehicle** | 2 PDFs (Poster, Design Brief), 1 empty file | Autonomous vehicle | None |
| **Capstone** (untracked) | 2 PDFs (Final Review, Midterm Report) | Senior capstone | None |
| **Microwaves & Antennas** (untracked) | 2 PDFs (Lab 2, Lab 3) | RF/Antenna labs | None |

**Personal info from README.md:** Arye Mindell, EE, passionate about signal processing, embedded systems, audio technology. Worked at Aurora Flight Sciences and Humatics Inc. Involved in FIRST Robotics and LMU Hillel. Email: arimindell@gmail.com, LinkedIn: linkedin.com/in/arye-mindell.

---

## Architecture Decision: Static HTML/CSS/JS (No Framework)

### Rationale
- **Zero build step** -- GitHub Pages serves static files directly; no CI/CD pipeline or npm needed.
- **Repository simplicity** -- This is a documentation repo, not a software project. Adding Node, React, or a static-site generator would add disproportionate tooling complexity.
- **Long-term maintenance** -- Plain HTML/CSS/JS requires no dependency updates, no security patches, and renders identically years from now.
- **Performance** -- No JavaScript bundle to download; instant paint. Small total payload.
- **Skill signal** -- For an EE portfolio (not a frontend developer portfolio), a clean, hand-crafted site signals attention to detail without over-engineering.

### Alternative Approaches Considered
1. **Jekyll (GitHub's built-in SSG):** Would allow Markdown-to-HTML conversion automatically. Trade-off: Ruby dependency, theme lock-in, `_config.yml` complexity, harder to customize layout precisely. Viable but more opaque.
2. **Hugo / Eleventy / Astro:** Powerful SSGs but require a build step (GitHub Actions). Overkill for ~9 project pages.
3. **React / Next.js SPA:** Massively over-engineered for a static portfolio. Introduces npm, bundling, hydration. Not recommended.

**Recommendation:** Pure static HTML/CSS with minimal vanilla JS for interactions (mobile nav toggle, smooth scroll, optional light/dark toggle). One CSS file, no framework.

---

## Deployment Strategy

- **Source:** `/docs` folder on `main` branch
- **GitHub Pages config:** Repository Settings > Pages > Source: "Deploy from a branch" > Branch: `main`, Folder: `/docs`
- **Why `/docs` over `gh-pages` branch:** Keeps website files visible alongside project content in a single branch. No branch-switching workflow. GitHub natively supports `/docs` as a Pages source.
- **URL:** `https://amindell11.github.io/engineering-portfolio/`

---

## Site Structure & Information Architecture

```
Home (index.html)
  |-- Hero section (name, title, tagline, CTA)
  |-- About section (bio, skills, contact links)
  |-- Projects grid (cards linking to individual pages)
  |-- Footer (contact, copyright)

Project Pages (one per project):
  |-- projects/dogfight-aisteroids.html
  |-- projects/frc-team-1389.html
  |-- projects/humatics-internship.html
  |-- projects/idt-coding-contest.html
  |-- projects/junior-design-lab.html
  |-- projects/misc-projects.html
  |-- projects/osv-autonomous-vehicle.html
  |-- projects/capstone.html
  |-- projects/microwaves-antennas.html
```

---

## File Tree to Create

```
docs/
|-- index.html                          # Landing page
|-- css/
|   |-- style.css                       # All styles (variables, layout, components, responsive)
|-- js/
|   |-- main.js                         # Mobile nav toggle, smooth scroll, optional theme toggle
|-- assets/
|   |-- images/                         # Optimized copies of project images for web
|   |   |-- hero-bg.webp                # (optional) Hero background or pattern
|   |   |-- profile.jpg                 # (if available) Headshot
|   |   |-- asteroids/                  # Copied from CMSI_Final_Proj_Asteroids/
|   |   |-- misc/                       # Copied from Misc_Projects/ and Misc_Projects/images/
|   |   |-- osv/                        # Any images extracted/referenced for OSV
|   |-- docs/                           # PDF files linked for download/viewing
|   |   |-- (symlinks or copies of key PDFs from project folders)
|-- projects/
|   |-- dogfight-aisteroids.html
|   |-- frc-team-1389.html
|   |-- humatics-internship.html
|   |-- idt-coding-contest.html
|   |-- junior-design-lab.html
|   |-- misc-projects.html
|   |-- osv-autonomous-vehicle.html
|   |-- capstone.html
|   |-- microwaves-antennas.html
```

---

## Implementation Plan

### Phase 1: Foundation & Scaffolding

- [x] **1.1 Create `/docs` directory structure.** Create the folder tree: `docs/`, `docs/css/`, `docs/js/`, `docs/assets/images/`, `docs/assets/docs/`, `docs/projects/`. This establishes the deployment root without disturbing existing project folders.

- [x] **1.2 Create `docs/css/style.css` with design system.** Define CSS custom properties (variables) for colors, typography, spacing, and breakpoints. Establish a cohesive visual identity:
  - Color palette: dark navy/charcoal primary (`#1a1a2e` or similar), accent color (electric blue `#0ea5e9` or teal), light text on dark sections, dark text on light sections.
  - Typography: Use system font stack or Google Fonts (e.g., Inter for body, a monospace for technical labels).
  - Spacing scale: 4px base unit (0.25rem increments).
  - Breakpoints: mobile-first with breakpoints at 640px, 768px, 1024px, 1280px.

- [x] **1.3 Create `docs/index.html` with full semantic structure.** Build the landing page with these sections:
  - `<header>` -- Fixed/sticky nav bar with logo/name, navigation links (About, Projects, Contact), mobile hamburger menu.
  - `<section id="hero">` -- Full-viewport hero with name ("Arye Mindell"), title ("Electrical Engineer"), a one-line tagline drawn from the README's "About Me," and a CTA button scrolling to projects.
  - `<section id="about">` -- Two-column layout (text + optional photo placeholder). Content sourced from `README.md:7`: signal processing, embedded systems, audio technology, Aurora Flight Sciences, Humatics Inc., FIRST Robotics, LMU Hillel.
  - `<section id="projects">` -- CSS Grid of project cards (3 columns on desktop, 2 on tablet, 1 on mobile). Each card: thumbnail image, project title, short description (1-2 sentences), tech tags, link to detail page.
  - `<section id="contact">` -- Email link, LinkedIn link, GitHub profile link.
  - `<footer>` -- Copyright, "Built with HTML/CSS/JS" badge.

- [x] **1.4 Create `docs/js/main.js` with interaction logic.** Implement:
  - Mobile hamburger menu toggle (add/remove `.active` class on nav).
  - Smooth scroll for anchor links.
  - Optional: Intersection Observer for fade-in animations on scroll.
  - Optional: Light/dark theme toggle using `prefers-color-scheme` as default, with localStorage persistence.

### Phase 2: Design & Styling

- [x] **2.1 Implement responsive CSS layout.** Style all structural elements:
  - Sticky header with blur backdrop (`backdrop-filter: blur`).
  - Hero section with gradient or subtle background pattern.
  - Project card grid with hover effects (subtle scale, shadow elevation, accent border).
  - Consistent spacing, clean typography hierarchy (h1-h4, body, caption).
  - Fully responsive: test at 320px, 375px, 768px, 1024px, 1440px widths.

- [x] **2.2 Style project cards with visual hierarchy.** Each card should include:
  - A thumbnail area (16:9 or 4:3 aspect ratio, `object-fit: cover`).
  - Project title in bold.
  - 1-2 line description.
  - Tech/skill tags as small pill badges (e.g., "Unity", "Python", "PPO", "Op-Amp Design", "Arduino").
  - "View Project" link/button.

- [x] **2.3 Add CSS transitions and micro-interactions.** Subtle polish:
  - Card hover: `transform: translateY(-4px)`, `box-shadow` increase.
  - Nav link underline animation on hover.
  - Button hover state changes.
  - Smooth color transitions for theme toggle (if implemented).
  - `scroll-behavior: smooth` on `html`.

### Phase 3: Project Detail Pages

- [x] **3.1 Create a reusable HTML template for project pages.** All 9 project pages share the same layout structure:
  - Same `<header>` navigation as index (with "Back to Portfolio" link).
  - Hero/banner area with project title and subtitle.
  - Main content area: project description, images/screenshots, embedded PDF viewer or download links, links to external repos.
  - Sidebar or metadata block: date/timeframe, skills used, course/context, external links.
  - Footer consistent with index.

- [x] **3.2 Build `docs/projects/dogfight-aisteroids.html`.** Content from `CMSI_Final_Proj_Asteroids/README.md`:
  - Overview: 3D space dogfighting game with BT and PPO RL opponents.
  - Screenshots: `Demo_Screenshot_1.png`, `Demo_Screenshot_2.png`.
  - GIF: `PPO_Lesson_1.gif` (RL training visualization).
  - Testing screenshot: `Tests_Passing.png`.
  - Reward curve chart: `Self_Play_Cum_Reward.png`.
  - Link to full source repo: `https://github.com/amindell11/astronomical-home`.
  - PDF links: proposal, presentation slides.
  - Tags: Unity, C#, PPO, Reinforcement Learning, Behaviour Trees, ML-Agents.

- [x] **3.3 Build `docs/projects/frc-team-1389.html`.** Content from `README.md:13` and PDF title:
  - Overview: FIRST Robotics Competition team software -- autonomous path-following, computer vision, reusable libraries, real-time web dashboards.
  - PDF embed/download: `Team_1389_STRATUS_Software_Overview.pdf`.
  - Tags: Java, FRC, WPILib, Computer Vision, Path Planning, WebSockets.

- [x] **3.4 Build `docs/projects/humatics-internship.html`.** Content from README and PDF title:
  - Overview: Internship at Humatics Inc. working on ultra-wideband precision positioning systems; hands-on assembly of Apollo product.
  - PDF embed/download: `Humatics_Apollo_Assembly_Instructions.pdf`.
  - Tags: UWB, Sensor Systems, Hardware Assembly, Technical Documentation.

- [x] **3.5 Build `docs/projects/idt-coding-contest.html`.** Content from file names:
  - Overview: IDT Winter 2016 coding competition as team "The Flatlanders."
  - PDF links: Competition Introduction, Software Brief, Feedback document.
  - Tags: Competitive Programming, Embedded Systems, IDT.

- [x] **3.6 Build `docs/projects/junior-design-lab.html`.** Content from `Junior_Design_Lab/README.md`:
  - Overview: Two semesters of EE Junior Lab coursework.
  - Highlight 1: Multi-stage operational amplifier design (gain, impedance, bandwidth specs).
  - Highlight 2: Synchronous digital counter (prime numbers with JK flip-flops).
  - PDF links: Lab 6 (Op-Amp) and Expt 8 (Prime Counter) reports.
  - External repo links: JLab1, JLab-II.
  - Tags: Op-Amp Design, Digital Logic, Circuit Simulation, LTSpice, Lab Reports.

- [x] **3.7 Build `docs/projects/misc-projects.html`.** Content from `Misc_Projects/README.md`:
  - Gallery-style layout with image + description pairs.
  - 7 projects: Maelstrom Shooter Arm, Truss Design, FRC Field Replica, IMU Tilt Table, 3D Printed Parts, Ultrasonic Doorbell, Maelstrom Electrical Board.
  - All images from `Misc_Projects/images/`.
  - Tags: CAD, 3D Printing, Arduino, Mechanical Design, FRC, Prototyping.

- [x] **3.8 Build `docs/projects/osv-autonomous-vehicle.html`.** Content from file names and README:
  - Overview: ENES100 Over-Sand Vehicle ("Urban Turtle") -- sensor rigs, drivetrain, autonomous navigation.
  - PDF links: Poster and Design Brief.
  - Tags: Autonomous Navigation, Sensor Integration, Arduino, Mechanical Design.

- [x] **3.9 Build `docs/projects/capstone.html`.** Content from file names (currently untracked):
  - Overview: Senior capstone design project (2025).
  - PDF links: Final Design Review, Semester 2 Midterm Report.
  - Note: These files must be committed to Git first.
  - Tags: Capstone, Systems Engineering, Design Review.

- [x] **3.10 Build `docs/projects/microwaves-antennas.html`.** Content from file names (currently untracked):
  - Overview: Microwaves and antennas coursework lab reports.
  - PDF links: Lab 2, Lab 3.
  - Note: These files must be committed to Git first.
  - Tags: RF Engineering, Antennas, Microwave Circuits, S-Parameters.

### Phase 4: Asset Pipeline

- [x] **4.1 Copy and organize image assets into `docs/assets/images/`.** Replicate relevant images from project folders into the `docs` asset tree so the website is self-contained within `/docs`. Organize into subdirectories by project (e.g., `asteroids/`, `misc/`). Convert `.HEIC` to `.jpg` or `.webp` for browser compatibility (HEIC is not web-supported).

- [x] **4.2 Copy or symlink PDF files into `docs/assets/docs/`.** Place copies of all PDFs that the website will link to. Use relative paths from the project pages. Alternatively, link directly to the PDFs in their original repo locations (e.g., `../../CMSI_Final_Proj_Asteroids/Presentation.pdf`) -- this avoids duplication but couples the site to the repo's folder structure. **Recommendation:** Link to the original paths using relative URLs to avoid maintaining duplicate PDFs. GitHub Pages will serve the entire repo if deploying from `main` branch root, but with `/docs` folder deployment, only `/docs` is served. Therefore, **PDFs must be copied or the deployment source must be set to root `/` instead of `/docs`**.

  **Decision point:** Either (a) deploy from repo root `/` and place `index.html` at root (mixes website files with project files), or (b) deploy from `/docs` and copy needed assets into `/docs` (clean separation, some duplication). **Recommend option (b)** for cleanliness, with a note that PDF links can point to the GitHub-hosted raw file URLs (e.g., `https://github.com/amindell11/engineering-portfolio/raw/main/FRC_Team_1389/Team_1389_STRATUS_Software_Overview.pdf`) to avoid copying large PDFs.

- [x] **4.3 Optimize images for web.** Resize large JPG/PNG images to reasonable web dimensions (max ~1200px wide for detail views, ~600px for thumbnails). Compress with quality 80-85%. Consider converting to WebP for modern browsers with JPG fallback. The `.HEIC` file (`IMG_1983.HEIC`) must be converted to a web format.

### Phase 5: Commit Untracked Files

- [ ] **5.1 Commit the `Capstone/` directory.** Add and commit the two untracked PDFs (`CapstoneFInalDesignReview2025.pdf`, `CapstoneS2MidtermReport.pdf`) so they are available on GitHub.

- [ ] **5.2 Commit the `Microwaves_and_Antennas_Labs/` directory.** Add and commit the two untracked PDFs (`Mindell_Lab_2.pdf`, `Mindell_Lab_3.pdf`).

- [x] **5.3 Add a `.gitignore` file.** Create a root `.gitignore` to exclude OS artifacts (`.DS_Store`, `Thumbs.db`), editor configs, and any local-only files.

### Phase 6: SEO, Accessibility & Meta

- [x] **6.1 Add proper `<meta>` tags to all HTML pages.** Include:
  - `<meta charset="UTF-8">`
  - `<meta name="viewport" content="width=device-width, initial-scale=1.0">`
  - `<meta name="description" content="...">` (unique per page).
  - Open Graph tags (`og:title`, `og:description`, `og:image`, `og:url`) for social media link previews.
  - `<title>` tags unique per page (e.g., "Arye Mindell | Electrical Engineer Portfolio").

- [x] **6.2 Ensure WCAG accessibility compliance.** Verify:
  - All images have descriptive `alt` text.
  - Color contrast ratios meet WCAG AA (4.5:1 for text, 3:1 for large text).
  - Keyboard navigation works for all interactive elements.
  - Semantic HTML elements used throughout (`<nav>`, `<main>`, `<article>`, `<section>`, `<footer>`).
  - Focus styles visible on interactive elements.

- [x] **6.3 Add a favicon.** Create or source a simple favicon (initials "AM" or a circuit/engineering icon) in `.ico` and `.png` formats. Place in `docs/` and reference in all HTML `<head>` sections.

### Phase 7: GitHub Pages Deployment

- [ ] **7.1 Enable GitHub Pages in repository settings.** Navigate to Settings > Pages > Source: "Deploy from a branch" > Branch: `main` > Folder: `/docs`. Save.

- [ ] **7.2 Verify deployment.** After pushing the `/docs` folder, confirm the site is live at `https://amindell11.github.io/engineering-portfolio/`. Test all links, images, PDF downloads, and responsive breakpoints.

- [ ] **7.3 (Optional) Configure custom domain.** If a custom domain is desired, add a `CNAME` file to `/docs` with the domain name and configure DNS records accordingly.

---

## Verification Criteria

- The site loads correctly at `https://amindell11.github.io/engineering-portfolio/`
- All 9 project pages are accessible and display correct content
- All images render (no broken images, no unsupported formats like HEIC)
- All PDF links work (either embedded viewers or direct downloads)
- External repository links open correctly in new tabs
- The site is fully responsive: renders correctly at 320px through 1440px+ widths
- Navigation works on both desktop (horizontal nav) and mobile (hamburger menu)
- Lighthouse scores: Performance > 90, Accessibility > 90, Best Practices > 90, SEO > 90
- All text is readable with sufficient contrast
- Page titles and meta descriptions are unique and descriptive
- No console errors in browser DevTools

---

## Potential Risks and Mitigations

1. **PDF file sizes bloating the repository**
   Mitigation: Link to PDFs via GitHub raw URLs (`https://github.com/amindell11/engineering-portfolio/raw/main/...`) rather than copying them into `/docs`. This avoids duplicating ~12+ PDFs. The trade-off is that links go through GitHub's raw file serving rather than Pages, but this is reliable and avoids repository bloat.

2. **HEIC image format not supported by browsers**
   Mitigation: Convert `IMG_1983.HEIC` to `.jpg` or `.webp` before including it on the site. No major browser supports HEIC natively.

3. **GitHub Pages `/docs` folder only serves content within `/docs`**
   Mitigation: Any asset referenced by the website must either live inside `/docs` or be linked via absolute GitHub URLs. Plan accordingly when referencing project PDFs and images.

4. **Untracked files (`Capstone/`, `Microwaves_and_Antennas_Labs/`) not available on GitHub**
   Mitigation: Commit these directories before building their project pages (Phase 5, tasks 5.1 and 5.2).

5. **Large image files causing slow page loads**
   Mitigation: Optimize and compress images in Phase 4.3. Use `loading="lazy"` attribute on all below-fold images. Consider WebP format.

6. **Maintenance burden of hand-written HTML across 10 pages**
   Mitigation: Keep the HTML template consistent and simple. Use CSS classes for all styling (no inline styles). If the portfolio grows significantly (20+ projects), consider migrating to a static site generator at that point.

7. **Inconsistent file naming in existing repo (spaces, mixed case extensions)**
   Mitigation: When copying files to `/docs/assets/`, normalize names to lowercase-kebab-case (e.g., `competition-introduction.pdf`). Keep original files untouched.

---

## Alternative Approaches

1. **Jekyll with a portfolio theme (e.g., Minimal Mistakes, Beautiful Jekyll):** Write project content as Markdown files with YAML front matter; Jekyll converts to HTML automatically. Pros: Less HTML to write, built-in GitHub Pages support. Cons: Ruby dependency for local dev, less layout control, theme update churn. Best if rapid Markdown-to-site conversion is the priority.

2. **Single-page application (SPA) with vanilla JS:** Put everything on `index.html` with hash-based routing and dynamically load project content. Pros: Single file to maintain, no page reloads. Cons: Poor SEO, no deep-linkable project URLs, more complex JS. Not recommended.

3. **Astro or Eleventy with GitHub Actions:** Write content in Markdown, build to static HTML via CI. Pros: Best of both worlds (Markdown authoring + full HTML control). Cons: Requires GitHub Actions workflow, npm/Node dependency, more complex setup. Best for a developer who wants Markdown authoring with modern tooling.

4. **Deploy from repo root `/` instead of `/docs`:** Place `index.html` at root alongside project folders. Pros: Can reference PDFs/images directly without copying. Cons: Mixes website infrastructure files with portfolio content, clutters the root directory. Not recommended for organizational clarity.
