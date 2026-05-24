---
layout: default
title: MarkEd Privacy Policy
permalink: /privacy/
---

# MarkEd Privacy Policy

**Last updated:** May 24, 2026

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

## Data we do not collect

- We do not run analytics or telemetry on your browsing.
- We do not track which pages you visit.
- We do not log the content of your highlights, questions, or AI responses on our infrastructure.
- We do not sell, share, or expose your data to any third party other than the sub-processors listed below.

## Sub-processors

- **Anthropic** — receives your highlighted text, question, article text, and conversation history to generate the AI response. Subject to [Anthropic's privacy policy](https://www.anthropic.com/legal/privacy).
- **Cloudflare** — hosts the proxy Worker, D1 (rate-limit counters and usage metadata), and KV (token introspection cache). Subject to [Cloudflare's privacy policy](https://www.cloudflare.com/privacypolicy/).
- **Google** — issues the OAuth token used to sign you in. Subject to [Google's privacy policy](https://policies.google.com/privacy).

## Permissions

- **activeTab** — inject the highlighting script on the page you're currently viewing.
- **storage** — save highlights, comments, and settings locally.
- **downloads** — produce markdown / Obsidian export files when you trigger an export.
- **identity** — Google sign-in flow.
- **host_permissions** — talk to our Cloudflare Worker, the Anthropic API (for the AI request), and Google sign-in endpoints.

## Data deletion

- **Local data:** uninstalling the extension removes all locally stored highlights, comments, conversations, and settings from your browser.
- **Proxy-side data:** to delete your account-level usage records on our server, open a [GitHub issue](https://github.com/nayanbandaru/agentmark/issues) with your account email — we will purge your `sub`-keyed records. A self-serve delete endpoint is planned.

## Children

MarkEd is not directed at children under 13 and we do not knowingly collect data from them.

## Changes

If this policy changes, the update will be published in this repository before any extension update that depends on it is submitted to the Chrome Web Store.

## Contact

Questions or concerns: [GitHub Issues](https://github.com/nayanbandaru/agentmark/issues)
