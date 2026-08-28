# Omar Mowafy — Personal Portfolio Site

**Live site:** https://aquamarine-swan-babaac.netlify.app/ *(update to custom domain once live)*
**Repo:** https://github.com/omar-mowafy66/omar-mowafy-portfolio--1-

## What it is, and for whom

A single-page personal portfolio for Omar Mowafy, a Communication & Electronics
Engineering student specializing in Embedded Systems and AI. It exists for two
audiences: **recruiters/mentors** scanning for proof of skills and a way to reach
out, and **Omar himself**, as a permanent, public anchor for his work as it grows
(FlyRank capstone, future write-ups, projects).

It intentionally does one thing: state who Omar is, link out to real proof
(GitHub, LinkedIn, CV), and offer a working way to get in touch. No fake case
studies, no placeholder projects — the empty "Posts & capstone work" section
says exactly that it's empty, on purpose.

## Setup (reproducible from scratch)

This is a static site with zero build step and zero dependencies.

1. Clone the repo:
   ```
   git clone https://github.com/omar-mowafy66/omar-mowafy-portfolio--1-.git
   cd omar-mowafy-portfolio--1-
   ```
2. Open `index.html` directly in a browser — that's it. No `npm install`,
   no server required for local viewing.
3. To deploy: connect the repo to [Netlify](https://netlify.com) (or drag-and-drop
   the folder into Netlify's dashboard). Netlify auto-detects it as a static site.
4. The contact form uses **Netlify Forms** — this only works once deployed on
   Netlify (it won't submit anywhere when opened locally as a plain file).
   No extra configuration needed beyond the `data-netlify="true"` attribute
   already in the form markup.
5. Analytics: add your Cloudflare Web Analytics `<script>` snippet before the
   closing `</body>` tag (see "Architecture" below for where it plugs in).

No API keys, no environment variables, no `.env` file — everything needed to
run this is in the repo.

## Usage examples

- **View the site:** open the live URL above in any browser, desktop or mobile.
- **Download the CV:** click "Resume → Download CV →" in the nav bar; downloads
  `Omar_Mowafy_CV.docx` directly.
- **Contact Omar:** fill the form under "Get in touch" (name, email, message)
  and submit — it lands in the Netlify Forms dashboard on the site owner's side.
  Alternatively, click "Reach out → Book a call →" to open a pre-filled mailto link.

## Architecture

```
[ Visitor's browser ]
        |
        v
[ Netlify CDN — static hosting, HTTPS by default ]
   |
   |-- index.html      (structure, inline CSS, no JS framework)
   |-- favicon.svg      (site icon)
   |-- Omar_Mowafy_CV.docx  (static download asset)
        |
        v
[ Netlify Forms ] -----submission-----> [ Netlify dashboard, Forms tab ]
        |
        v
[ Cloudflare Web Analytics ] --pageview beacon--> [ Cloudflare dashboard ]
```

No backend server, no database. Netlify's built-in form handling replaces
the need for a custom API endpoint; Cloudflare's script tag replaces the need
for a self-hosted analytics stack.

## v2 eval results

A hardening pass was run against the live site before this v2 (see the full
hardening review report in this repo). Summary:

| Check | v1 result | v2 result |
|---|---|---|
| CV download link | 404 (broken) | ✅ Fixed, verified working |
| SEO meta description present | ❌ Missing | ✅ Added |
| Open Graph / social-share tags | ❌ Missing | ✅ Added |
| Favicon present | ❌ Missing | ✅ Added |
| Empty form submission blocked | ✅ Pass (browser `required` validation) | ✅ Pass |
| All nav links functional (LinkedIn, GitHub, CV, mailto) | 1 of 4 broken (CV) | ✅ 4 of 4 working |
| Site findable via name search on Google | ❌ Not indexed | 🟡 Pending re-crawl post-redeploy |

## Limitations (known, not hidden)

- **No confirmation message after form submit.** The form clears on submit
  but doesn't show an explicit "message sent" confirmation on-page — a visitor
  has to trust it worked. Submissions are visible in the Netlify dashboard.
- **"Posts & capstone work" is empty.** This is accurate, not a bug — no
  write-ups exist yet.
- **No automated tests.** This is a static marketing/identity page, not an
  application with business logic, so there's no test suite. Verification is
  manual (see hardening review report).
- **Search indexing takes time.** Adding meta tags doesn't force Google to
  index the page immediately; this depends on crawl timing outside the site's
  control.
- **Single language (English only).** No localization.

## Built with AI — what and how

This site's HTML/CSS structure, the SEO/meta tag additions, the favicon, the
hardening review report, and this README were built in collaboration with
Claude (Anthropic). Specifically:
- Claude drafted the meta tag block (description, Open Graph, Twitter card)
  and the favicon SVG, which I reviewed and inserted into the live file myself.
- I directed and ran the "break it" testing (empty form, broken links, garbage
  input, cross-device checks) myself; Claude fetched the live page to confirm
  which links actually failed and helped me triage findings into fix-now vs.
  known-limitation.
- I personally verified every fix (CV download, form submission, meta tags)
  on the live deployed site before considering this checkpoint done — nothing
  here was accepted on Claude's word alone.
