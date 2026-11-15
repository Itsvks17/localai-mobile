# LocalAI Mobile — Complete Customization & Reskinning Guide

Welcome to the **LocalAI Mobile** customization and reskinning documentation. This comprehensive guide walks you through every step required to transform this starter kit into your own uniquely branded, production-ready on-device AI assistant for Android and iOS.

---

## Table of Contents
1. [Quickstart & Development Setup](#1-quickstart--development-setup)
2. [App Identity: Name, App ID & Package Name](#2-app-identity-name-app-id--package-name)
3. [App Icons & Splash Screen](#3-app-icons--splash-screen)
4. [Color Palettes, Themes & Visual Styling](#4-color-palettes-themes--visual-styling)
5. [Interface Strings & App Text Customization](#5-interface-strings--app-text-customization)
6. [AI Personas & System Prompts](#6-ai-personas--system-prompts)
7. [Default Models & HuggingFace Downloader](#7-default-models--huggingface-downloader)
8. [Building for Production (Google Play & Apple App Store)](#8-building-for-production-google-play--apple-app-store)

---

## 1. Quickstart & Development Setup

### System Prerequisites
* **Node.js**: Version 18.x, 20.x, or 22.x LTS
* **Android Development**: Android Studio, JDK 17, Android SDK (API 34/35)
* **iOS Development**: macOS with Xcode 15 or 16 (iOS 16+ deployment target)
* **Package Manager**: `npm` (included with Node.js)

### Initial Installation
Open your terminal in the root project folder:

```bash
# 1. Install dependencies
npm install

# 2. Run automated verification (verifies types and tests)
npm run typecheck
npm test

# 3. Launch on physical device or emulator
npm run android   # For Android (generates native android/ via Expo prebuild)
npm run ios       # For iOS (generates native ios/ via Expo prebuild)
```

> **Important**: This app uses native C++ bindings (`llama.rn`) and native ML Kit modules. It must be run using native development builds (`npm run android` / `npm run ios`) rather than the standard Expo Go sandboxed client.

---

## 2. App Identity: Name, App ID & Package Name

All high-level app identifiers are configured in `app.json` located at the root of the project.

### Step 2.1: Changing the App Display Name & Slug
Open `app.json` and update the top-level `expo` fields:

```json
{
  "expo": {
    "name": "MyBrand AI",          // <-- Your App's public display name (under phone icon)
    "slug": "mybrand-ai",          // <-- URL-safe project slug
    "scheme": "mybrand",           // <-- Custom URL scheme (e.g., mybrand://chat)
    "version": "1.0.0"             // <-- Your app release version
    // ...
  }
}
```

### Step 2.2: Changing the Android Package Name (Application ID)
In `app.json`, navigate to the `expo.android` block:

```json
{
  "expo": {
    "android": {
      "package": "com.mycompany.mybrandai",  // <-- Replace with your unique Android package name
      // ...
    }
  }
}
```
*Naming rule*: Use reverse domain notation with all lowercase letters (e.g., `com.yourcompany.appname`). Avoid dashes or special symbols.

### Step 2.3: Changing the iOS Bundle Identifier
In `app.json`, navigate to the `expo.ios` block:

```json
{
  "expo": {
    "ios": {
      "bundleIdentifier": "com.mycompany.mybrandai",  // <-- Replace with your unique iOS bundle identifier
      // ...
    }
  }
}
```

### Step 2.4: Updating the Signing Prefix
In `app.json` under `plugins`, look for the `./plugins/withAndroidConfig.cjs` entry:

```json
[
  "./plugins/withAndroidConfig.cjs",
  {
    "signingPrefix": "MYBRAND_RELEASE"    // <-- Update with your custom release prefix
  }
]
```

---

## 3. App Icons & Splash Screen

All image assets are stored inside the `assets/` directory.

### Asset Specifications

| Asset File | Recommended Resolution | Format | Purpose |
| :--- | :--- | :--- | :--- |
| `assets/icon.png` | **1024 × 1024 px** | 32-bit PNG (No alpha for iOS) | Standard App Store & Google Play Icon |
| `assets/adaptive-icon.png` | **1024 × 1024 px** | Transparent PNG (Center icon within 66% safe area) | Android Adaptive Icon Foreground |
| `assets/splash.png` | **1284 × 2778 px** | PNG | Launch / Splash Screen image |

### Step 3.1: Replacing the Files
1. Design your branded icons with the dimensions above.
2. Replace `assets/icon.png`, `assets/adaptive-icon.png`, and `assets/splash.png` with your new designs.

### Step 3.2: Updating Background Colors in `app.json`
Update the background colors that display behind your icons and splash screen:

```json
{
  "expo": {
    "backgroundColor": "#0A0A0C",    // <-- Root window background
    "splash": {
      "image": "./assets/splash.png",
      "resizeMode": "contain",
      "backgroundColor": "#0A0A0C"   // <-- Splash screen background color
    },
    "android": {
      "adaptiveIcon": {
        "foregroundImage": "./assets/adaptive-icon.png",
        "backgroundColor": "#0A0A0C" // <-- Android adaptive icon background color
      }
    }
  }
}
```

### Step 3.3: Applying Asset Changes
Whenever you update icons or `app.json`, regenerate the native projects:

```bash
npx expo prebuild --clean
```

---

## 4. Color Palettes, Themes & Visual Styling

The app includes an intelligent color engine that derives harmonic, high-contrast UI palettes from a seed color or user-selected wallpapers.

### Step 4.1: Changing the Default Theme Seed Color
Open `src/core/appearancePalette.ts` at line 17:

```typescript
// Replace '#8297AB' with your brand's primary hex color:
export const DEFAULT_SEED = '#6366F1'; // Example: Indigo accent
```

### Step 4.2: Overriding Specific UI Colors
In `src/core/appearancePalette.ts`, find the `createTheme` function (lines 43–55). You can override individual color tokens:

```typescript
return {
  colors: {
    background: '#0B0F19',          // <-- Main screen background
    surface: '#111827',             // <-- Chat cards, drawer & dialog surfaces
    surfaceHighlight: '#1F2937',    // <-- Hover/active states
    surfaceBorder: '#374151',       // <-- Subtle borders
    divider: '#1F2937',             // <-- Dividing lines
    accent: '#6366F1',              // <-- Primary buttons, badges, highlights
    accentLight: '#818CF8',         // <-- Secondary highlights
    accentDark: '#4F46E5',          // <-- Button press state
    text: '#F9FAFB',                // <-- Primary text color (high contrast)
    textSecondary: '#D1D5DB',       // <-- Subtitles, secondary text
    textMuted: '#9CA3AF',           // <-- Timestamps, placeholders
    codeBackground: '#030712',      // <-- Code block background
    codeBorder: '#1F2937',          // <-- Code block border
    success: '#10B981',             // <-- Success green
    warning: '#F59E0B',             // <-- Warning amber
    error: '#EF4444',               // <-- Error red
    // ...
  }
};
```

---

## 5. Interface Strings & App Text Customization

To fully reskin the text in the app:

### Step 5.1: Header Brand Name
Open `src/components/Header.tsx` at line 12:

```typescript
// Change "Qwerty" to your app name:
export const Header: React.FC<HeaderProps> = ({ busy, ...actions }) => (
  <ChatHeader brand="MyBrand AI" busy={busy} {...actions} />
);
```

### Step 5.2: Welcome & Empty State Text
Open `app/index.tsx` around line 371:

```tsx
{/* Empty state title and subtitle */}
<Text style={styles.emptyTitle}>MyBrand AI</Text>
<Text style={styles.emptySubtitle}>Your private, on-device intelligence.</Text>

{/* First-time welcome CTA */}
<Text style={styles.welcomeTitle}>Start a conversation</Text>
<TouchableOpacity style={styles.welcomeButton}>
  <Text style={styles.welcomeButtonText}>Choose a model</Text>
</TouchableOpacity>
```

### Step 5.3: Chat Input Placeholder Text
Open `src/components/Composer.tsx` around line 275:

```typescript
// Change input placeholder:
placeholder={
  isModelLoaded
    ? 'Ask MyBrand anything…'       // <-- Replace with your customized placeholder
    : 'Select a model to begin…'
}
```

### Step 5.4: Default Chat Export Filename
Open `app/index.tsx` around line 348:

```typescript
// Set the export header prefix:
await exportChatSession(current, 'MyBrand', alert);
```

---

## 6. AI Personas & System Prompts

The app comes pre-loaded with 6 distinct persona modes. You can edit existing personas or add new ones.

Open `src/constants/models.ts` and inspect the `PERSONA_PRESETS` array (lines 120–163):

```typescript
export const PERSONA_PRESETS: PersonaPreset[] = [
  {
    id: 'companion',
    label: 'Everyday Companion',
    icon: '🧠',
    shortDesc: 'Friendly, concise, helpful conversational assistant',
    prompt: 'You are MyBrand, a friendly, concise, and helpful everyday conversational assistant running 100% offline directly on this mobile device. Assist with questions, daily tasks, ideas, and advice with warm, natural clarity.',
  },
  {
    id: 'writer',
    label: 'Creative Writer',
    icon: '✍️',
    shortDesc: 'Drafts emails, blogs, stories, speeches, captions',
    prompt: 'You are MyBrand in Creative Writer mode. Specialize in drafting expressive emails, engaging blog posts, vivid stories, and compelling social captions with polished grammar.',
  },
  {
    id: 'coder',
    label: 'Developer',
    icon: '💻',
    shortDesc: 'Clean, robust code with syntax formatting',
    prompt: 'You are MyBrand in Developer mode. Provide clean, robust, production-ready code. Always enclose all code inside triple backticks with the language identifier (```lang ... ```).',
  },
  // Add your own custom persona here:
  {
    id: 'legal',
    label: 'Contract Analyst',
    icon: '⚖️',
    shortDesc: 'Reviews agreements, extracts clauses, highlights risks',
    prompt: 'You are an offline contract analysis assistant. Summarize legal documents accurately, highlight indemnities and liabilities, and point out potential risks clearly.',
  }
];
```

---

## 7. Default Models & HuggingFace Downloader

You can customize which local GGUF models are presented in the model manager.

Open `src/constants/models.ts` and edit the `CURATED_MODELS` array (lines 3–110):

```typescript
export const CURATED_MODELS: GGUFModelInfo[] = [
  {
    id: 'llama-3.2-1b-instruct-q4',
    name: 'Llama 3.2 1B Instruct (Fastest)',
    filename: 'Llama-3.2-1B-Instruct-Q4_K_M.gguf',
    sizeBytes: 807694464,
    sizeFormatted: '770 MB',
    // Hugging Face direct download link:
    downloadUrl: 'https://huggingface.co/bartowski/Llama-3.2-1B-Instruct-GGUF/resolve/main/Llama-3.2-1B-Instruct-Q4_K_M.gguf?download=true',
    // Secondary mirror for reliable global downloads:
    mirrorDownloadUrl: 'https://hf-mirror.com/bartowski/Llama-3.2-1B-Instruct-GGUF/resolve/main/Llama-3.2-1B-Instruct-Q4_K_M.gguf?download=true',
    description: 'Instant general conversational AI by Meta. Rapid responses, minimal battery consumption.',
    contextLength: 1024,
    recommendedThreads: 2,
    isSpecialist: 'chat',
    author: 'Meta AI',
    isDownloaded: false,
  },
  // Add additional fine-tuned or custom GGUF models following this exact structure!
];
```

### Performance Tuning Defaults
In `src/constants/models.ts`, find `DEFAULT_INFERENCE_CONFIG` (lines 165–177):
* `contextSize`: Default `1024` (Safe window for mobile RAM; increase to `2048` if targeting high-end devices).
* `threads`: Default `2` (Optimal for mobile batteries; prevents thermal throttling).
* `temperature`: Default `0.65` (Balanced creativity and factual adherence).

---

## 8. Building for Production (Google Play & Apple App Store)

### Option A: Cloud Builds via Expo Application Services (EAS Build) — Recommended

1. Install the EAS CLI globally:
   ```bash
   npm install -g eas-cli
   ```
2. Log in with your Expo account:
   ```bash
   eas login
   ```
3. Configure your project:
   ```bash
   eas build:configure
   ```
4. Build a signed Android App Bundle (AAB) for Google Play:
   ```bash
   eas build --platform android --profile production
   ```
5. Build an iOS IPA archive for Apple TestFlight / App Store:
   ```bash
   eas build --platform ios --profile production
   ```

---

### Option B: Local Android Gradle Build

If you prefer building locally without EAS:

1. Generate your release Keystore:
   ```bash
   keytool -genkeypair -v -keystore release.keystore -alias release -keyalg RSA -keysize 2048 -validity 10000
   ```
2. Move `release.keystore` to `android/app/`.
3. In `android/gradle.properties`, add your keystore credentials:
   ```properties
   MYAPP_RELEASE_STORE_FILE=release.keystore
   MYAPP_RELEASE_KEY_ALIAS=release
   MYAPP_RELEASE_STORE_PASSWORD=your_keystore_password
   MYAPP_RELEASE_KEY_PASSWORD=your_key_password
   ```
4. Build the release App Bundle (.aab):
   ```bash
   cd android
   ./gradlew bundleRelease
   ```
   *Your production bundle will be generated at:*  
   `android/app/build/outputs/bundle/release/app-release.aab`

---

## Need Support or Have Questions?

Feel free to post questions or open issues on our repository:
* **GitHub Repository**: https://github.com/Itsvks17/localai-mobile
* **Gumroad Store**: https://darkryoshin.gumroad.com/l/localai-mobile

Happy Building! 🚀
