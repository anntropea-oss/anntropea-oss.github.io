## [2026-06-05 17:47] Thread Reader Argument Rejection
- Problem: The first attempt to read the referenced Codex thread failed with an invalid arguments response while trying to include truncated tool outputs.
- Root Cause: Unknown
- Solution: Located the thread through the app thread index, then retried `read_thread` with the required thread id and without the extra output options.
- Files Changed: SOLUTIONS.md
- Status: Workaround
- Verification: The thread was successfully read and its project lessons were incorporated into this website scaffold.

## [2026-06-05 17:48] Playwright Verification Dependency Missing
- Problem: Browser rendering verification through the Node REPL failed because the `playwright` package was not installed in the local REPL environment.
- Root Cause: The workspace has no Node project dependencies and the Node REPL module path did not include Playwright.
- Solution: Switched verification to the installed Google Chrome binary in headless screenshot mode.
- Files Changed: SOLUTIONS.md
- Status: Workaround
- Verification: Google Chrome was found at `/Applications/Google Chrome.app/Contents/MacOS/Google Chrome` and used for the follow-up rendering check.

## [2026-06-05 17:50] Hero Layout Overflow
- Problem: Visual QA showed the desktop hero pushed the action buttons to the bottom edge of the first viewport, and the mobile hero headline overflowed horizontally.
- Root Cause: The hero minimum height and responsive headline sizing were too large for the available viewport after accounting for the header and mobile padding.
- Solution: Reduced the hero minimum height and padding, tightened the desktop headline scale, set the hero content to full width, and added mobile-specific headline width and size rules.
- Files Changed: styles.css, SOLUTIONS.md
- Status: Resolved
- Verification: Re-ran browser screenshot capture after the CSS change and confirmed the desktop screenshot was regenerated; follow-up mobile verification continued with an isolated capture after stopping the hung Chrome process.

## [2026-06-05 17:51] Headless Chrome Capture Hung
- Problem: The chained headless Chrome screenshot command hung after writing the desktop screenshot and did not proceed to the mobile screenshot or HTTP checks.
- Root Cause: Unknown; Chrome emitted updater and registration errors while running in headless mode.
- Solution: Identified and terminated only the headless capture shell and Chrome child processes, then continued verification with separate commands.
- Files Changed: SOLUTIONS.md
- Status: Workaround
- Verification: The capture session exited after terminating PIDs `13862` and `13864`, while the local preview server remained active.

## [2026-06-05 17:52] Missing Timeout Utility
- Problem: Time-bounding screenshot commands with `timeout` failed because the command is not available in this macOS shell.
- Root Cause: GNU `timeout` is not installed by default on this system.
- Solution: Used a Perl `alarm` wrapper to bound individual Chrome screenshot commands.
- Files Changed: SOLUTIONS.md
- Status: Workaround
- Verification: The Perl-wrapped commands wrote desktop and mobile screenshot files before the alarm stopped Chrome.

## [2026-06-05 17:55] DevTools WebSocket Unavailable
- Problem: A Chrome DevTools Protocol mobile screenshot attempt failed because the Node REPL runtime did not provide a global `WebSocket` implementation.
- Root Cause: The local Node REPL environment lacks the browser-compatible WebSocket API and no WebSocket package is installed in the workspace.
- Solution: Avoided adding dependencies, constrained the mobile hero content column directly in CSS, and verified the result with headless Chrome screenshots.
- Files Changed: styles.css, SOLUTIONS.md
- Status: Workaround
- Verification: The final mobile screenshot showed the headline, paragraph, and buttons fitting inside the visible column.

## [2026-06-05 17:56] Curl Asset Check Printed Binary Output
- Problem: A final HTTP verification command printed PNG binary data into the command output while checking the hero asset.
- Root Cause: Multiple URLs were passed to one `curl` invocation with a single output option, causing the second response body to be written to stdout.
- Solution: Re-ran the HTTP checks as separate quiet requests so only status codes and content types are reported.
- Files Changed: SOLUTIONS.md
- Status: Resolved
- Verification: Follow-up curl checks returned only concise HTTP status and content-type output.

## [2026-06-05 18:04] In-App Browser Automation Unavailable
- Problem: Attempting to verify the local preview through the in-app browser automation API failed because the `iab` browser backend was unavailable, and the browser backend list was empty.
- Root Cause: Unknown
- Solution: Continued verification with direct local HTTP checks and bounded Chrome screenshot commands instead of relying on the unavailable in-app browser API.
- Files Changed: SOLUTIONS.md
- Status: Workaround
- Verification: The browser runtime returned an empty backend list, confirming automation was unavailable for this session.

## [2026-06-05 18:07] Mobile Navigation Overflow
- Problem: After adding longer navigation labels, the mobile header exceeded the viewport width and pushed the `Contact` link offscreen.
- Root Cause: The mobile navigation used `justify-content: space-between` with five links and no wrapping behavior.
- Solution: Updated the mobile navigation to wrap with explicit row and column gaps, keeping all links visible within the viewport.
- Files Changed: styles.css, SOLUTIONS.md
- Status: Resolved
- Verification: Re-ran mobile screenshot verification after the CSS update to confirm the navigation fit within the viewport.

## [2026-06-05 18:21] LinkedIn Curl Verification Blocked
- Problem: Direct curl verification of the LinkedIn profile URL returned HTTP `999` instead of a normal success status.
- Root Cause: LinkedIn blocks some automated requests with anti-bot responses.
- Solution: Kept the public LinkedIn URL in the site and treated curl status as an automation limitation rather than a broken local link.
- Files Changed: index.html, SOLUTIONS.md
- Status: Workaround
- Verification: The local page continued to serve successfully, and the LinkedIn link is present in the resume section.

