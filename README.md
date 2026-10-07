# Phishing Link Analyzer

A single-file web app that checks a suspicious link for common phishing signs and explains each one in plain English. No install, no server, no dependencies.

**Live demo:** https://kingtausolarin.github.io/phishing-link-analyzer/

Built by King Sabatsu Kisuke as part of a growing cybersecurity portfolio. It's a learning project: I built it to understand how phishing links actually work, and I'm still improving it.

## Features

- Heuristic risk score from 0 to 100 (not a probability) with a verdict: Low Risk Indicators, Suspicious or High Risk
- Every red flag listed with a short explanation of why it matters
- "How the score works" guide so the number is never a mystery
- Link breakdown: scheme, host, registered domain, subdomains, port, path and parameters
- **Defanged copy:** copies a safe version of the link (`hxxp://evil[.]example`) so it can be shared without anyone clicking it by accident
- Built-in examples to try: a fake bank link, the `@` trick and a legitimate link

## How to use

1. Open the live demo (or `index.html` in any modern browser).
2. Paste a link and press **Analyze**.
3. Read the verdict, then the findings underneath to see what triggered it.

### Examples

| Link | Score | Why |
|------|-------|-----|
| `http://paypa1-secure-login.verify-account.xyz/signin` | 85, High Risk | HTTP, brand imitation (`paypa1`), risky `.xyz` ending, many hyphens, pressure words |
| `http://192.168.0.1@evil.example/login` | 45, Suspicious | Text before `@` is a decoy, HTTP, login keyword |
| `https://accounts.google.com` | 5, Low Risk Indicators | HTTPS and a real domain; only a mild keyword match |

## What it checks

| Signal | Points |
|--------|--------|
| Brand imitation with swapped characters (`paypa1`) | +35 |
| Brand name used on a domain that isn't the brand | +30 |
| IP address instead of a domain name | +30 |
| Text before an `@` sign | +30 |
| Lookalike (punycode) domain | +30 |
| Link shortener | +20 |
| Risky domain ending (`.xyz`, `.top`, `.zip` and others) | +15 |
| Pressure or login words (`verify`, `secure`, `login`...) | +5 each, up to 15 |
| No HTTPS | +10 |
| Unusual port | +10 |
| Another link hidden inside the link | +10 |
| Four or more subdomains | +10 |
| Three or more hyphens in the domain | +10 |
| Brand name only in the path | +10 |
| Very long link | +5 or +10 |
| Lots of encoded characters | +5 |
| Non-web scheme (`javascript:`, `data:`) | +50 |

Scores add up and are capped at 100. 0 to 19 is Low Risk Indicators, 20 to 49 is Suspicious, and 50 or more is High Risk. The score is the combined weight of detected warning signs, not a probability.

## How it works

- The link is parsed with the browser's built-in `URL` API.
- The registered domain is found with a simple rule plus a short list of common two-part endings (`.co.uk`, `.com.ng` and similar).
- Brand imitation is detected by normalising look-alike characters (`0` to `o`, `1` to `l` or `i`, `rn` to `m`) and comparing against a list of commonly impersonated brands.
- Everything is rule-based. There is no machine learning and no external service.

## Privacy

Everything runs locally in your browser. The link you paste is never opened, fetched or sent anywhere.

## Limitations

- It only analyses the **text of the link**. It cannot see the page behind it, check reputation databases or follow redirects.
- A low score does **not** prove a link is safe, and a high score does not prove it is malicious. Treat it as a second opinion, not a verdict.
- The brand and keyword lists are short and English-focused, so some local brands and languages will be missed.
- Some legitimate links will trigger mild warnings (for example, a real login page containing the word `account`).

## Project structure

| File | Purpose |
|------|---------|
| `index.html` | The entire app (HTML, CSS and JS) |
| `README.md` | This file |

## Roadmap

- Paste a full email and extract and check every link in it
- Add more brands, including regional banks and fintech apps
- Detect more homograph patterns
- Optional reputation lookups (opt-in, since they would send the link to a third party)
- Screenshot and demo GIF for this README

## Contributing and feedback

Found a link it gets wrong, or a brand it should know about? Open an issue. Honest feedback is welcome.
