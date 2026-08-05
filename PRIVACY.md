# COTS Comic Studio Privacy Notice

Effective date: August 4, 2026  
Privacy notice version: 2026-08-04.2

Created by **COTS Beasto**.  
Copyright © 2026 COTS Beasto. All rights reserved.

Contact: `cotsbeasto@gmail.com`

This notice describes COTS Comic Studio as implemented in the current source. It distinguishes local application behavior, optional provider use, authentication, and update checks.

## Local-First Behavior

COTS Comic Studio is a local-first Windows desktop application. COTS Beasto does not operate a server that receives your project files, prompts, references, generated images, analytics, telemetry, crash reports, or usage monitoring from the application.

Projects are stored where you create or save the `.cotscm` project file. A project folder can contain:

- `project.db`, a local SQLite database for project data.
- Imported references, generated candidates, externally imported images, exports, backups, cache files, and logs.
- Character, location, prop/equipment, continuity, prompt, page, panel, lettering, and generation records.

The app also stores normal application settings in Electron's user-data folder. On Windows this is usually under the current user's AppData area for `cots-comic-studio`. The app may store window/layout preferences, recent-project information, provider settings, update state, accepted EULA version, and logs there.

Uninstalling the app may leave user-created projects, exports, backups, cached data, recent-project markers, and Electron user-data settings behind. Delete those folders manually if you want to remove them.

## Credentials and Authentication

Saved API keys are stored by the Electron main process in the app user-data folder. When Electron `safeStorage` encryption is available, keys are encrypted before being written. The renderer receives masked previews only, not raw provider keys. Environment-provided keys such as `OPENAI_API_KEY`, `STABILITY_API_KEY`, `REPLICATE_API_TOKEN`, `GEMINI_API_KEY`, and `PIXELLAB_API_TOKEN` are read by the main process and are not written into project files by COTS Comic Studio.

Codex OAuth/ChatGPT sign-in is handled by the installed Codex App Server. COTS Comic Studio stores the selected Codex executable path when needed, but it does not store ChatGPT account tokens.

Exported provider configurations omit API keys.

## When Data Leaves the Computer

Project data stays local unless you deliberately invoke an online provider or Codex ImageGen. When you do, the request can include prompts, refinement instructions, project style text, panel descriptions, character details, location details, prop/equipment details, continuity notes, reference-image metadata, and selected reference images.

COTS Comic Studio records local generation diagnostics such as provider, auth mode, request type, request fingerprint, status, timing, reference count, and sanitized errors. These logs are stored locally and are intended for troubleshooting. They should not include raw provider credentials or image contents.

## Confirmed Provider Integrations

Provider terms, prices, limits, retention, moderation, model training rules, and privacy practices are controlled by the provider and may change. Review the current provider policies before sending content.

| Provider mode | What COTS sends when used | Authentication | Current policy links |
| --- | --- | --- | --- |
| Codex ImageGen | Prompt context and selected reference images through the installed Codex App Server and `$imagegen` flow. | ChatGPT/Codex sign-in handled outside COTS. | [OpenAI privacy](https://openai.com/policies/privacy-policy/), [OpenAI API data controls](https://platform.openai.com/docs/models/default-usage-policies-by-endpoint) |
| OpenAI Direct API | Image prompts, refinement instructions, and reference images for `gpt-image-2` generation/edit requests. | User-provided OpenAI API key or environment key. | [OpenAI privacy](https://openai.com/policies/privacy-policy/), [OpenAI API data controls](https://platform.openai.com/docs/models/default-usage-policies-by-endpoint) |
| Gemini | Prompt text and supported image references through the Google Gemini API. | User-provided Gemini/Google API key. | [Google privacy policy](https://policies.google.com/privacy), [Gemini API terms](https://ai.google.dev/gemini-api/terms) |
| PixelLab | Prompt text and supported references through the PixelLab API. | User-provided PixelLab API token. | [PixelLab terms and privacy links](https://www.pixellab.ai/termsofservice) |
| Stability AI | Prompt text and supported generation options through the Stability API. | User-provided Stability API key. | [Stability AI privacy](https://stability.ai/privacy-policy), [Stability AI acceptable use](https://stability.ai/use-policy) |
| Replicate | Prompt text and supported generation settings through the Replicate API. | User-provided Replicate API token. | [Replicate privacy](https://replicate.com/privacy), [Replicate terms](https://replicate.com/terms) |
| Custom HTTP/OpenAI-compatible/local provider | Request body, headers, prompts, and references according to the user-configured template. | User-configured endpoint and credentials. | Review the endpoint operator's policies. |
| Manual providers and Offline Placeholder | No provider request is sent by COTS Comic Studio. | None. | Not applicable. |

## Update Checks

Installed builds can perform anonymous GitHub release update checks through the public COTS Comic Studio releases repository. Portable builds do not self-update; they open the public release page for manual downloads. GitHub may receive normal request metadata such as IP address and user-agent according to GitHub's own policies.

## Offline and Manual Use

The core workflow can be used without API access: create projects, build pages and panels, maintain Bible references and continuity, write prompts, copy prompts, import externally created images, add lettering, and export pages. No provider data is transmitted for those local/manual actions.

## Deleting Local Data

To remove local data, delete your project folders and `.cotscm` files wherever you saved them. You may also delete the Electron user-data folder for `cots-comic-studio` to remove local settings, provider configuration, accepted EULA version, recent-project state, and local logs. Back up anything you want to keep before deleting.
