#!/usr/bin/env bash
set -euo pipefail

# -----------------------------------------------------------------------------
# GPT-5 Job Application Wizard Repo Bootstrap (Bash)
# Requirements:
#   - GitHub CLI: https://cli.github.com (authenticated: gh auth login)
#   - git
# Usage:
#   chmod +x bootstrap_repo.sh
#   ./bootstrap_repo.sh [REPO_NAME] [public|private]
# Defaults:
#   REPO_NAME = gpt5-job-app-wizard
#   VISIBILITY = private
# Environment overrides:
#   GITHUB_USER (defaults to JaZeR-444 from session)
# -----------------------------------------------------------------------------

REPO_NAME="${1:-gpt5-job-app-wizard}"
VISIBILITY="${2:-private}"
DEFAULT_BRANCH="main"
GITHUB_USER="${GITHUB_USER:-JaZeR-444}"

command -v gh >/dev/null 2>&1 || { echo "Error: gh (GitHub CLI) is not installed."; exit 1; }
command -v git >/dev/null 2>&1 || { echo "Error: git is not installed."; exit 1; }

if [[ "$VISIBILITY" != "public" && "$VISIBILITY" != "private" ]]; then
  echo "Visibility must be 'public' or 'private' (got '$VISIBILITY')"
  exit 1
fi

if [[ -e "$REPO_NAME" ]]; then
  echo "Error: directory '$REPO_NAME' already exists. Choose a new repo name or remove it."
  exit 1
fi

mkdir -p "$REPO_NAME"/{prompts,templates,config}
cd "$REPO_NAME"

# .gitignore
cat > .gitignore << 'EOF'
# Node / Python / Mac / general
node_modules/
.DS_Store
venv/
.env
*.log
EOF

# README.md
cat > README.md << 'EOF'
# GPT-5 Job Application Wizard

This repository contains:
- Exact resume and cover letter templates (your formatting preserved)
- A merged, ready-to-run GPT‑5 prompt that generates BOTH documents in one pass
- A JSON config capturing rules and validation

## Quick Start (GPT‑5)
1) Open `prompts/merged_prompt.md`
2) Copy everything and paste into GPT‑5
3) Provide:
   - Your current resume/experience
   - The target job description
   - Your contact info (name, phone, email, LinkedIn, location)
4) GPT‑5 will output both the resume and cover letter using your exact template formatting.

## Files
- prompts/merged_prompt.md — Paste directly into GPT‑5
- templates/resume_template.txt — Exact resume template
- templates/cover_letter_template.txt — Exact cover letter template
- config/job_app_wizard.json — Config with strict formatting rules

EOF

# prompts/merged_prompt.md
mkdir -p prompts
cat > prompts/merged_prompt.md << 'EOF'
# Complete Job Application Wizard for GPT‑5 (All-in-One)

SYSTEM ROLE: You are an expert ATS-optimized resume and cover letter specialist that creates both documents simultaneously using exact user template formatting.

CURRENT DATE: Auto-detect current date when running
USER: Use the user's provided name

COMPLETE WORKFLOW
When I provide my current resume/experience, target job description, and contact information, automatically generate BOTH documents using the exact templates below without asking for additional input.

CRITICAL FORMATTING REQUIREMENTS
- You MUST use the EXACT template structures below. Do NOT modify any spacing, line breaks, or formatting elements.
- Contact line uses emojis and exact spacing.
- Resume competencies use pipe separators with specific spacing.
- Resume bullets use "•	" (bullet + tab).
- Education header must be spaced: "E D U C A T I O N".
- Cover letter signature shows name twice with a line break between.

RESUME TEMPLATE (EXACT FORMAT)[bootstrap_repo.sh](https://github.com/user-attachments/files/22673449/bootstrap_repo.sh)
designed as a one stop shop resume customizer and cover letter editor for a job application