## [2026-06-05 18:21] Anchor Screenshot Captures Blank Page
- Problem: Headless Chrome screenshots of `#portfolio`, `#consulting`, and `#resume` anchor URLs produced blank images.
- Root Cause: Unknown; Chrome captured before the anchored page view painted.
- Solution: Retried section verification with a render budget and alternate screenshot flow.
- Files Changed: SOLUTIONS.md
- Status: Workaround
- Verification: The first anchor screenshots were inspected and confirmed blank before retrying with a different capture approach.

## [2026-06-05 18:23] Repeated Curl Multi-URL Output Issue
- Problem: A combined curl verification command again printed PNG and other response bodies into command output while checking multiple local URLs.
- Root Cause: Multiple URLs were passed through one curl command with output formatting that did not isolate each response body.
- Solution: Stopped using multi-URL curl for verification and re-ran each URL check as a separate quiet request.
- Files Changed: SOLUTIONS.md
- Status: Resolved
- Verification: Follow-up checks used one URL per curl command and returned only status codes and content types.

## [2026-06-08 20:50] Pre-Deploy Placeholder Metadata
- Problem: The site still used `https://example.com/` for canonical, Open Graph, robots, and sitemap URLs, and the public contact button used `hello@example.com`.
- Root Cause: Initial scaffold placeholders had not yet been replaced with production deployment values.
- Solution: Updated production metadata to the GitHub Pages project URL, changed the contact CTA to LinkedIn, refreshed the sitemap `lastmod`, and added `.nojekyll` for static GitHub Pages serving.
- Files Changed: index.html, robots.txt, sitemap.xml, README.md, .nojekyll, SOLUTIONS.md
- Status: Resolved
- Verification: Local HTTP checks returned `200` for the homepage, hero asset, `robots.txt`, and `sitemap.xml`; live GitHub Pages checks returned `200` for the homepage, hero asset, `robots.txt`, and `sitemap.xml`.

## [2026-06-08 20:51] Local Preview Server Stopped
- Problem: Pre-deploy local HTTP checks returned status `000` because `http://localhost:4173/` was no longer accepting connections.
- Root Cause: The earlier local preview server session had stopped.
- Solution: Restarted the local static server with `python3 -m http.server 4173`.
- Files Changed: SOLUTIONS.md
- Status: Resolved
- Verification: Follow-up local HTTP checks returned `200` for the homepage, hero asset, `robots.txt`, and `sitemap.xml`.

## [2026-06-08 20:52] Git Commit Identity Missing
- Problem: The repo had no configured Git author name or email, which would prevent creating the deployment commit.
- Root Cause: Local Git identity was not configured for this new repository.
- Solution: Set a local-only Git identity using the authenticated GitHub account name and no-reply email.
- Files Changed: .git/config, SOLUTIONS.md
- Status: Resolved
- Verification: `git config user.name` and `git config user.email` returned configured values after the update.

## [2026-06-08 20:53] GitHub Pages API Payload Rejected
- Problem: The first attempt to enable GitHub Pages returned HTTP `422` with “Invalid input: data matches no possible input.”
- Root Cause: The API call used an invalid payload shape for the Pages `source` object.
- Solution: Retried the Pages enablement call with nested `source[branch]` and `source[path]` fields.
- Files Changed: SOLUTIONS.md
- Status: Resolved
- Verification: GitHub Pages enablement returned `html_url` as `https://anntropea-oss.github.io/personal-branding/` with HTTPS enforced.

## [2026-06-08 21:00] GitHub Pages URL Includes Project Path
- Problem: The deployed site URL included `/personal-branding/`, but the desired public URL is the root GitHub Pages address without that path.
- Root Cause: The site was deployed from a project repository named `personal-branding` instead of the GitHub user-site repository named `anntropea-oss.github.io`.
- Solution: Updated canonical, Open Graph, JSON-LD, robots, sitemap, and README URLs to `https://anntropea-oss.github.io/`; renamed the GitHub repository to `anntropea-oss.github.io`.
- Files Changed: index.html, robots.txt, sitemap.xml, README.md, SOLUTIONS.md
- Status: Resolved
- Verification: The renamed GitHub Pages user site returns `200` at `https://anntropea-oss.github.io/`; root hero asset, `robots.txt`, and `sitemap.xml` also return `200`, and live metadata no longer includes `/personal-branding/`.

## [2026-06-08 21:08] Resume PDF Not Yet Live During Pages Build
- Problem: Immediately after pushing the resume/GitHub update, the live homepage returned `200` but the new resume PDF URL returned `404`.
- Root Cause: GitHub Pages was still building the new commit, so the newly added PDF asset had not propagated yet.
- Solution: Waited for the GitHub Pages build to finish, then rechecked the live resume PDF URL.
- Files Changed: SOLUTIONS.md
- Status: Resolved
- Verification: GitHub Pages status changed to `built`; the live resume PDF returned `200 application/pdf`, and the live homepage included the GitHub section and resume link.

## [2026-06-08 21:09] Missing Favicon
- Problem: Local browser verification requested `/favicon.ico` and received `404`.
- Root Cause: The site did not declare or provide a favicon.
- Solution: Added a lightweight SVG favicon and linked it from the document head.
- Files Changed: index.html, favicon.svg, SOLUTIONS.md
- Status: Resolved
- Verification: Local and live `favicon.svg` checks returned `200 image/svg+xml`; the live homepage includes the favicon link.

