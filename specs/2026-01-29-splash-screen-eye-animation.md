# Feature: Oculog Splash Screen with Eye Animation & App Icon

## Summary
Add an animated splash screen with an eye/ocular-themed loader that displays while the app loads initial data from the API, plus a matching app icon.

## Trigger
- App launch (automatic)
- Splash shows until all initial data (auth, config, content) is loaded from API

## Expected Result
- Stunning eye-themed animation plays during loading
- Smooth transition to main content when loading completes
- Error state with retry button if API fails
- Professional app icon matching the eye theme

## Edge Cases
- API timeout → Show error with retry button
- API returns error → Show error with retry button
- Network unavailable → Show error with retry button
- Fast API response → Still show animation briefly for polish (minimum display time)

## Technical Design

### Scope
**Swift Client only** - No API changes needed

### New Files to Create

1. **`Sources/SplashView.swift`** - The splash screen view
   - Full-window animated eye graphic
   - Loading progress indicator
   - "Oculog" branding text
   - Animated iris/pupil with pulsing, scanning effects

2. **`Sources/EyeAnimationView.swift`** - Custom eye animation
   - Concentric circles (iris rings) with rotation animations
   - Pupil that pulses/dilates
   - "Scanning" lines radiating outward
   - Glow effects using shadows and gradients
   - Pure SwiftUI - no external dependencies

3. **`Sources/AppState.swift`** - Centralized app state management
   - Loading states enum: `.loading`, `.loaded`, `.error(String)`
   - Async data loading orchestration
   - Retry logic

4. **`Resources/Assets.xcassets/`** - App icon asset catalog
   - `AppIcon.appiconset/` with all required sizes
   - Eye-themed icon design (generated programmatically as PNG)

### Modified Files

1. **`Sources/OculogApp.swift`**
   - Add `@StateObject` for AppState
   - Conditional view: SplashView vs ContentView based on loading state

2. **`Sources/ContentView.swift`**
   - Remove data loading logic (moved to AppState)
   - Accept items as parameter or from environment
   - Keep the existing table display

3. **`Package.swift`**
   - Add Resources folder configuration for assets

### Animation Design (Eye Theme)

```
┌─────────────────────────────────────┐
│                                     │
│         ╭───────────────╮           │
│       ╱   ╭─────────╮    ╲          │
│      │   ╱ ╭───────╮ ╲   │          │
│      │  │  │   ●   │  │  │  ← Pupil (pulses)
│      │   ╲ ╰───────╯ ╱   │          │
│       ╲   ╰─────────╯    ╱  ← Iris rings (rotate)
│         ╰───────────────╯           │
│                                     │
│           ● ● ● ● ●        ← Loading dots
│            OCULOG                   │
│                                     │
└─────────────────────────────────────┘
```

**Animation Effects:**
1. **Outer iris rings** - Rotate in opposite directions (slow, mesmerizing)
2. **Inner iris pattern** - Radial gradient with shimmer effect
3. **Pupil** - Pulses (dilates/contracts) rhythmically
4. **Scanning lines** - Radiate outward periodically
5. **Glow** - Subtle outer glow that pulses
6. **Loading dots** - Animated beneath the eye

### App Icon Design

Eye-themed icon with:
- Dark background (#1a1a2e or similar)
- Stylized eye with neon/cyber glow
- Gradient iris (purple → cyan)
- Clean, modern look suitable for macOS dock

Will be generated as PNG files at required sizes:
- 16x16, 32x32, 64x64, 128x128, 256x256, 512x512, 1024x1024

## Implementation Plan

1. [ ] Create `AppState.swift` with loading state management
2. [ ] Create `EyeAnimationView.swift` with the animated eye graphic
3. [ ] Create `SplashView.swift` combining animation + loading status
4. [ ] Update `OculogApp.swift` to show splash during loading
5. [ ] Update `ContentView.swift` to work with new state management
6. [ ] Create `Assets.xcassets` with app icon
7. [ ] Update `Package.swift` to include resources
8. [ ] Test full flow: launch → splash → load → content / error → retry

## Verification

1. Run `swift build` - should compile without errors
2. Run `swift run OculogClient` with API running - should show splash then content
3. Run `swift run OculogClient` without API - should show splash then error with retry
4. Click retry button - should attempt reload
5. Verify app icon appears in dock and app switcher
