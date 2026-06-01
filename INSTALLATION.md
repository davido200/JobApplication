# Career-Ops — Installation Guide

A complete, step-by-step guide to get Career-Ops running on your machine.

---

## Required Tools

Before you begin, make sure the following tools are installed on your system.

### 1. Node.js (v18 or later) — **Required**

Used to run all scripts: PDF generation, pipeline utilities, deduplication, and health checks.

- **Download:** https://nodejs.org/en/download
- Choose the **LTS** version (Long Term Support)
- After installing, verify in your terminal:
  ```powershell
  node --version   # Should print v18.x.x or higher
  npm --version    # Should print 8.x.x or higher
  ```

---

### 2. Gemini CLI — **Required**

The AI agent that powers the entire pipeline. Career-ops is built around Gemini CLI's slash command system.

- **Download & Install:** https://geminicli.com
- Follow the official installation instructions on that page
- After installing, verify:
  ```powershell
  gemini --version
  ```
- You will need a **Gemini API key** or a valid AI provider configuration to use it

---

### 3. Git — **Required**

Used to clone the repository (you likely already have this).

- **Download:** https://git-scm.com/download/win
- After installing, verify:
  ```powershell
  git --version
  ```

---

### 4. Go (v1.21 or later) — **Optional**

Only needed if you want to use the **Dashboard TUI** — the terminal UI to visually browse and filter your job pipeline.

- **Download:** https://go.dev/dl
- After installing, verify:
  ```powershell
  go version   # Should print go1.21.x or higher
  ```

> Skip this if you don't plan to use the visual dashboard.

---

## Installation Steps

### Step 1 — Clone the Repository

If you haven't already cloned the project:

```powershell
git clone https://github.com/santifer/career-ops.git
cd career-ops
```

If you already have the project locally (e.g. at `d:\project\job\career-ops`), just navigate to it:

```powershell
cd d:\project\job\career-ops
```

---

### Step 2 — Install Node.js Dependencies

This installs Playwright and all other required packages defined in `package.json`.

```powershell
npm install
```

---

### Step 3 — Install Playwright's Chromium Browser

Playwright uses a headless Chromium browser to scrape job portals and generate PDF resumes.

```powershell
npx playwright install chromium
```

> This downloads ~150MB. It only needs to be done once.

---

### Step 4 — Create Your Profile Config

Copy the example profile template and fill in your personal details.

```powershell
copy config\profile.example.yml config\profile.yml
```

Then open `config\profile.yml` in your editor and fill in **your** information:

| Field | What to put |
|-------|-------------|
| `candidate.full_name` | Your full name |
| `candidate.email` | Your email address |
| `candidate.phone` | Your phone number |
| `candidate.location` | Your city and country |
| `candidate.linkedin` | Your LinkedIn profile URL |
| `candidate.portfolio_url` | Your personal website (if any) |
| `target_roles.primary` | Job titles you're targeting |
| `target_roles.archetypes` | Role types with fit level (primary / secondary / adjacent) |
| `narrative.headline` | Your one-line professional summary |
| `narrative.superpowers` | Your top 3–5 skills or strengths |
| `compensation.target_range` | Your target salary range |
| `compensation.minimum` | Your walk-away number |
| `location.visa_status` | Whether you need sponsorship |

---

### Step 5 — Add Your CV

Create a file called `cv.md` in the project root. This is the **most important file** — the AI reads it to evaluate job fit and generate tailored resumes.

```powershell
# Create the file (then open and paste your CV content)
New-Item -Path cv.md -ItemType File
```

Format your CV in standard Markdown. See `examples\` folder for reference CVs. Include:

- Professional summary
- Work experience (with dates, company, role, and bullet points of impact)
- Skills / Tech stack
- Education
- Projects or achievements with measurable metrics

> **Optional:** Also create `article-digest.md` with links to your articles, portfolio projects, or case studies and their key results.

---

### Step 6 — Configure Job Portals

Copy the portals template and customize it with the companies you want to scan.

```powershell
copy templates\portals.example.yml portals.yml
```

Open `portals.yml` and update:

- **`title_filter.positive`** — Keywords matching your target roles (e.g. `"AI Engineer"`, `"ML"`)
- **`tracked_companies`** — Companies you want the scanner to monitor
- **`search_queries`** — Custom search terms for job boards like Ashby, Greenhouse, Lever

> The template already comes pre-configured with 45+ AI/tech companies.

---

### Step 7 — Run the Doctor Check

This validates your entire setup and tells you exactly what's missing or broken.

```powershell
npm run doctor
```

You should see a checklist like this:

```
career-ops doctor
================

✓ Node.js >= 18 (v20.x.x)
✓ Dependencies installed
✓ Playwright chromium installed
✓ cv.md found
✓ config/profile.yml found
✓ portals.yml found
✓ Fonts directory ready
✓ data/ directory ready
✓ output/ directory ready
✓ reports/ directory ready

Result: All checks passed. You're ready to go! Run `claude` to start.
```

Fix any `✗` items shown before proceeding.

---

### Step 8 — Start Using Career-Ops

Open Claude Code in the project directory:

```powershell
claude
```

Then you can either:

- **Paste a job URL or description** — Career-ops auto-detects it and runs the full pipeline
- **Use a slash command:**

```
/career-ops              → Show all available commands
/career-ops scan         → Scan configured portals for new offers
/career-ops pdf          → Generate a tailored ATS-optimized CV PDF
/career-ops batch        → Evaluate multiple offers in parallel
/career-ops tracker      → View your application pipeline status
/career-ops pipeline     → Process all pending URLs
/career-ops deep         → Deep-dive research on a company
/career-ops interview    → Interview preparation mode
```

---

## Optional — Build the Dashboard TUI

If you installed Go, you can build the terminal dashboard to visually browse your pipeline:

```powershell
cd dashboard
go build -o career-dashboard.exe .
.\career-dashboard.exe --path ..
```

This opens an interactive terminal UI with filtering, sorting, and status management for all your tracked applications.

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| `node: command not found` | Install Node.js from https://nodejs.org and restart your terminal |
| `claude: command not found` | Install Claude Code from https://claude.ai/code |
| `npx playwright install` fails | Make sure `npm install` ran successfully first |
| `cv.md not found` error | Create `cv.md` in the project root (Step 5) |
| `config/profile.yml not found` | Run `copy config\profile.example.yml config\profile.yml` (Step 4) |
| `portals.yml not found` | Run `copy templates\portals.example.yml portals.yml` (Step 6) |
| PDF generation fails | Verify Playwright chromium is installed: `npx playwright install chromium` |
| Dashboard won't build | Install Go 1.21+ from https://go.dev/dl |

Run `npm run doctor` at any time to re-check your setup status.

---

## Quick Reference — Key Files

| File | Purpose |
|------|---------|
| `cv.md` | Your CV — source of truth for all evaluations and PDF generation |
| `config/profile.yml` | Your personal profile, target roles, and salary expectations |
| `portals.yml` | Companies and job boards to scan |
| `CLAUDE.md` | Agent instructions (do not delete) |
| `modes/` | The 14 skill modes that power each `/career-ops` command |
| `data/` | Your tracker data (auto-created, gitignored) |
| `reports/` | Evaluation reports (auto-created, gitignored) |
| `output/` | Generated PDF resumes (auto-created, gitignored) |

---

*For more details, see [docs/SETUP.md](docs/SETUP.md), [docs/CUSTOMIZATION.md](docs/CUSTOMIZATION.md), and [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md).*
