# Accessibility

> **Release scope:** v0.1.0 is a portable Windows x64 release. The Android requirements below describe the project's broader design targets; no Android build is included in the v0.1.0 GitHub release. The manual Windows assistive-technology test matrix has not been completed for this preview.

Accessibility is an acceptance criterion, not a theme.

## Desktop requirements

- Native Qt controls with labels, accessible names, descriptions, roles, states, and logical tab order.
- All editing is available as timestamped tables/forms; dragging a timeline is optional.
- Menus and frequent actions have discoverable keyboard shortcuts.
- Progress changes are announced at phase changes and meaningful percentage boundaries, not every frame.
- Errors identify the field, cause, and recovery action and receive focus.
- High contrast uses system palette; status never relies on color alone.
- `Tab` and `Shift+Tab` leave every multiline editor instead of inserting indentation and trapping keyboard focus; literal tab characters are not required by any editor workflow.

## Android requirements

- Compose semantics include labels, roles, state descriptions, headings, and custom actions where appropriate.
- Interactive targets are at least 48 dp and remain usable with large font/display scaling.
- Traversal follows reading/task order. Dynamic status uses polite live regions.
- SAF avoids broad storage permissions. WorkManager notifications identify the video and offer a labeled cancel action.
- Icons with actions always have text/semantic labels; decorative images have no semantics.
- Player controls work with TalkBack, Switch Access, Voice Access, keyboard/D-pad, and orientation/lifecycle changes.

## Caption and reading presentation

Users can configure text size, line/letter spacing, contrast, background opacity, position, speaker labels, reading speed, and simplified language. Explicit positioning never burns captions permanently into the only export; users retain a selectable caption track and can override placement.

Current implementation exports styled WebVTT with user-selected top/bottom/automatic placement, font scale, colours, opacity, letter spacing, line height, speaker labels, and a machine-readable reading-speed report. SRT remains deliberately style-neutral. Desktop and Android players provide local caption size/high-contrast controls and use a non-obscuring text panel or selectable soft-caption track. Automatic important-region collision detection is not yet validated; users retain explicit position control rather than the application claiming a safe visual placement.

The Windows text editors and player can read transcript text through the installed Windows speech service. This local convenience reader is separate from the selected AI narration provider and does not replace reviewed audio description.

## Manual release matrix

The following must be completed on release builds and recorded with version/device/user findings:

- NVDA: browse/focus mode, every section, dialogs, tables, editor, player, errors, progress.
- Windows keyboard-only: no traps; visible focus; all actions and seek controls.
- TalkBack: linear/explore-by-touch, player, upload, provider form, progress, sharing.
- Switch Access and Voice Access: core workflow and large controls.
- 200% Windows scaling; Android largest font/display sizes; high contrast/dark mode.
- At least one usability session each with blind, Deaf/hard-of-hearing, cognitive/learning, and motor-disabled participants where feasible and consented.

Automated checks supplement but cannot replace this matrix. Until it is completed, documentation must say “designed for accessibility,” not “fully accessible” or compliant with a particular standard.

Host automation currently verifies named native sections/controls, multiline-editor Tab focus traversal, scrollable large-text layouts, explicit Qt accessibility announcements, Compose semantics source/instrumentation compilation, and minimum control sizing patterns. No manual row above has been marked passed.
