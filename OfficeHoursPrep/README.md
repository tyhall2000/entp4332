# Office Hours Prep

A small, free-standing web app that helps a student organize their thoughts
before asking a professor for help. You fill in six short fields, click one
button, and get a readable "discussion outline" you can bring to office
hours — plus a set of starter questions to ask.

**Try it:** open `index.html` directly in a browser. No install, no server,
no build step.

## What it does

1. You fill in:
   - Course name (optional)
   - Topic or assignment (required)
   - What I understand so far (optional)
   - What I have already tried (optional)
   - Where I am stuck (required)
   - What I want to understand after office hours (required)
2. Click **Create My Prep Sheet**. The app drops your own words into a fixed
   outline template — it does not rewrite, summarize, or invent anything you
   didn't type.
3. The outline also includes a **Questions to Ask During Office Hours**
   section: six starter questions. Two of them quote your own "Where I am
   stuck" and "What I want to understand" text back to you inside a question;
   the other four are generic, always-useful prompts (e.g. "Can you walk me
   through where my approach breaks down, step by step?"). These come from a
   fixed list in the code — nothing is AI-generated or guessed about your
   specific problem.
4. From the outline screen you can **Copy** it (for pasting into an email or
   chat), **Print** it, **Edit answers** (go back to the form without losing
   what you typed), or **Start over** (clears everything, asks you to
   confirm first).
5. **Try an example** fills the form with clearly-labeled sample answers, so
   you can see what a finished sheet looks like before writing your own.

## Why it's built this way

- **Plain HTML/CSS/JS, one file, no framework, no build step.** This matches
  the rest of this repo's pattern (see the project's `CLAUDE.md`) and means
  there is nothing to install to run or edit it — you can open `index.html`
  in a browser or a text editor and see exactly what's happening.
- **No backend, no database, no accounts, no AI API.** Everything happens in
  your browser. Nothing you type is sent anywhere. This was an explicit
  requirement: the app should never appear to be smarter than it is, or
  invent information about a problem it can't actually understand.
- **Required vs. optional fields.** *Topic/assignment*, *Where I am stuck*,
  and *What I want to understand after office hours* are required, because a
  professor can't help without at least those three. *Course name*, *What I
  understand so far*, and *What I have already tried* are optional — a
  student might genuinely not have tried anything yet, and that's fine.
- **Fixed-template "questions" section, not AI.** The assignment specifically
  ruled out an AI API, so the "questions to ask" feature can't generate
  contextual questions from arbitrary text. Instead it uses a fixed list of
  well-tested office-hours questions, with two of them built by quoting the
  student's own words back into a question frame — tailored-feeling, without
  inventing anything.
- **Input is escaped before it's rendered**, so pasting something like
  `<script>` into a field shows up as plain text in the outline instead of
  being treated as HTML/code.

## Project structure

```
OfficeHoursPrep/
  index.html     — the entire app (HTML + CSS + JS, self-contained)
  README.md      — this file
  BUILD_LOG.md   — a running log of real errors, fixes, and decisions
                   made while building this, for writing an assignment memo
```

## Deploying

This repo is connected to Vercel, which auto-deploys the `main` branch. The
live app is served straight from `OfficeHoursPrep/index.html` — see the
Vercel project settings for the configured root directory.
