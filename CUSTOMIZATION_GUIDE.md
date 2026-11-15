# LocalAI Mobile — Complete App Customization & Reskinning Guide

This guide provides step-by-step instructions for reskinning, rebranding, customizing, and building **LocalAI Mobile** for production release on Google Play and Apple App Store.

---

## Table of Contents
1. [Changing App Name & App ID (Package Name)](#1-changing-app-name--app-id)
2. [Replacing App Icon & Splash Screen](#2-replacing-app-icon--splash-screen)
3. [Customizing Colors & Theme Palette](#3-customizing-colors--theme-palette)
4. [Customizing Interface Strings & Branding](#4-customizing-interface-strings--branding)
5. [Customizing AI Personas & Prompts](#5-customizing-ai-personas--prompts)
6. [Adding or Changing GGUF Models](#6-adding-or-changing-gguf-models)
7. [Building for Production (Google Play AAB & App Store IPA)](#7-building-for-production)
8. [Troubleshooting & Verification](#8-troubleshooting--verification)

---

## 1. Changing App Name & App ID

All core identity parameters are configured in [`app.json`](file:///app.json).

Open `app.json` and locate the `expo` object:

```json
{
  "expo": {
    "name": "YourAppName",
    "slug": "your-app-slug",
    "scheme": "yourapp",
    "version": "1.0.0",
    "ios": {
      "bundleIdentifier": "com.yourcompany.yourapp"
    },
    "android": {
      "package": "com.yourcompany.yourapp"
    }
  }
}
```

### Steps to Customize:
1. **App Display Name** (`name`): The name shown on user home screens (e.g., `"MindAI"`).
2. **URL Scheme / Slug** (`slug` & `scheme`): Lowercase alphanumeric string used for deep linking (e.g., `"mindai"`).
3. **Android Package Name** (`android.package`): Unique reverse-domain identifier for Google Play (e.g., `"com.mycompany.mindai"`).
4. **iOS Bundle Identifier** (`ios.bundleIdentifier`): Unique bundle identifier matching your Apple Developer portal (e.g., `"com.mycompany.mindai"`).
5. **Clean & Regenerate Native Projects**:
   Whenever you change the package name or bundle identifier, run:
   ```bash
   npx expo prebuild --clean
   ```

---

## 2. Replacing App Icon & Splash Screen

All graphic assets are located in the `assets/` directory:

```text
assets/
├── icon.png               # 1024 x 1024 px (Main App Icon)
├── adaptive-icon.png      # 1024 x 1024 px (Android Adaptive Foreground Icon)
└── splash.png             # 1284 x 2778 px or 2048 x 2048 px (Splash Screen)
```

### Step-by-Step Asset Replacement:
1. **App Icon (`assets/icon.png`)**:
   * Dimensions: **1024 x 1024 px**, PNG format.
   * Requirement: Must have **no alpha transparency** for Apple App Store compliance.
2. **Android Adaptive Icon (`assets/adaptive-icon.png`)**:
   * Dimensions: **1024 x 1024 px**, PNG format.
   * The logo artwork should sit inside the center **660 x 660 px safe area** with a transparent background.
   * Set the background color behind the icon in `app.json`:
     ```json
     "android": {
       "adaptiveIcon": {
         "foregroundImage": "./assets/adaptive-icon.png",
         "backgroundColor": "#0A0A0C"
       }
     }
     ```
3. **Splash Screen (`assets/splash.png`)**:
   * Dimensions: **1284 x 2778 px** or **2048 x 2048 px**, PNG format.
   * Center your app logo on a solid background matching `backgroundColor` in `app.json`:
     ```json
     "splash": {
       "image": "./assets/splash.png",
       "resizeMode": "contain",
       "backgroundColor": "#16110D"
     }
     ```
4. **Apply Changes**:
   ```bash
   npx expo prebuild --clean
   ```

---

## 3. Customizing Colors & Theme Palette

The application uses an accessible, algorithmic dark-mode palette system defined in [`src/core/appearancePalette.ts`](file:///src/core/appearancePalette.ts).

### Option A: Change the Global Seed Color (One-line rebrand)
Open `src/core/appearancePalette.ts` at line 17:

```typescript
// Replace this HEX code with your brand color:
export const DEFAULT_SEED = '#6366F1'; // E.g., Indigo: #6366F1 | Emerald: #10B981 | Blue: #3B82F6
```
The palette engine will automatically calculate high-contrast, accessible backgrounds, surfaces, borders, and accent highlights from this single seed color!

### Option B: Override Specific Theme Colors
In `src/core/appearancePalette.ts`, find `createTheme()` (lines 44–52):

```typescript
colors: {
  background: '#090D16',         // Main app background
  surface: '#121826',            // Card and drawer backgrounds
  surfaceHighlight: '#1A2338',   // Selected item background
  surfaceBorder: '#232D48',      // Border outlines
  accent: '#38BDF8',             // Primary button & active indicator color
  text: '#F8FAFC',               // Main primary text color
  textSecondary: '#94A3B8',      // Subtitle & placeholder text
  userBubble: '#1E293B',         // User message chat bubble
  assistantBubble: '#111827',    // AI assistant chat bubble
  success: '#10B981',            // Success badges & status
  warning: '#F59E0B',            // Warning badges
  error: '#EF4444',              // Error states
}
```

### Option C: Update Native Background Colors
Update the system window background in `app.json`:
```json
"backgroundColor": "#090D16"
```

---

## 4. Customizing Interface Strings & Branding

All UI strings and labels can be customized directly in the following component files:

| Element | File Path | Code Location | Default Value |
| :--- | :--- | :--- | :--- |
| **Top App Header Brand** | `src/components/Header.tsx` | Line 12 | `brand="LocalAI"` (or `"Qwerty"`) |
| **Chat Input Placeholder** | `src/components/Composer.tsx` | Line 275 | `'Message LocalAI…'` |
| **AI Assistant Bubble Name** | `src/components/ChatBubble.tsx` | Line 315 | `message.modelName \|\| 'LocalAI'` |
| **Chat Export File Title** | `src/components/HistoryDrawer.tsx`| Line 105 | `'LocalAI'` |
| **Empty State Header** | `src/components/EmptyState.tsx` | Line 45 | `'Start a conversation'` |

### Example: Renaming the Header
In `src/components/Header.tsx`:
```tsx
// Before:
<ChatHeader brand="Qwerty" busy={busy} {...actions} />

// After:
<ChatHeader brand="MindAI" busy={busy} {...actions} />
```

---

## 5. Customizing AI Personas & Prompts

The starter kit comes with 6 tailored personas defined in [`src/constants/models.ts`](file:///src/constants/models.ts).

You can modify, delete, or add new personas to the `PERSONA_PRESETS` array:

```typescript
export const PERSONA_PRESETS: PersonaPreset[] = [
  {
    id: 'companion',
    label: 'Everyday Companion',
    icon: '💬',
    shortDesc: 'A relaxed approach for everyday chat & conversation',
    prompt: 'You are a warm, engaging, and thoughtful assistant. Speak naturally and concisely.',
  },
  {
    id: 'fitness',
    label: 'Fitness Trainer',
    icon: '💪',
    shortDesc: 'Workouts, nutrition planning, and habit tracking',
    prompt: 'You are an elite fitness coach and sports nutritionist. Provide actionable workout splits and meal plans.',
  },
  // Add as many custom personas as your app needs!
];
```

---

## 6. Adding or Changing GGUF Models

Models are configured in [`src/constants/models.ts`](file:///src/constants/models.ts) under `CURATED_MODELS`.

To add any GGUF model hosted on Hugging Face:

```typescript
{
  id: 'llama-3.2-1b-instruct',
  name: 'Llama 3.2 1B Instruct (Fastest)',
  filename: 'llama-3.2-1b-instruct-q4_k_m.gguf',
  sizeBytes: 770000000,
  sizeFormatted: '734 MB',
  downloadUrl: 'https://huggingface.co/bartowski/Llama-3.2-1B-Instruct-GGUF/resolve/main/Llama-3.2-1B-Instruct-Q4_K_M.gguf',
  mirrorDownloadUrl: 'https://hf-mirror.com/bartowski/Llama-3.2-1B-Instruct-GGUF/resolve/main/Llama-3.2-1B-Instruct-Q4_K_M.gguf',
  description: 'Instant general conversational AI by Meta. Ultra-low battery consumption, recommended for all devices.',
  contextLength: 1024,
  recommendedThreads: 2,
  isSpecialist: 'general',
  author: 'Meta AI',
  isDownloaded: false,
}
```

---

## 7. Building for Production

### Step A: Verify Code & Tests
Before compiling release binaries, run the automated verification suite:
```bash
npm run typecheck    # Must pass with 0 errors
npm test             # Verifies all 119 unit & regression tests
```

### Step B: Build with EAS (Recommended by Expo)
Install EAS CLI globally if you haven't already:
```bash
npm install -g eas-cli
eas login
eas build:configure
```

* **Generate Android Production AAB (for Google Play Console)**:
  ```bash
  eas build --platform android --profile production
  ```
* **Generate iOS Production IPA (for Apple App Store / TestFlight)**:
  ```bash
  eas build --platform ios --profile production
  ```

### Step C: Local Offline Release Builds
If building locally without cloud EAS:

* **Android Release APK / AAB**:
  ```bash
  npx expo prebuild --platform android
  cd android
  ./gradlew assembleRelease    # Outputs APK: android/app/build/outputs/apk/release/
  ./gradlew bundleRelease      # Outputs AAB: android/app/build/outputs/bundle/release/
  ```

* **iOS Release Archive**:
  ```bash
  npx expo prebuild --platform ios
  cd ios
  pod install
  # Open YourApp.xcworkspace in Xcode, select 'Any iOS Device', and Product -> Archive.
  ```

---

## 8. Troubleshooting & Verification

* **Issue: Changed package name and app crashes on launch?**  
  *Solution*: Run `npx expo prebuild --clean` to ensure Android and iOS project folders regenerate with the updated bundle identifier.
* **Issue: High memory usage on 3B models?**  
  *Solution*: Open `src/constants/models.ts` and ensure `recommendedThreads: 2` and `contextLength: 1024` are set for memory safety.
* **Issue: Native C++ compile error?**  
  *Solution*: Make sure Android NDK is installed and JDK 17 is active in your terminal.

---

### Need Support?
* Refer to `README.md` for full architecture and background service documentation.
* Check [Expo SDK 52 Documentation](https://docs.expo.dev).
