---
layout: default
title: MarkEd Privacy Policy
permalink: /privacy/
---

# MarkEd Privacy Policy

This is the official privacy policy for the MarkEd Chrome extension.

**Last updated:** June 20, 2026

## Summary

MarkEd is a Chrome extension that lets you highlight any webpage, add comments, and ask an AI assistant ("Mark") questions about what you've highlighted.

Your highlights, comments, and AI conversation history are stored **only in your browser** (IndexedDB). They never leave your device unless you explicitly export them.

To answer your questions, MarkEd forwards them through a Cloudflare Worker we operate, which calls the Anthropic Claude API. You sign in with Google so we can rate-limit usage per account.

## Data stored locally (always)

The following live in your browser's IndexedDB and never sync to a server:

- Highlights (text, page URL, prefix/suffix context for re-anchoring)
- Comments and AI conversation threads
- Page metadata (URL, title, extracted article text used as AI context)
- Settings (chosen AI model, Obsidian vault path if you set one)

Uninstalling the extension removes all locally stored data.

## Data we process when you ask Mark a question

When you sign in with Google and ask the AI assistant a question, the request goes from your browser through our Cloudflare Worker to Anthropic.

**At sign-in, we store on our infrastructure:**
- Google account `sub` (a stable opaque user identifier issued by Google)
- Email address
- Display name
- Profile picture URL

These are used to issue you a session token (JWT) that authenticates subsequent requests. We do not use them for marketing, advertising, or any purpose other than authenticating you to the proxy.

**At each AI request, our worker sees in transit:**
- Your highlighted text
- Your question
- The article text we extracted from the page (used as context)
- Your prior conversation history for the same highlight

This content is forwarded to Anthropic to generate the response and is **not persisted on our infrastructure**.

**At each AI request, we persist on our infrastructure:**
- Metadata only: your user identifier (`sub`), the endpoint hit, the model used, and input/output token counts. Used for per-user rate limiting and (eventually) billing.

We enforce a per-user rate limit (currently 20 requests/hour) and hard size limits on every request field to prevent abuse.

## Product analytics (activation milestones)

We record two account-level milestone events to understand the activation funnel: how many people who install MarkEd go on to sign in and use the core highlight feature.

- `signed_in` — recorded server-side the first time you complete Google sign-in.
- `first_highlight` — sent by the extension the first time you create a highlight.

Each milestone row stores only your Google account `sub`, the event name, an optional extension version string (`client_version`), and a timestamp. These records are pseudonymous because they are tied to a stable account identifier. They do not contain page content, URLs, highlight text, question text, or AI responses.

These milestone events are stored in Cloudflare D1 alongside the usage metadata described above. We use them only to improve onboarding.

## Data we do not collect

- We do not track which pages you visit.
- We do not log the content of your highlights, questions, or AI responses on our infrastructure.
- We do not run page-level or behavioral analytics on your browsing; we only record the two account-level activation milestones described above.
- We do not sell, share, or expose your data to any third party other than the sub-processors listed below.

## Sub-processors

- **Anthropic** — receives your highlighted text, question, article text, and conversation history to generate the AI response. Subject to [Anthropic's privacy policy](https://www.anthropic.com/legal/privacy).
- **Cloudflare** — hosts the proxy Worker, D1 (rate-limit counters, usage metadata, and activation milestone events), and KV (token introspection cache). Subject to [Cloudflare's privacy policy](https://www.cloudflare.com/privacypolicy/).
- **Google** — issues the OAuth token used to sign you in. Subject to [Google's privacy policy](https://policies.google.com/privacy).

## Permissions

- **activeTab** — access the page you're currently viewing only after you explicitly activate MarkEd on that tab, using the popup "Enable MarkEd on this page" button, the "Highlight with MarkEd" context-menu item, or the Cmd/Ctrl+Shift+M keyboard shortcut. MarkEd does not use broad page host permissions or an always-on content script.
- **scripting** — inject the MarkEd content script into that one active tab when you activate the extension.
- **contextMenus** — provide the right-click "Highlight with MarkEd" action for selected text.
- **storage** — save highlights, comments, and settings locally.
- **downloads** — produce markdown / Obsidian export files when you trigger an export.
- **identity** — Google sign-in flow.
- **host_permissions** — talk to our Cloudflare Worker, which calls Anthropic for AI requests, and Google sign-in endpoints.

MarkEd cannot see pages you do not explicitly activate it on. As a result, saved highlights reappear only after you re-activate MarkEd on that page.

## Data deletion

- **Local data:** uninstalling the extension removes all locally stored highlights, comments, conversations, and settings from your browser.
- **Proxy-side data:** to delete your account-level usage and activation milestone records on our server, open a [GitHub issue](https://github.com/nayanbandaru/marked-ext/issues) with your account email — we will purge your `sub`-keyed records. A self-serve delete endpoint is planned.

## Children

MarkEd is not directed at children under 13 and we do not knowingly collect data from them.

## Changes

If this policy changes, the update will be published in this repository before any extension update that depends on it is submitted to the Chrome Web Store.

## Contact

Questions or concerns: [GitHub Issues](https://github.com/nayanbandaru/marked-ext/issues)
