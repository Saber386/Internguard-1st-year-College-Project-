# InternGuard

> A client-side internship scam detection tool for students.

InternGuard helps students verify suspicious internship offers before responding. Paste an email or enter a company name — it checks against common scam patterns and returns a plain-language verdict with specific flags.

**No backend. No external API. No data leaves your browser.**

---

## Features

- **Email Scanner** — Analyses sender address, subject, and body for red flags like payment requests, free-domain senders, urgency language, and phishing links
- **Company Check** — Flags suspicious domain extensions, missing addresses, offers without interviews, and generic name patterns
- **Trust Score** — A 0–100 score derived from weighted flag severity
- **Scan History** — Session-persistent history stored in `localStorage`
- **Auth System** — Optional account creation with `localStorage`-based user storage
- **Guest Mode** — Full functionality without signing up

---

## Demo

Open `index.html` directly in any modern browser. No server required.

---

## How It Works

InternGuard uses **rule-based pattern matching** — no AI, no external database calls.

### Email Analysis Rules

| Flag | Severity | Trigger |
|------|----------|---------|
| Free email provider used | High | Sender is `@gmail`, `@yahoo`, `@hotmail`, etc. |
| Payment or fee mentioned | High | Keywords: `pay`, `fee`, `deposit`, `advance` |
| Moves off-platform | High | Keywords: `whatsapp`, `telegram`, `call this number` |
| Unrealistic compensation | High | Keywords: `high pay`, `earn from home`, salary patterns |
| Link click / verification | High | Keywords: `click the link`, `verify your account` |
| Unsolicited offer language | Medium | Phrases: `you have been selected`, `congratulations` |
| Vague requirements | Medium | Keywords: `no experience`, `work from home`, `no skills` |
| Artificial urgency | Medium | Keywords: `urgent`, `respond within 24 hours`, `act now` |
| No application process | Low | Body > 20 chars with no mention of CV/interview |

### Company Analysis Rules

| Flag | Severity | Trigger |
|------|----------|---------|
| Offer without interview | High | Keywords: `no interview`, `immediate offer` |
| Payment mentioned | High | Keywords: `pay`, `fee`, `security deposit` |
| Suspicious TLD | High | Domain ends in `.xyz`, `.top`, `.tk`, `.ml`, etc. |
| Career-focused subdomain | Medium | Domain contains `careers`, `jobs`, `recruit` |
| Numbers in domain | Medium | `\d{2,}` in the domain name |
| Unsolicited outreach | Medium | Keywords: `never applied`, `found your profile` |
| No verifiable address | Medium | Keywords: `no address`, `no office`, `remote only` |
| Generic name pattern | Low | Short name with buzzwords like `global`, `solutions` |

### Verdict Logic

```
High flags ≥ 2  OR  (High ≥ 1 AND Medium ≥ 2)  →  SCAM
High flags = 1  OR  Medium ≥ 2                  →  WARNING
No flags                                         →  SAFE
```

### Trust Score Formula

```
score = 100 − (high × 30) − (medium × 14) − (low × 5)
score = max(0, score)
```

---

## Project Structure

```
internguard/
└── index.html        # Entire application — HTML, CSS, and JS in one file
```

Single-file architecture. All logic, styles, and markup are self-contained.

---

## Local Setup

```bash
git clone https://github.com/Saber386/internguard.git
cd internguard
# Open index.html in your browser
open index.html       # macOS
start index.html      # Windows
xdg-open index.html   # Linux
```

No dependencies. No build step. No package manager.

---

## Data & Privacy

- All data is stored in **`localStorage`** in your browser
- Nothing is sent to any server
- Clearing browser storage removes all accounts and history
- Passwords are stored in plaintext in `localStorage` — this is intentional for a demo/educational project; **do not use a real password**

---

## Limitations

InternGuard is a **heuristic tool**, not a definitive checker.

- It cannot verify whether a company is legally registered
- It cannot access external databases (LinkedIn, MCA, Companies House)
- Pattern matching produces false positives and false negatives
- A "Safe" verdict does not guarantee an offer is legitimate

Always cross-check independently on **LinkedIn**, **Glassdoor**, and your country's company registry before sharing personal documents.

---

## Tech Stack

| Layer | Choice |
|-------|--------|
| Language | Vanilla HTML / CSS / JavaScript |
| Fonts | [Syne](https://fonts.google.com/specimen/Syne) + [Inter](https://fonts.google.com/specimen/Inter) via Google Fonts |
| Storage | Browser `localStorage` |
| Dependencies | None |

---

## Roadmap

- [ ] URL/link scanner (check for suspicious redirect domains)
- [ ] Export scan result as PDF
- [ ] Browser extension version
- [ ] Company registry lookup via public APIs (MCA21, Companies House)
- [ ] Shareable result links

---

## Contributing

Pull requests are welcome.

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m "add: your feature"`
4. Push to the branch: `git push origin feature/your-feature`
5. Open a pull request

For major changes, open an issue first to discuss the approach.

---

## License

[MIT](LICENSE)

---

## Author

**Abhinav Tiwary** — [@Saber386](https://github.com/Saber386) · [LinkedIn](https://linkedin.com/in/abhinavtiwary400)

> Built for students. Not affiliated with any company mentioned in results.
