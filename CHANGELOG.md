# Changelog

All notable changes to the **PCAP SIP Analyzer** (COEO AI Enablement).

Format: each entry is a semantic version and a summary of what changed. Newest first.

## v2.9.11

Call-flow lifelines now labeled: each IP column shows its role (client UA / proxy / registrar / internal node / endpoint) and public/private, plus the SIP domain when the packets carry one (e.g. registrar → snt-mt1.voip.chi.snetcom.net with platform type). No DNS is invented — labels come from the SIP headers and role inference.

## v2.9.10

Added a note under the Call flow ladder explaining it shows SIP signaling only; Operations Monitor traces contain no DNS lookups or RTP audio, so those steps never appear even on a healthy call (addresses shown are already-resolved SIP endpoints).

## v2.9.9

LinkedIn credit now uses the official LinkedIn brand blue (#0A66C2) for the icon and name in light mode, and a lightened LinkedIn blue (#4DA0E8) in dark mode for legibility.

## v2.9.8

Added a "Created by Ron Mangune" credit line in the footer with a LinkedIn icon linking to https://www.linkedin.com/in/ronmangune/ (opens in a new tab).

## v2.9.7

Platform tenancy rule corrected: the multi-tenant "mt" marker is now detected in both forms (hyphenated snt-mt1/snt-mt and as a segment mt1/snt.mt), with word-boundary matching so it never false-positives inside words like "management"; every other voip.<region>.snetcom.net host (including the bare region domain) is classified Standalone Phone System. Backed by a 13-case host unit test.

## v2.9.6

Fixed the logo not displaying: the show rule (.logo-dark) had lower CSS specificity than the base img.logo display:none, so the base rule won and hid the logo. Raised the override to img.logo.logo-dark. Logo now renders in both modes.

## v2.9.5

Updated to the full COEO "Business Connectivity" logo (light-on-navy variant for the banner in both modes, taller to fit the tagline); trimmed the header eyebrow to "AI Enablement" since the wordmark now appears in the logo.

## v2.9.4

Fixed the banner hiding the logo: the header background was tracking --ink (navy in light, near-white in dark), so each mode showed the same-colored logo on the same-colored band. Header is now a fixed brand navy in light mode and a deeper navy in dark mode, and always uses the light-on-navy logo variant; header controls pinned to light-on-navy colors (all WCAG AA).

## v2.9.3

Refreshed the COEO banner logo (light + dark variants); the dark variant is re-tuned to near-white with the brand blue "e" in CLIR signal blue for the CLIR dark card background.

## v2.9.2

Text legibility pass: ran a WCAG contrast audit across both themes and adjusted the tertiary text token (--faint) and amber so all body-size text now meets AA in light and dark mode, while staying on the CLIR palette. Ladder labels, timestamps, and arrow colors verified legible on the themed canvas.

## v2.9.1

Call-flow (ladder) diagram and header chrome brought fully onto the CLIR palette: the ladder canvas now uses CLIR card/surface tokens (light card in light mode, CLIR dark card in dark mode), lifelines/node boxes/time gutter follow theme tokens, arrows keep CLIR wire/amber/alert, and the theme toggle + version pill + brand eyebrow use CLIR tokens.

## v2.9.0

Recolored to match the COEO CLIR project palette exactly in both light and dark mode: CLIR tokens (--ink #1c355e, --wire #0e7a5f teal, --signal #416ba9 blue, --amber #9a6b0a, --alert #b13a2f and their dark html.dark equivalents) mapped onto every surface, chip, verdict pill, legend, table, and route line.

## v2.8.1

Platform tenancy rule refined: any *.voip.<region>.snetcom.net host without an -mtN segment is a Standalone Phone System (previously only explicit .pbx. hosts matched); -mtN remains Multi-tenant VPS.

## v2.8.0

Calls now mirror registrations: caller device dissection panel (Vendor/Model/Firmware/MAC/UA of the calling endpoint) and a network route line (Call from source IP:port → Via platform → Dialed number), including forwarded-to target and NAT private-address note.

## v2.7.2

Removed the duplicate device UA from the registration route line; it is already shown in the device dissection panel above. Route line is now purely the network path (source → registrar).

## v2.7.1

Removed the Export JSON button (Copy summary for Confluence remains).

## v2.7.0

Device dissection on registration verdicts: User-Agent parsed into Vendor / Model / Firmware / device-kind (hardphone/softphone/server), and MAC address harvested from the Mac header, UA string, or the +sip.instance UUID tail, with OUI→vendor lookup and a mismatch flag if the MAC OUI disagrees with the UA vendor. Shown as a device panel on the dashboard and as Vendor/Model/Firmware/MAC columns in the Confluence table.

## v2.6.0

Platform tenancy detection from registrar hostname: -mtN → Multi-tenant VPS (shared, faults can affect other tenants), .pbx. → Standalone Phone System (isolated to one customer); anything else flagged unrecognized. Shown as a badge + note on the registration route line and a Platform column in the Confluence table.

## v2.5.2

Registration source→registrar route line moved to just above the status-meaning / Technical detail section (was under the headline).

## v2.5.1

Registration verdict now shows the source it is registering from (IP:port, public/private, device UA) and the registrar it is targeting, as a route line on the dashboard; source + registrar added to the Confluence sign-in table.

## v2.5.0

Validated against 11 real Operations Monitor call traces. New REDIRECTED outcome for 3xx (forward target from Contact + Diversion reason, "working as configured" story, legend entries). Root-cause vs translated code: when hops answer differently (inner 404 → edge 503), the verdict names the root cause and notes what the caller actually saw. Trunk/session callee IDs (287102-8sgp...) prettified to the extension. 20-case synthetic suite passing.

## v2.4.0

Legend tab now includes the specific states: per-section reference of the reason qualifiers (401/403/404/408/423/480/5xx/NO RESPONSE/SIGNED OUT for registrations; 486/487/404/403/480/603/5xx/NO RESPONSE for calls), each with a one-line meaning, present-in-capture highlighting retained.

## v2.3.1

All states verified: canonical SIP reason table fills in missing/blank reason phrases; reason labels computed in the engine (single source for UI + JSON + Confluence); specific ELI5 for 603/600 Decline and 480 Temporarily Unavailable calls; 19-case synthetic state test suite (registrations: 401/403/404/408/423/480/500/503/no-response/sign-out; calls: busy/cancelled/404/480/503/declined/connected-open/completed/no-response) all passing.

## v2.3.0

Verdict status now carries the specific reason as a qualifier pill: NOT REGISTERED · 401 UNAUTHORIZED, · NO RESPONSE, · SIGNED OUT, CALL FAILED · 486 BUSY HERE, etc. Reason also included in Confluence status line.

## v2.2.4

Legend tab split into two sections: Registration statuses and Call statuses.

## v2.2.3

Each verdict banner now includes a one-line "status meaning" strip above the Technical detail section (Legend tab retained; both share one definition source).

## v2.2.2

Status legend moved to its own Legend tab; the main dashboard shows only the status verdicts.

## v2.2.1

"What these statuses mean" legend on the dashboard: every REGISTERED/NOT REGISTERED/CALL status pill defined in one line; statuses present in the capture highlighted, others dimmed.

## v2.2.0

Call analysis: INVITE dialog tracking, B2BUA leg linking (one logical call across internal legs), answer/ring/PDD timing, talk time, hang-up attribution via tags, SDP codec negotiation, hold/re-INVITE detection, outcome classification (completed/cancelled/busy/failed/no-response), plain-English call verdict hero with recommendations, Calls tab + card, call findings (very short call, instant answer), Confluence export section.

## v2.1.0

Recommended actions on incomplete registrations: ownership-tagged fix steps (Device / Customer network / Server side / Escalate) in the verdict hero and Confluence export.

## v2.0.1

Footer now themes with light/dark mode: paper bar with hairline in light, ink surface in dark, teal accent dot; overscroll background follows theme.

## v2.0.0

ELI5 dashboard: plain-English verdict hero (headline, story, "what to check first" checklist, collapsible technical detail), "What is SIP registration?" intro. Engine: client-leg-only registration counting (proxy relays no longer duplicate), retry-storm grouping (attempts/sessions), 401-challenge-answered detection, placeholder-identity (%NULL%) detection, 504 upstream-timeout diagnosis, aggregated warnings.

## v1.2.0

COEO logo in header (light + dark variants, embedded base64). Dark/light mode: theme toggle, prefers-color-scheme default, saved preference, full dark palette for cards, chips, tables, verdicts.

## v1.1.0

Registration verdict banner on dashboard: per-AOR REGISTERED / NOT REGISTERED status with likely-cause diagnosis (auth, ALG/firewall, provisioning, server-side), prior-event trail, Confluence summary section.

## v1.0.0

Initial release.
