# COTS Character HUD

COTS Character HUD is a Foundry VTT v14 module for D&D 5e worlds. It adds a compact live player-character HUD, a multi-companion tray, and an SNES-style character presentation overlay for table talk in Discord or ordinary non-roll Foundry chat.

## Installation

Install from this manifest URL in **Add-on Modules > Install Module**:

```text
https://raw.githubusercontent.com/imabeast5212011-sketch/COTS-Comic-Studio-Releases/main/foundry/cots-character-hud/module.json
```

For local development, copy or link the `cots-character-hud` folder into your Foundry user data folder under `Data/modules/cots-character-hud`, restart Foundry, and enable **COTS Character HUD** in your D&D 5e world.

Recent releases use versioned JS, CSS, template, and language asset paths to force browsers, especially Chrome, to load the current module files after updating.

## Player Setup

Open **Configure Settings > Module Settings > COTS Character HUD > Character HUD Configuration** or use the HUD gear button.

Choose one main Actor. Non-GM users only see Actors they own with OWNER permission. GMs can choose from world Actors. Add any number of companions from owned Actors, or from controlled tokens on the current scene when available. Scene-token companions are stored by token UUID, so two controlled minions that share the same base Actor can both appear in the tray. Removing a companion only removes it from the tray; it does not delete the Actor or token.

Drag the HUD header to save its client-side screen position. Drag the lower-right resize grip to resize the HUD window width and height; the module keeps portraits, bars, and the companion tray fitted inside that saved size. Use the header chevron to collapse the HUD and the companion chevron to collapse the tray. The HUD header includes local opt-out buttons for manual talk presentation and your own chat-triggered presentation.

## Speaker Controls

The module registers Foundry keybindings without default keys, so you can assign non-conflicting controls in **Configure Controls**:

- Hold to Present Main Character
- Toggle Main Character Presentation
- Hold to Present GM Speaker

Holding a presentation key broadcasts a native Foundry module socket message. Other connected clients show the configured Actor portrait and name, with no written dialogue box. Releasing the key stops the speaking state and the portrait lingers briefly before fading.

The overlay keeps a single remembered participant collection and derives the visible layout from it. Up to two speakers appear in large focus positions. Additional recent participants appear in a compact carousel above the frame. If more than two people are actively speaking, the two most recently activated speakers stay in focus and displaced active speakers remain marked as speaking in the carousel.

Manual presentation can be disabled per client with **Manual Speaker Presentation** or the HUD talk opt-out button.

## Auto Voice Presentation

Enable **Auto Voice Presentation** on a client to ask the browser for microphone permission and present that user's main Actor while local microphone loudness is above the configured threshold. For GMs, auto voice uses the selected GM speaker actor first, so the bottom scene speaker selector can drive NPC voice presentation; it falls back to the GM main HUD actor only when no GM speaker is selected. The module does not send, store, transcribe, or analyze speech content; it only broadcasts speaker start/stop state after local loudness detection.

Use **Voice Activity Threshold** and **Voice Release Delay** to tune false positives. Disable **Auto Voice Presentation** to stop the microphone stream.

Open **Voice Input Setup** in module settings to ask the browser for microphone access again, refresh the available input list, test a selected microphone, and save the chosen input for this browser. If a saved microphone is unavailable later, the module falls back to the browser default.

## Text Chat

When **Present Non-Roll Chat** is enabled, ordinary text-box chat messages briefly present the chat speaker's Actor and message text after the message is sent. Roll messages, item cards, feature cards, and other clicked sheet output are ignored. Text is escaped before rendering and trimmed to a short dialogue-sized excerpt. Each user can disable their own chat-triggered presentation from the HUD without disabling chat presentation for everyone else.

If the chat message has no explicit Actor speaker, the module falls back to the sender's assigned Foundry character, then the sender's selected COTS HUD main Actor. For GM-authored plain chat, the GM speaker picker is preferred over the chat/HUD actor selector and is broadcast to connected clients; if no GM speaker is selected, it falls back normally.

The module does not integrate with Discord directly. Auto voice presentation listens only to the browser microphone source the user permits.

## Overlay Layout and Appearance

Each client can move and resize the speaker overlay with the overlay move and resize controls. Position and width are stored locally. If a non-GM hides the local speaker overlay, a small recovery eye button remains so they can show it again. GMs can use the eye button on the GM dock.

Open **HUD and Chat Bubble Appearance** in module settings to choose Character HUD and chat bubble background colors, gradient start/end colors, border colors, text color for the HUD, panel opacity, and portrait opacity with native color-wheel controls. The window shows separate live examples for the HUD and the chat bubble/speaker overlay, and it temporarily previews changes on the live HUD and speaker overlay before you save. These theme settings are client-side, so each user can make their own screen look different.

