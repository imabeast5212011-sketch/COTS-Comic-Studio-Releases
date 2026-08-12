# COTS Character HUD

COTS Character HUD is a Foundry VTT v14 module for D&D 5e worlds. It adds a compact live player-character HUD, a multi-companion tray, and a two-speaker SNES-style character presentation overlay for table talk in Discord or ordinary non-roll Foundry chat.

## Development Installation

Copy or link the `cots-character-hud` folder into your Foundry user data folder under `Data/modules/cots-character-hud`, restart Foundry, and enable **COTS Character HUD** in your D&D 5e world.

The module manifest intentionally does not include public download, manifest, repository, or release URLs.

## Player Setup

Open **Configure Settings > Module Settings > COTS Character HUD > Character HUD Configuration** or use the HUD gear button.

Choose one main Actor. Non-GM users only see Actors they own with OWNER permission. GMs can choose from world Actors. Add any number of companions from owned Actors, or from owned tokens on the current scene when available. Removing a companion only removes it from the tray; it does not delete the Actor or token.

Drag the HUD header to save its client-side screen position. Use the header chevron to collapse the HUD and the companion chevron to collapse the tray.

## Speaker Controls

The module registers Foundry keybindings without default keys, so you can assign non-conflicting controls in **Configure Controls**:

- Hold to Present Main Character
- Toggle Main Character Presentation
- Hold to Present GM Speaker

Holding a presentation key broadcasts a native Foundry module socket message. Other connected clients show the configured Actor portrait and name, with no written dialogue box. Releasing the key stops the speaking state and the portrait lingers briefly before fading.

The overlay supports two simultaneous or recent speakers: first on the left, second on the right. A third speaker replaces the least-recent inactive speaker, or the least-recently-started active speaker if both slots are active.

Manual presentation can be disabled per client with **Manual Speaker Presentation**.

## Auto Voice Presentation

Enable **Auto Voice Presentation** on a client to ask the browser for microphone permission and present that user's main Actor while local microphone loudness is above the configured threshold. The module does not send, store, transcribe, or analyze speech content; it only broadcasts speaker start/stop state after local loudness detection.

Use **Voice Activity Threshold** and **Voice Release Delay** to tune false positives. Disable **Auto Voice Presentation** to stop the microphone stream.

## Text Chat

When **Present Non-Roll Chat** is enabled, ordinary chat messages briefly present the chat speaker's Actor. Roll messages are ignored. The message text is not displayed in the overlay.

The module does not integrate with Discord directly. Auto voice presentation listens only to the browser microphone source the user permits.

## Overlay Layout and Appearance

Each client can move and resize the speaker overlay with the overlay move and resize controls. Position and width are stored locally.

Client settings allow customization of the overlay background color, gradient start/end colors, border color, panel opacity, and portrait opacity. Actor accent colors still override the speaker frame color for specific Actors.

## GM Speaker Picker

GMs get a small overlay dock with the selected NPC speaker, preview, and stop-all controls. Open the picker from the dock or from module settings. The picker builds an Actor index once and filters that local index as you type, so it does not search every Actor document on each keypress.

The selected GM speaker is stored in the GM user's flags and remains selected until changed or cleared.

## Cinematic Actor Settings

Open an Actor sheet and use the **COTS cinematic settings** header control, or open the same form from the HUD configuration. Actor flags store:

- Optional cinematic display name
- Optional cinematic portrait path
- Optional accent color
- Portrait side preference
- Optional portrait flip

If no cinematic portrait is configured, the module uses the Actor portrait. Accent values are sanitized to hex colors before rendering.

## Settings

World settings control whether the player HUD and speaker overlay are enabled, max speakers, default linger duration, player accent permissions, GM-only cinematic portrait restrictions, client linger overrides, and non-roll chat presentation.

Client settings control local HUD visibility, HUD position and scale, companion tray default collapse, speaker overlay visibility and scale, linger override, and reduced animation.

User-specific Actor selections and tray state are stored in User flags. Actor cinematic settings are stored in Actor flags.

## Current Limitations

- Manual companion selection is the supported MVP workflow.
- Temporary summoned token detection is a quick-add helper only; it is not a full summoning automation system.
- Native Foundry module sockets relay payloads between clients. The module validates the claimed user, Actor UUID, GM mode, and ownership relationship before display, but native relayed module sockets do not provide cryptographic sender identity to client code.
- Browser microphone permission behavior varies by host/browser security policy. Auto voice presentation requires a secure context or an Electron client that permits `getUserMedia`.
- Live multiplayer behavior still needs validation in a running Foundry v14 D&D 5e world.

## Troubleshooting

- If the HUD is empty, confirm your user owns the selected Actor and that the module is enabled in a D&D 5e world.
- If token selection or panning does nothing, make sure the Actor has an active token on the currently viewed scene.
- If the overlay is hidden, re-enable **Speaker Overlay Enabled** in client module settings.
- If NPC portraits do not appear for players, configure a cinematic portrait path on the Actor so the GM socket payload includes a display-safe portrait.

## Tested Versions

Static checks were performed outside Foundry. The local Foundry MCP bridge was not connected to a running world during development, so live Foundry v14 and D&D 5e runtime testing remains required.
