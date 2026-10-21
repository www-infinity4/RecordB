# RecordB — Bubbles screen and sound recorder

RecordB is the source home for Bubbles' compact **in-chat Android recorder card**. Its working first-party browser/WebView integration is mirrored at `www-infinity4/QuantaPhi/recordb-card.mjs`. It uses the Bubbles APK's *existing* Android foreground screen/video and shared-microphone recording services rather than adding a competing microphone or third-party screen recorder.

## Chat card
- **Record** requests Android screen recording plus microphone audio. The user must approve MediaProjection on the phone. Opening the card never starts capture.
- **Pause / Resume** send the native Android capture commands. Only the relevant control appears for the current native state.
- **Stop & save** calls the existing Android recorder; status comes from Android's actual save/processing result.
- The original recording stays locally in the phone's **Movies/Bubbles** collection. No clips, screens, audio, tokens, or credentials are sent to this GitHub repository. This repository holds app source and tests only.

## Wiring
`src/recordb-card.mjs` exports `mountRecordBCard` and `recordBState`. The QuantaPhi Bubbles client loads a same-origin copy in its Android WebView. The card sends text commands via `QuantaBubbles.drainNativeCaptureRequest()`, which the Android `FloatingOracleWindow` parses through the existing `VoiceCaptureCommands` and forwards to `BubblesService` / `ScreenCaptureService`. Android's system approval is never bypassed.

This is not a stand-alone APK or a claim that a physical phone recording has been tested. Native source and web tests must pass, then phone start, pause, resume, stop, playback and screen-off behavior must be checked on-device.

Run tests: `npm test` (Node.js 22+).
