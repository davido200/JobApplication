# Career-Ops

Career-Ops is a local, open-source job search automation system that turns an AI coding CLI into a structured application command center. It helps candidates evaluate job opportunities, generate tailored ATS-ready CVs, scan job portals, track applications, and prepare for interviews.

The system is designed for selective, high-quality applications. It evaluates role fit before recommending action and does not submit applications automatically. Users remain responsible for reviewing all generated materials before sending them to employers.

## Key Features

- Structured job evaluation with role fit, CV match, gaps, compensation context, and interview preparation.
- Tailored PDF CV generation based on the target job description.
- Automated portal scanning for new job opportunities.
- Batch processing for multiple job evaluations.
- Centralized application tracking with status normalization, deduplication, and health checks.
- Interview preparation using reusable STAR+Reflection stories.
- Negotiation guidance and application response drafting.

## Quick Start

```bash
git clone https://github.com/santifer/career-ops.git
cd career-ops
npm install
npx playwright install chromium
npm run doctor
```

Create and customize the required user files:

```bash
cp config/profile.example.yml config/profile.yml
cp templates/portals.example.yml portals.yml
```

Then add your CV as `cv.md` in the project root and customize your profile, target roles, portals, and preferences.

## Basic Usage

Career-Ops can be used through slash commands in supported AI coding CLIs or by pasting a job URL or job description directly.

Common commands:

```text
/career-ops              Show available commands
/career-ops scan         Scan configured job portals
/career-ops pdf          Generate a tailored CV PDF
/career-ops batch        Evaluate multiple roles
/career-ops tracker      Review application status
/career-ops apply        Assist with application forms
/career-ops deep         Run company research
```

When a job URL or description is provided, the system can generate an evaluation report, tailor the CV, create a PDF, and update the application tracker.

## Project Structure

```text
career-ops/
+-- cv.md                    User CV source of truth
+-- config/                  User profile configuration
+-- modes/                   AI workflow instructions
+-- templates/               CV, portal, and status templates
+-- data/                    Application and pipeline tracking data
+-- reports/                 Generated evaluation reports
+-- output/                  Generated CV PDFs
+-- batch/                   Batch processing files
+-- dashboard/               Terminal dashboard
+-- docs/                    Setup and architecture documentation
```

## Technology Stack

- Node.js for automation scripts.
- Playwright for PDF generation and job posting verification.
- Markdown, YAML, and TSV for local data storage.
- Go, Bubble Tea, and Lipgloss for the terminal dashboard.
- Compatible with AI coding CLIs such as Claude Code, Gemini CLI, and OpenCode.

## Responsible Use

Career-Ops is intended to help candidates identify strong-fit opportunities, not to mass-apply indiscriminately. Always review generated CVs, reports, messages, and form responses before submission. The user is responsible for complying with third-party job board and ATS terms of service.

## License

The project is licensed under the MIT License. The `career-ops` name and brand are governed by the project trademark policy. See `LICENSE`, `TRADEMARK.md`, and `LEGAL_DISCLAIMER.md` for full details.