Enable **Show Token Speech Bubbles** to display a small animated three-dot bubble above an on-screen token while that Actor is actively speaking.

Actor accent colors are actor-specific and travel with the speaker presentation. If Rose is configured pink, connected clients see Rose with that accent when Rose speaks; your Actor can use a different accent when you speak.

## GM Speaker Picker

GMs get a draggable black-and-red overlay dock with the selected NPC speaker, local overlay show/hide, preview, persistent display off/on, pin-current, GM-only focus, clear-inactive, and reset-position controls. The GM dock is a fixed viewport dock, so chat bubble presentation does not move it, and it remains visible as a recovery/control shell when the GM locally hides the speaker overlay. Open the picker from the dock or from module settings. The picker builds an Actor index once and filters that local index as you type, including quick All, PC, and NPC filters, so it does not search every Actor document on each keypress.

The selected GM speaker is stored in a separate GM-only client setting and remains selected until changed or cleared. It is separate from the GM user's personal HUD actor selection. The power button is a persistent GM-controlled display toggle: turning it off clears active, lingering, and pinned participants and ignores new presentations until a GM turns the speaker display back on. **Turn off except GM** clears every current speaker except the selected or currently active GM speaker.

The picker also shows current participants and their state. GMs can pin the selected NPC, pin or unpin current participants, and clear inactive unpinned entries. Pinned participants remain in the conversation display but do not displace active speakers from focus.

## Cinematic Actor Settings

Open an Actor sheet and use the **COTS cinematic settings** header control, or open the same form from the HUD configuration. Actor flags store:

- Optional cinematic display name
- Optional cinematic portrait path
- Optional focus portrait override
- Optional carousel portrait override
- Optional short carousel name
- Portrait focal X/Y percentages
- Optional accent color
- Portrait side preference
- Optional portrait flip

If no cinematic portrait is configured, the module uses the Actor portrait. Focal position is applied with safe CSS object-position values in the HUD, focus portrait, and carousel portrait. Focus portraits auto-flip when layout places them on the opposite side so they continue facing inward. Accent values are sanitized to hex colors before rendering.

## Settings

World settings control whether the player HUD and speaker overlay are enabled, max focus speakers, maximum remembered conversation participants, default linger duration, player accent permissions, optional player pinning, pinned scene-change survival, GM-only cinematic portrait restrictions, client linger overrides, non-roll chat presentation, and the GM speaker-display off state.

Client settings control local HUD visibility, HUD position, width, height, and scale, companion tray default collapse, speaker overlay visibility and scale, carousel visibility and scale, GM dock position, linger override, HUD/chat-bubble appearance, voice input device, GM speaker selection, and reduced animation.

User-specific Actor selections, tray state, and chat presentation opt-out are stored in User flags. Actor cinematic settings are stored in Actor flags.

## Current Limitations

- Manual companion selection is the supported MVP workflow.
- Temporary summoned token detection is a quick-add helper only; it is not a full summoning automation system.
- Native Foundry module sockets relay payloads between clients. The module validates the claimed user, Actor UUID, GM mode, and ownership relationship before display, but native relayed module sockets do not provide cryptographic sender identity to client code.
- Browser microphone permission behavior varies by host/browser security policy. Auto voice presentation requires a secure context or an Electron client that permits `getUserMedia`. If a browser-level block is saved for the site, the browser's site permissions may still need to be changed outside Foundry.
- Live multiplayer behavior still needs validation in a running Foundry v14 D&D 5e world.

## Troubleshooting

- If the HUD is empty, confirm your user owns the selected Actor and that the module is enabled in a D&D 5e world.
- If token selection or panning does nothing, make sure the Actor has an active token on the currently viewed scene.
- If the overlay is hidden, re-enable **Speaker Overlay Enabled** in client module settings.
- If the GM speaker dock is off-screen or hidden, open **GM Speaker Picker** from module settings and use **Reset GM dock position**. GMs can also use the dock's eye button to show the local overlay again.
- If auto voice uses the wrong microphone, open **Voice Input Setup**, ask for microphone access, refresh the list, choose the input, test it, and save.
- If NPC portraits do not appear for players, configure a cinematic portrait path on the Actor so the GM socket payload includes a display-safe portrait.

## Tested Versions

Static checks were performed outside Foundry. The local Foundry MCP bridge was not connected to a running world during development, so live Foundry v14 and D&D 5e runtime testing remains required.