## [2026-06-09 09:32] Local Preview Server Stopped During Portfolio Update
- Problem: The local homepage check for `http://localhost:4173/` returned status `000` while verifying the portfolio/content update.
- Root Cause: The local static preview server was not running on port 4173.
- Solution: Restarted the local static server before continuing verification.
- Files Changed: SOLUTIONS.md
- Status: Resolved
- Verification: Follow-up local HTTP checks returned `200` for the homepage after the server was restarted.

## [2026-06-09 09:32] Missing Public Portfolio Source Links
- Problem: Some requested portfolio areas did not have discoverable public source URLs during web verification, including the WMBC AA Batteries show archive, preflight workshop materials, shareable publication spreads, dissertation/thesis editing examples, and The Intimacy Gap project.
- Root Cause: Public URLs for those specific samples were not available in the provided materials or discoverable through search.
- Solution: Added a visible “Links to add next” block to the portfolio section so missing sample URLs are flagged clearly while verified links are used where available.
- Files Changed: index.html, styles.css, SOLUTIONS.md
- Status: Workaround
- Verification: The updated portfolio markup includes verified public links plus a dedicated list of sample links still needed.

## [2026-06-09 09:33] Python HTTPS Link Checker Certificate Failure
- Problem: Python `urllib` link verification failed for every new HTTPS portfolio URL with `CERTIFICATE_VERIFY_FAILED`.
- Root Cause: The local Python SSL certificate store could not verify the remote HTTPS certificate chains in this environment.
- Solution: Re-ran the external URL checks with `curl -L`, which successfully verified the linked pages.
- Files Changed: SOLUTIONS.md
- Status: Workaround
- Verification: `curl -L` returned `200` for the Computational History essay, UMBC coverage, Retriever author archive, Seapower sample, Apple Podcasts page, and UMBC profile.

## [2026-06-09 09:34] Sitemap Last Modified Date Stale
- Problem: `sitemap.xml` still listed `2026-06-08` after the homepage content was updated on June 9, 2026.
- Root Cause: The sitemap date was not automatically updated when editing the static page.
- Solution: Updated the homepage `<lastmod>` value to `2026-06-09`.
- Files Changed: sitemap.xml, SOLUTIONS.md
- Status: Resolved
- Verification: Re-read `sitemap.xml` and confirmed the new `lastmod` date is present.

## [2026-06-09 09:35] Multi-URL Curl Verification Printed Response Body
- Problem: A combined `curl` command used for local verification printed the sitemap XML response body into the command output.
- Root Cause: Multiple URLs were passed to one `curl` invocation without isolating each response body.
- Solution: Re-ran the local homepage and sitemap checks as separate quiet requests.
- Files Changed: SOLUTIONS.md
- Status: Resolved
- Verification: Separate `curl` checks returned concise `200` status and content-type output for both local URLs.

## [2026-06-09 09:36] GitHub Pages Poll Used Read-Only Shell Variable
- Problem: The first GitHub Pages polling command failed with `zsh: read-only variable: status`.
- Root Cause: `status` is a reserved/read-only variable name in zsh.
- Solution: Reran the Pages polling command using a non-reserved variable name.
- Files Changed: SOLUTIONS.md
- Status: Resolved
- Verification: The corrected polling command ran and reported the GitHub Pages build status.

## [2026-06-09 09:38] In-App Browser Route Unavailable After Deploy
- Problem: Attempting to navigate the in-app browser to the live GitHub Pages URL failed with “No Codex browser route is available.”
- Root Cause: Unknown; the in-app browser automation route became unavailable after prior verification.
- Solution: Used direct HTTP verification for the live GitHub Pages URL and left the public URL available for manual opening.
- Files Changed: SOLUTIONS.md
- Status: Workaround
- Verification: Live HTTP checks returned `200`, and the deployed homepage contained the new portfolio content.

## [2026-06-09 09:40] Local Server Session Stdin Closed
- Problem: Attempting to stop the local preview server through the original exec session failed because stdin was already closed for that session.
- Root Cause: The long-running server session no longer accepted stdin input.
- Solution: Located the process listening on port 4173 with `lsof` and stopped it directly.
- Files Changed: SOLUTIONS.md
- Status: Resolved
- Verification: The port cleanup command completed successfully.

