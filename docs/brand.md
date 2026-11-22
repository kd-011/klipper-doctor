---
slug: klipper-doctor-brand
name: klipper-doctor brand
repo: .
status: active
last_verified: 2026-11-22
needs_review: true
---

# klipper-doctor: brand

Who the project speaks as. There is no DESIGN.md because the repo has no user-facing UI: the CLI's text and the README are the whole voice. Drafted only from copy already shipped, every line cited; the maintainer owns it and clears needs_review once checked.

## Name and casing

- The name is `klipper-doctor`: all lower case, with a hyphen. It is the npm package, the command, the README title and the GitHub Action's name. <!-- src: package.json#name, package.json#bin, README.md:1, action.yml:1 -->
- It stays lower case at the start of a sentence ("klipper-doctor reads a `klippy.log`..."), and in the social card it is drawn as `klipper-doctor` with the hyphen picked out in amber. <!-- src: README.md:6, scripts/render-assets.mjs:141-165 -->
- Words around it keep their own casing: Klipper, Moonraker, Mainsail, Fluidd, and file names such as `klippy.log` and `printer.cfg` in code font. <!-- src: README.md:6, README.md:29 -->
- The maker credit is "Built by Hotend", with Hotend capitalised and linked. <!-- src: README.md:80 -->
- Never "Klipper Doctor", "KlipperDoctor" or "klipper doctor". This is a guess drawn from the one consistent form above, and is listed under Unverified.

## One-liner

"klipper-doctor reads a `klippy.log` or `printer.cfg` and tells you in plain English why Klipper stopped and what to try next." The npm description says the same in the third person, and the social card says "Why your Klipper print died, in plain English." <!-- src: README.md:6, package.json#description, scripts/render-assets.mjs:141-165 -->

## Audience

People who run a Klipper 3D printer and have a log or a config that is not behaving: someone who has the `klippy.log` from Mainsail or Fluidd, someone checking a printer from another computer on the same network, and someone linting a config repo in CI. Contributors are told to open an issue with a redacted log snippet. <!-- src: README.md:15-35, CONTRIBUTING.md:3 -->

## Is and is not

- It is a tool that reads text and explains a Klipper shutdown, with the cause and the fix, and lints a printer config. <!-- src: README.md:6, src/options.ts:47 -->
- It is built from the Klipper docs and source: every rule traces to one, so it covers less than a model might. <!-- src: README.md:72 -->
- It runs on your machine and uploads nothing. <!-- src: src/options.ts:58 -->
- It is a companion to the log graphers and config linters it credits, not a replacement. <!-- src: README.md:72 -->
- It is not a print quality tool: ringing, adhesion and layer shifts leave nothing in the log. <!-- src: README.md:65 -->
- It is not a diagnosis for vendor forks and third-party modules, nor a way to tell a bad cable from a bad power supply, nor proof that a config is correct. <!-- src: README.md:66-68 -->

## Voice

Plain, specific, modest.

Do:

- Say what happened, the likely cause and the fix, in that order. <!-- src: CONTRIBUTING.md:20 -->
- State the limits next to the claim, such as "An AI model did that reading and nobody has second-checked it, so treat it as one reader's judgement." <!-- src: README.md:61 -->
- Quote Klipper's own words and link its docs or source. <!-- src: CONTRIBUTING.md:9 -->
- Credit other tools by name. <!-- src: README.md:70-72 -->
- Give the next command to try, as in the CLI's error messages. <!-- src: src/cli.ts:204-205 -->

Do not:

- Use em dashes or marketing words. <!-- src: CONTRIBUTING.md:20 -->
- Claim to be the first or the only. <!-- src: README.md:70-72 -->
- Invent thresholds or advice with no Klipper source. <!-- src: CONTRIBUTING.md:9 -->

Banned words: the only banned class in shipped copy is "marketing words", with no list. The list is not settled and is under Unverified.

## Copy bank

- README title and opening: `# klipper-doctor`, then the one-liner above, then a real captured output rather than a mock-up. <!-- src: README.md:1-13 -->
- Section titles are short and plain: "Try it", "What you get", "How often it helps", "When not to use this", "Other tools", "More". <!-- src: README.md:15-74 -->
- Package description: "Reads a klippy.log or printer.cfg and says in plain English why Klipper stopped and what to try next." <!-- src: package.json#description -->
- CLI help tagline: "Explain why a Klipper print died and lint the printer config." and "Runs on your machine. Nothing is uploaded; a Moonraker URL is fetched from that host only." <!-- src: src/options.ts:47-58 -->
- GitHub Action description: "Lint Klipper printer.cfg files and annotate the problems on the commit or pull request." <!-- src: action.yml:2 -->
- Pre-commit hook description: "Lint Klipper printer config files (printer.cfg and the other .cfg files in a config repo)". <!-- src: .pre-commit-hooks.yaml:12 -->
- Social card: "Why your Klipper print died, in plain English." above the command `npx klipper-doctor klippy.log`. <!-- src: scripts/render-assets.mjs:141-165 -->
- CTA wording is an instruction with the command to run: "To see a report without a printer, feed it the sample log from this repo" and "No Node? The same engine runs at hotend.app/klipper-doctor." <!-- src: README.md:17-35 -->
- Finding shape: a title with the rule id in brackets, a plain detail paragraph, then a line starting "Fix:". Example: "Timer too close [mcu-timer-too-close]". <!-- src: README.md:37-57, src/core/report.ts:136-148 -->
- Empty state: the note "No failure found in the log", with the line "If the print still failed, check that this is the right session and the right log file." <!-- src: src/core/rules/crash/fallback.ts:18-25 -->
- Error tone: lower case, prefixed `klipper-doctor: `, one plain sentence that names the cause and the next step, for example "no such file: x" followed by where klippy.log lives and how to fetch it. <!-- src: src/cli.ts:20-23, src/cli.ts:204-205, src/cli.ts:207-228 -->
- Issue form titles and prompts: "Bug, wrong diagnosis", and "klipper-doctor blamed the wrong thing, missed the real cause, or flagged something that is fine." <!-- src: .github/ISSUE_TEMPLATE/bug-wrong-diagnosis.yml:1-2 -->
- OG title and description: none. The repo has no web page of its own, so it ships no meta tags. <!-- src: package.json#files -->

## Assets

- No logo and no favicon in the repo. <!-- src: package.json#files -->
- `assets/demo.png`: the README demo, real output drawn from `test/fixtures/demo.log`. <!-- src: scripts/render-assets.mjs:1-4, README.md:8-11 -->
- `assets/demo-hero.png`: a phone-width crop of one finding, used by the README at 600px and narrower. <!-- src: scripts/render-assets.mjs:1-4, README.md:8-11 -->
- `assets/social-preview.png` (1280 by 640): the social card, drawn by `npm run assets`. `.github/social-preview.png` is a separate 1280 by 640 file that the tests check. <!-- src: scripts/render-assets.mjs:1-4, test/kit.test.mjs:153-158 -->
- The Action's marketplace branding is the `check-circle` icon in orange. <!-- src: action.yml:5-7 -->

## Contact and legal

- Bugs and rule requests go to the GitHub issue tracker, through the issue forms. Blank issues are off. <!-- src: package.json#bugs.url, .github/ISSUE_TEMPLATE/config.yml:1 -->
- Security problems go through GitHub's private vulnerability reporting, not a public issue. SECURITY.md carries no email address. <!-- src: SECURITY.md:5, .github/ISSUE_TEMPLATE/config.yml:2-5 -->
- Conduct reports use the same private channel and say they are a conduct report. It is the only private channel the project has. <!-- src: CODE_OF_CONDUCT.md:19 -->
- Licence: MIT, "Copyright (c) 2026 kd-011". Contributors agree their work is released under it. <!-- src: LICENSE:3, CONTRIBUTING.md:41 -->
- Author in `package.json` is `kd-011`. <!-- src: package.json#author -->
- No privacy page, terms page or business name: the repo has no web page, and the tool uploads nothing. <!-- src: package.json#files, src/options.ts:58 -->

## Unverified

- Never-forms of the name ("Klipper Doctor", "KlipperDoctor") are inferred from the one form the repo uses.
- Banned words: the shipped copy bans "marketing words" without a list. Any list beyond that would be a guess.
- Voice adjectives (plain, specific, modest) are read from the README and CONTRIBUTING.md, not stated anywhere.
- Whether the maintainer wants "Built by Hotend" kept as the credit line, given `LICENSE` and `package.json` name `kd-011`.
- Which of `assets/social-preview.png` and `.github/social-preview.png` is the one set as the GitHub social preview. The repo holds both and they differ.
- hotend.app/klipper-doctor is another repo's page, so its copy and meta tags were not read.