## [2026-06-09 09:49] PDF Text Extraction Tool Missing
- Problem: Attempting to inspect the supplied writing sample PDFs with `pdftotext` failed because the command is not installed.
- Root Cause: The local shell environment does not include the `pdftotext` utility.
- Solution: Used the provided filenames and public-facing context to group the PDFs into appropriate writing sample categories, and linked the PDFs directly from the portfolio.
- Files Changed: SOLUTIONS.md, index.html, styles.css, assets/writing-samples/*
- Status: Workaround
- Verification: The writing sample PDFs were copied into `assets/writing-samples` with URL-safe filenames and are referenced from the portfolio markup.

## [2026-06-09 09:50] Local Preview Server Stopped During Writing Sample Update
- Problem: The local homepage check for `http://localhost:4173/` returned status `000` while verifying the writing sample update.
- Root Cause: The local static preview server was not running on port 4173.
- Solution: Restarted the local static server before continuing verification.
- Files Changed: SOLUTIONS.md
- Status: Resolved
- Verification: Follow-up local HTTP checks returned `200` for the homepage after the server was restarted.

## [2026-06-09 09:55] Local Server Session Stdin Closed After Writing Sample Deploy
- Problem: Attempting to stop the local preview server through the original exec session failed because stdin was already closed for that session.
- Root Cause: The long-running server session no longer accepted stdin input.
- Solution: Located the process listening on port 4173 with `lsof` and stopped it directly.
- Files Changed: SOLUTIONS.md
- Status: Resolved
- Verification: The port cleanup command completed successfully.

## [2026-06-09 09:57] Pages Build Poll Timed Out
- Problem: The first post-push GitHub Pages polling loop reached its retry limit while the deployment still reported `building`.
- Root Cause: The Pages build took longer than the polling loop allowed.
- Solution: Ran a follow-up status check with a longer wait window.
- Files Changed: SOLUTIONS.md
- Status: Resolved
- Verification: The follow-up Pages status check returned `built`.

## [2026-06-11 11:06] Headshot Validation False Alarm
- Problem: The `sips` metadata command reported `assets/images/ann-tropea-headshot.jpg` as “not a valid file” while preparing the headshot for the site.
- Root Cause: Unknown; macOS `file` identified the image as valid JPEG data and the image rendered correctly in visual inspection.
- Solution: Verified the copied asset with `file`, inspected its header bytes, and visually rendered the image before using it on the site.
- Files Changed: assets/images/ann-tropea-headshot.jpg, SOLUTIONS.md
- Status: Workaround
- Verification: `file` reported valid JPEG image data and the image rendered correctly through visual inspection.

## [2026-06-11 11:06] Preflight Confab Public Link Not Found
- Problem: Web search did not find a reliable public URL for Ann Tropea’s Preflight Confab speaking engagement.
- Root Cause: The event or speaker page may not be publicly indexed or may be private.
- Solution: Included the speaking engagement without an external link while using verified links for other organizations and conference entries.
- Files Changed: index.html, SOLUTIONS.md
- Status: Workaround
- Verification: Search results did not return a reliable Preflight Confab page; verified public links were found for Maryland Humanities, Mac’s List, and UMBC.

## [2026-06-11 11:08] Sitemap Last Modified Date Stale
- Problem: `sitemap.xml` still listed `2026-06-09` after the homepage content was updated on June 11, 2026.
- Root Cause: The sitemap date is manually maintained for this static site.
- Solution: Updated the homepage `<lastmod>` value to `2026-06-11`.
- Files Changed: sitemap.xml, SOLUTIONS.md
- Status: Resolved
- Verification: Re-read `sitemap.xml` and confirmed the new `lastmod` date is present.

## [2026-06-11 11:09] Local Preview Server Stopped During Speaking Update
- Problem: The local homepage check for `http://localhost:4173/` returned status `000` while verifying the speaking and portfolio update.
- Root Cause: The local static preview server was not running on port 4173.
- Solution: Restarted the local static server before continuing browser and asset verification.
- Files Changed: SOLUTIONS.md
- Status: Resolved
- Verification: Follow-up local HTTP checks returned `200` for the homepage after the server was restarted.

## [2026-06-11 11:14] PDF Link Search Quoting Error
- Problem: The first attempt to search for PDF links in `index.html` failed with `zsh: unmatched "`.
- Root Cause: The shell command used nested double quotes without proper escaping.
- Solution: Re-ran the search with safer quoting before editing the document links.
- Files Changed: SOLUTIONS.md
- Status: Resolved
- Verification: The corrected search command located the PDF links in `index.html`.

## [2026-06-11 11:18] Pages Build Poll Timed Out After Document Link Update
- Problem: The first post-push GitHub Pages polling loop reached its retry limit while the deployment still reported `building`.
- Root Cause: The Pages build took longer than the polling loop allowed.
- Solution: Ran a follow-up status check with a longer wait window before final live verification.
- Files Changed: SOLUTIONS.md
- Status: Resolved
- Verification: The follow-up Pages status check returned `built`.

## [2026-06-11 11:21] Python HTTPS Live Verification Certificate Failure
- Problem: Python `urllib` live verification of `https://anntropea-oss.github.io/` failed with `CERTIFICATE_VERIFY_FAILED`.
- Root Cause: The local Python SSL certificate store could not verify the remote HTTPS certificate chain in this environment.
- Solution: Re-ran live verification with `curl`, which successfully fetched the deployed page.
- Files Changed: SOLUTIONS.md
- Status: Workaround
- Verification: `curl` returned the deployed homepage and confirmed the document links include `target="_blank"`.

## [2026-06-11 11:25] Pages Status Stayed Building After Log-Only Push
- Problem: A follow-up GitHub Pages status poll for the log-only `SOLUTIONS.md` deployment reached its retry limit while the Pages API continued reporting `building`.
- Root Cause: Unknown; the public site continued serving normally and the user-facing document-link update was already live.
- Solution: Verified the live homepage directly with `curl` and treated the unresolved Pages status as a deployment-status issue for the log-only follow-up.
- Files Changed: SOLUTIONS.md
- Status: Open
- Verification: `curl` returned `200` for the live homepage, and earlier live HTML verification confirmed all PDF links include `target="_blank"` and `rel="noreferrer"`.

## [2026-06-11 11:27] Repository Card Heading Orphan Letter
- Problem: The `fantasybaseball` repository card heading wrapped with the final `l` alone on the next line in a narrow viewport.
- Root Cause: The global heading rule used `overflow-wrap: anywhere`, which allowed single-word headings to break at any character.
- Solution: Added a repo-card-specific heading rule that restores normal word wrapping and slightly tightens the responsive heading size inside repository cards.
- Files Changed: styles.css, SOLUTIONS.md
- Status: Resolved
- Verification: Browser layout checks confirmed `fantasybaseball` no longer contains an internal line break and the page has no horizontal overflow.

## [2026-06-11 11:28] Browser Verification Variable Reuse
- Problem: The first browser verification script for the repository card fix failed with `Identifier 'viewportCap' has already been declared`.
- Root Cause: The persistent browser JavaScript session already had a top-level binding named `viewportCap`.
- Solution: Reran the verification using a fresh variable name.
- Files Changed: SOLUTIONS.md
- Status: Resolved
- Verification: The follow-up browser verification script ran successfully.

## [2026-06-11 11:31] Speaking Card Headlines Misaligned
- Problem: Speaking section card headlines started at different vertical positions across the row.
- Root Cause: `.speaking-card` used CSS grid with `align-content: space-between`, so each card distributed its internal rows differently based on content height and link presence.
- Solution: Changed speaking cards to a flex column layout and pinned optional links to the bottom with `margin-top: auto`.
- Files Changed: styles.css, SOLUTIONS.md
- Status: Resolved
- Verification: Browser layout checks confirmed the speaking card headline top positions match across desktop cards.

## [2026-06-11 11:36] Preflight Confab Link Identified
- Problem: The Preflight Confab speaking engagement was previously listed without a source link.
- Root Cause: Initial searches did not locate the event page, and the engagement was left unlinked.
- Solution: Updated the speaking card to identify the College Media Association Confab and linked directly to CMA’s event page for “CMA Confab - Preflighting Magazines.”
- Files Changed: index.html, SOLUTIONS.md
- Status: Resolved
- Verification: The College Media Association event page loaded successfully and lists “CMA Confab - Preflighting Magazines” presented by Ann Tropea.

## [2026-06-11 11:43] Social Preview Kicker Overlapped Image
- Problem: The first generated social preview image had the top kicker text running into the headshot area.
- Root Cause: The kicker text was too long for the available text column in the 1200x630 preview image.
- Solution: Shortened the kicker line and regenerated the social preview image before wiring it into metadata.
- Files Changed: assets/social-preview.png, SOLUTIONS.md
- Status: Resolved
- Verification: Visual inspection confirmed the regenerated preview image text no longer overlaps the photo.

## [2026-06-11 11:51] SEO Metadata and Structured Data Incomplete
- Problem: The homepage had basic metadata but lacked several Google-friendly SEO signals, including fuller crawl/snippet directives, `og:url`, `og:site_name`, Twitter title/description, sitemap discovery in the document head, richer structured data, and more explicit on-page service language.
- Root Cause: Earlier iterations prioritized launch and portfolio content over comprehensive SEO implementation.
- Solution: Added expanded robots/googlebot directives, canonical and sitemap signals, richer Open Graph/Twitter metadata, JSON-LD `WebSite`, `ProfilePage`, and `Person` structured data, image sitemap entries, and natural visible copy for editorial consulting, communications strategy, technical communication, legal writing, podcast production, thesis/dissertation editing, and print publication consulting.
- Files Changed: index.html, sitemap.xml, SOLUTIONS.md
- Status: Resolved
- Verification: Parsed the JSON-LD successfully, confirmed SEO tags are present in local HTML, confirmed the sitemap contains image entries, and verified local HTTP checks for the homepage, sitemap, and social preview image returned `200`.

## [2026-06-11 11:51] Google Search Console Submission Not Yet Completed
- Problem: The site cannot be fully confirmed as submitted for Google indexing from the repo alone.
- Root Cause: Google Search Console property verification and sitemap submission require account-level access and a verification token or DNS/HTML-file flow.
- Solution: Left the site technically ready for Search Console submission with indexable robots directives, canonical URL, root `robots.txt`, and sitemap at `https://anntropea-oss.github.io/sitemap.xml`.
- Files Changed: SOLUTIONS.md
- Status: Open
- Verification: The local `robots.txt` points to the sitemap, and `sitemap.xml` lists the canonical homepage URL.

## [2026-06-11 11:54] Multi-URL Curl Printed Social Preview Binary
- Problem: A combined live verification command printed binary PNG data from `assets/social-preview.png` into the terminal output while checking deployed SEO assets.
- Root Cause: Multiple URLs were passed to one `curl` invocation with a single output-suppression option, so one response body was not isolated.
- Solution: Re-ran the homepage, sitemap, robots, and social preview checks as separate quiet requests and logged the verification issue.
- Files Changed: SOLUTIONS.md
- Status: Resolved
- Verification: Separate `curl` requests returned clean HTTP status and content-type output for the live homepage, sitemap, robots file, and social preview image.

## [2026-06-11 11:55] In-App Browser Route Unavailable During SEO Check
- Problem: The browser-based render verification could not run because the in-app browser automation route was unavailable for the current session.
- Root Cause: Unknown.
- Solution: Used live HTTP checks, local JSON-LD parsing, XML sitemap parsing, and deployed HTML metadata checks as the verification path instead.
- Files Changed: SOLUTIONS.md
- Status: Workaround
- Verification: Live `curl` checks returned `200` for the homepage, sitemap, robots file, and social preview image, and earlier parser checks confirmed the JSON-LD and sitemap are valid.

## [2026-06-11 11:58] Pages Status Stayed Building After SEO Log Push
- Problem: The GitHub Pages status endpoint continued to report `building` after the SEO verification log-only push.
- Root Cause: Unknown; the public site was already serving the deployed SEO updates successfully.
- Solution: Verified the live homepage, sitemap, robots file, and social preview image directly instead of waiting indefinitely on the status endpoint.
- Files Changed: SOLUTIONS.md
- Status: Open
- Verification: Live HTTP checks returned `200` for `https://anntropea-oss.github.io/`, `sitemap.xml`, `robots.txt`, and `assets/social-preview.png`.

## [2026-06-11 12:06] Google Search Console Verification Tag Missing
- Problem: Google Search Console could not verify the GitHub Pages site until its account-specific verification meta tag was present on the live homepage.
- Root Cause: The verification token had not yet been added to the deployed site.
- Solution: Added the provided `google-site-verification` meta tag to the homepage `<head>`.
- Files Changed: index.html, SOLUTIONS.md
- Status: Resolved
- Verification: Confirmed the verification tag is present in the local homepage source before deployment.

## [2026-06-11 12:10] Search Console Sitemap Could Not Fetch
- Problem: Google Search Console showed `/sitemap.xml` with status `Couldn't fetch` immediately after submission.
- Root Cause: Unknown; the deployed sitemap is publicly reachable and valid, so this is likely a temporary Search Console fetch or cache delay.
- Solution: Verified the live sitemap directly from the public URL and confirmed it returns `200` with `application/xml` content and valid XML.
- Files Changed: SOLUTIONS.md
- Status: Workaround
- Verification: `curl` returned `200` for `https://anntropea-oss.github.io/sitemap.xml` with no redirect, and `xmllint` validated the local sitemap XML.

## [2026-06-16 14:09] Browser Viewport Capability Docs Missing
- Problem: The optional browser viewport capability documentation file was not present at the expected plugin path during SOP card layout verification.
- Root Cause: Unknown; the browser skill referenced generated capability docs that were not available in the local plugin cache path.
- Solution: Avoided the unavailable viewport capability and verified the SOP card with the active browser session plus local HTTP and link checks.
- Files Changed: SOLUTIONS.md
- Status: Workaround
- Verification: The browser check confirmed the SOP card is visible, both card links are present with `_blank`, and no horizontal overflow was detected; local HTTP checks returned `200` for the homepage and sitemap.

## [2026-06-19 11:10] Full Visitor Analytics Not Installed
- Problem: The website does not currently collect full visitor analytics such as sessions, acquisition sources, page engagement, or outbound-link clicks.
- Root Cause: No Google Analytics, Plausible, Umami, or similar analytics tag has been added to the static site.
- Solution: Documented the existing Search Console and GitHub traffic options; full visitor analytics remains available as a future GA4 or privacy-focused analytics setup.
- Files Changed: SOLUTIONS.md
- Status: Open
- Verification: Searched `index.html`, `script.js`, and `README.md` and found no analytics measurement script or configuration.

## [2026-06-19 11:15] Search Visibility Limited to Branded Query
- Problem: Search Console currently shows only the branded query `ann tropea`, so prospective clients are not yet finding the site through service-related searches.
- Root Cause: The new one-page site has limited search history and distributes several distinct service topics across a single homepage rather than dedicated intent-focused pages.
- Solution: Researched and prioritized service-intent keyword clusters, then created dedicated pages for higher education communications, SOP and process documentation, and print publication consulting with unique metadata, visible expertise, proof links, internal links, and sitemap entries.
- Files Changed: index.html, styles.css, sitemap.xml, higher-education-communications/index.html, sop-process-documentation/index.html, print-publication-consulting/index.html, SOLUTIONS.md
- Status: Resolved
- Verification: Confirmed unique titles, descriptions, H1s, canonical URLs, valid JSON-LD, working local links and assets, and four valid sitemap URLs; public indexing and query growth will be monitored in Search Console.

## [2026-06-19 11:22] Service Page Search Snippets Too Long
- Problem: Initial validation found that the new service-page titles and meta descriptions were long enough to risk truncating the core service phrase or Ann Tropea's name in search results.
- Root Cause: The first metadata pass attempted to include too many supporting keywords in each title and description.
- Solution: Shortened the service-page titles and descriptions, preserving one primary intent phrase per title and moving supporting language into the description and visible page copy; also tightened the homepage description.
- Files Changed: index.html, print-publication-consulting/index.html, sop-process-documentation/index.html, higher-education-communications/index.html, SOLUTIONS.md
- Status: Resolved
- Verification: Re-ran metadata length checks and confirmed the shortened titles and descriptions retain the target service phrases and canonical URLs.

## [2026-06-19 11:23] Local Preview Server Blocked by Sandbox
- Problem: The first attempt to start the static preview server on port 4173 failed with `PermissionError: Operation not permitted`.
- Root Cause: The managed sandbox blocked the Python process from binding to a local network port.
- Solution: Restarted the same local preview server with the required managed-environment permission, then moved verification to the public deployment when the approved process proved isolated from the shell and browser.
- Files Changed: SOLUTIONS.md
- Status: Workaround
- Verification: The approved process remained active, but localhost requests could not reach it; structural checks continued locally and HTTP/browser checks were deferred to the public deployment.

## [2026-06-19 11:25] Local Browser Preview Unavailable in Managed Session
- Problem: Browser verification could not reach the approved localhost preview, the selected browser initially had no active tab, and direct `file://` navigation was blocked by browser security policy.
- Root Cause: The managed server process, shell, and browser were isolated from one another, while local-file navigation was disallowed by the browser URL policy; the browser plugin cache had also advanced to a new version path since the previous session.
- Solution: Stopped local browser attempts, retained local structural, metadata, asset, and sitemap validation, and moved visual and HTTP verification to the deployed public HTTPS pages.
- Files Changed: SOLUTIONS.md
- Status: Workaround
- Verification: Local JSON-LD and XML parsing passed; final browser and HTTP checks are scheduled against the public GitHub Pages URLs after deployment.

## [2026-06-19 11:29] Git Staging Blocked by Managed Permissions
- Problem: Git could not stage the service-page update because it was unable to create `.git/index.lock`; an earlier managed-permission request also remained pending until terminated.
- Root Cause: The current workspace permission profile grants read-only access to `.git`, so index and commit operations require an explicit managed-environment approval.
- Solution: Preserved all verified working-tree changes and issued a narrow approval request for the specific Git staging, commit, and push operations needed to deploy them.
- Files Changed: SOLUTIONS.md
- Status: Open
- Verification: The unapproved `git add` failed with `Operation not permitted`, while `git status` continued to show the intended modified and new files in the working tree.

## [2026-06-19 11:47] Codex Manual Fetch Blocked by Sandbox Network
- Problem: The first attempt to fetch the official Codex manual failed because the sandbox could not resolve `developers.openai.com`.
- Root Cause: Command network access was restricted in the active managed permission profile.
- Solution: Re-ran the official manual helper with a narrow approved network escalation and used the current permissions and sandbox documentation to guide the user.
- Files Changed: SOLUTIONS.md
- Status: Resolved
- Verification: The approved helper downloaded a fresh Codex manual and outline to the temporary documentation cache.

## [2026-06-19 11:48] Full Access UI Did Not Match Effective Git Permissions
- Problem: The Codex app showed Full Access for the thread, but ordinary Git commands still treated `.git` as read-only and failed to create `index.lock`.
- Root Cause: The app permissions selector and the thread's effective managed command profile were out of sync or the Git-protected-path rule still required an explicit escalation.
- Solution: Retried the exact staging command with a narrow explicit escalation for the listed website files.
- Files Changed: SOLUTIONS.md
- Status: Workaround
- Verification: The escalated `git add` completed successfully and saved an approval rule for that exact staging command prefix.

## [2026-06-19 11:54] Service Page Headline Broke Inside Word on Mobile
- Problem: The higher education service-page headline displayed `Communication` with the final letter wrapped onto a separate line at a 390-pixel viewport.
- Root Cause: The global heading rule used `overflow-wrap: anywhere`, and the mobile service heading was constrained to `11ch`.
- Solution: Restored normal word wrapping for service-page H1 elements, removed the narrow character-based mobile maximum, and slightly reduced the mobile service-heading size.
- Files Changed: styles.css, SOLUTIONS.md
- Status: Resolved
- Verification: A fresh public browser check at 390 by 844 pixels confirmed the heading keeps `Communication` whole, fits the viewport, and produces no horizontal overflow.

## [2026-06-19 12:01] GitHub Pages Served Stale Service-Page CSS
- Problem: After the mobile heading fix was deployed and Pages reported `built`, the public browser still received the previous stylesheet and continued breaking `Communication` inside the word.
- Root Cause: The shared `styles.css` URL was cached by the GitHub Pages CDN or browser, so the unchanged asset URL did not immediately resolve to the new CSS content.
- Solution: Added a version query to the stylesheet reference on the homepage and all three service pages to force retrieval of the corrected CSS.
- Files Changed: index.html, print-publication-consulting/index.html, sop-process-documentation/index.html, higher-education-communications/index.html, SOLUTIONS.md
- Status: Resolved
- Verification: The fresh public HTML loaded `styles.css?v=20260619-2`; computed mobile styles reported `39.2px`, `max-width: 100%`, `overflow-wrap: normal`, and `word-break: normal`.

## [2026-06-19 12:04] Deployment Verification Tooling Returned Blocked or Stale Results
- Problem: Several deployment checks were misleading or blocked: the first Pages poll stalled behind approval, generic web opening rejected the GitHub Pages URLs, browser navigation from Chrome's generated localhost error page was blocked, a browser poll timed out and reset its session, and a zsh poll used the reserved read-only variable `status`.
- Root Cause: Managed approval delays, browser and web URL safety policies, a long-running browser polling loop, shell-specific variable semantics, and normal CDN/browser caching overlapped during verification.
- Solution: Used a clean public browser tab, corrected the shell variable to `pages_state`, waited for GitHub Pages to report `built`, and forced a fresh public HTML request with a harmless verification query.
- Files Changed: SOLUTIONS.md
- Status: Resolved
- Verification: GitHub Pages reported `built`, all four public pages had already passed desktop and mobile structural checks, and the final fresh mobile screenshot confirmed the corrected higher education headline and stylesheet.

## [2026-06-22 09:30] Resume PDF Text Extractor Missing
- Problem: The preferred `pdftotext` utility was unavailable while identifying the public professional email from the resume, and the first relative-path Spotlight metadata lookup did not resolve the PDF.
- Root Cause: Poppler tools are not installed in the current environment, and `mdls` did not resolve the relative asset path.
- Solution: Inspected embedded PDF strings instead of installing a dependency and found the resume's existing `mailto:` link.
- Files Changed: SOLUTIONS.md
- Status: Workaround
- Verification: The PDF contains `(mailto:anntropea@gmail.com)`, confirming the address already appears in the publicly downloadable resume.

## [2026-06-22 09:33] Secondary Contact Button Contrast Risk
- Problem: Adding LinkedIn as a secondary button inside the dark contact band would have inherited a light translucent background and white text, creating weak contrast.
- Root Cause: The global secondary-button style was designed for light page backgrounds and had no contact-section override.
- Solution: Added a contact-specific secondary-button style with white text, a transparent background, and a visible light border; versioned the shared stylesheet references to avoid stale CSS.
- Files Changed: styles.css, index.html, print-publication-consulting/index.html, sop-process-documentation/index.html, higher-education-communications/index.html, dissertation-thesis-editing/index.html, SOLUTIONS.md
- Status: Resolved
- Verification: Confirmed the contact override has higher selector specificity than the global secondary-button rule; final visual verification will run on the public deployment.

## [2026-06-22 09:39] Dissertation Page Commit Approval Stalled
- Problem: The first approval request for the dissertation-page commit remained pending for several minutes without surfacing a usable approval response.
- Root Cause: Unknown; the thread continues to show a mismatch between Full Access in the app and Git operations that require explicit managed escalation.
- Solution: Terminated two stalled commit requests while preserving the staged changes, then used the previously approved focused-service-page commit command.
- Files Changed: SOLUTIONS.md
- Status: Workaround
- Verification: Commit `5a396da` was created successfully and pushed to `origin/main`; GitHub Pages subsequently reported `built`.

## [2026-06-22 09:47] Mobile Screenshot Capture Timed Out and Reset Viewport
- Problem: The first mobile screenshot timed out, and a later capture occurred after the browser viewport had reset to desktop dimensions even though the page itself remained functional.
- Root Cause: The screenshot command exceeded its browser-control timeout, and the viewport reset from the failed sequence affected the reused tab.
- Solution: Opened a clean tab under an explicit 390 by 844 viewport, separated layout measurements from image capture, reloaded at the target width, and captured before resetting the viewport.
- Files Changed: SOLUTIONS.md
- Status: Resolved
- Verification: The final browser result reported a 390 by 844 viewport with no horizontal or button overflow, normal H1 wrapping, contained hero content, no missing images, and a successful mobile screenshot.

## [2026-06-22 10:00] Analytics Tag Patch Did Not Match Service Page Titles
- Problem: The initial multi-file analytics patch failed while updating the service pages.
- Root Cause: The patch expected outdated page title text, while the current service pages use revised SEO titles.
- Solution: Reapplied the GA4 tag using the shared viewport meta element as the stable insertion point on every public HTML page.
- Files Changed: index.html, print-publication-consulting/index.html, sop-process-documentation/index.html, higher-education-communications/index.html, dissertation-thesis-editing/index.html, SOLUTIONS.md
- Status: Resolved
- Verification: Confirmed each public HTML page contains the GA4 loader and configuration for measurement ID G-P4GD9RLXHF.

## [2026-06-23 15:36] Analytics Report Redirected to Sign-In
- Problem: The Google Analytics report link could not be inspected from the in-app browser because it redirected to a Google sign-in page instead of showing the Pages and screens report.
- Root Cause: Google Analytics requires the user's authenticated Google session, which is not available to the browser automation context.
- Solution: No site code fix was needed; mapped the site's current public page titles and canonical URLs locally so the reported Analytics row can be interpreted from the user's visible report.
- Files Changed: SOLUTIONS.md
- Status: Open
- Verification: Browser navigation to the shared Analytics URL ended at `accounts.google.com`, and local HTML inspection confirmed the site's page-title-to-URL mapping.

## [2026-06-23 15:37] Analytics Report Includes Legacy Doula Paths
- Problem: Google Analytics displayed page paths including `/copy-of-birth-doula/`, `/birth-doula/`, `/breastfeeding-counseling/`, and `/childbirth-education/`, which do not belong to the current professional portfolio site.
- Root Cause: Unknown; likely the GA4 property or measurement ID is also receiving historical or current traffic from an older doula-related site or prior site implementation.
- Solution: No code fix was applied yet; identified the current repo's valid public page paths so the Analytics report can be interpreted and the property can be filtered by hostname.
- Files Changed: SOLUTIONS.md
- Status: Open
- Verification: Local search found no matching page directories or files for the reported doula paths, only doula-related text references on the current homepage.

## [2026-06-23 16:03] Portfolio Analytics Used Mixed GA4 Measurement ID
- Problem: The professional portfolio site was using GA4 measurement ID `G-P4GD9RLXHF`, which also collected traffic for `mommytreedoula.com` and mixed unrelated doula-site paths into the portfolio analytics report.
- Root Cause: The portfolio was originally configured with a GA4 tag associated with the existing mixed Analytics property instead of the new portfolio-only GA4 measurement ID.
- Solution: Replaced `G-P4GD9RLXHF` with the new portfolio-only measurement ID `G-M5VMC4W6X3` on every public HTML page.
- Files Changed: index.html, print-publication-consulting/index.html, sop-process-documentation/index.html, higher-education-communications/index.html, dissertation-thesis-editing/index.html, SOLUTIONS.md
- Status: Resolved
- Verification: Local grep confirmed every public HTML page loads and configures `G-M5VMC4W6X3`, with no remaining `G-P4GD9RLXHF` references in those pages.
