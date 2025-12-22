---
title: "Edge-to-Edge Display"
url: /refguide10/mobile/designing-mobile-user-interfaces/edge-to-edge/
weight: 25
description: "This guide explains how to implement edge-to-edge display in Mendix native mobile apps and configure bottom bar layouts."
---

## Introduction

Edge-to-edge display is a modern Android design approach where your app's content extends to the full screen, drawing behind the system bars (status bar at the top and navigation bar at the bottom). This creates a more immersive, modern look by utilizing the entire display area.

## What is Edge-to-Edge Display?

Edge-to-edge display is a modern Android design approach where your app's content extends to the full screen, drawing behind the system bars (status bar at the top and navigation bar at the bottom). This creates a more immersive, modern look by utilizing the entire display area.

**With our latest update, we've made the Android status bar and navigation bar transparent**, allowing your app's design to shine through. This means:

- The **status bar** takes on the color of your app's background (typically the header area)
- The **navigation bar** takes on the color of your app's bottom bar or main background
- System UI elements blend seamlessly with your app's design, creating a cohesive, polished appearance

### Visual Impact

- **Status Bar**: Your app content extends behind the status bar (showing time, battery, notifications)
- **Navigation Bar**: Your app content extends behind the navigation bar (back, home, recent apps buttons)
- **Modern Appearance**: Creates a seamless, full-screen experience where system bars feel like part of your app's design

## How Mendix Handles Edge-to-Edge Display

Mendix automatically implements edge-to-edge display following Google's recommendations to ensure your app looks modern and polished on Android devices:

### Our Solution

1. **Transparent Navigation Bar**: We make the Android navigation bar transparent, allowing it to blend seamlessly with your app's design. This means the navigation bar takes on the background color of your bottom bar, making it appear as an integrated part of your app rather than a separate system element.

2. **Automatic Height Adjustment**: When the Android navigation bar is present, we automatically detect its height and add it to your bottom bar height. This dynamic adjustment ensures that your bottom bar content remains fully visible and accessible, preventing any overlap with the system navigation buttons. 

3. **Seamless Integration**: By combining transparency with height calculation, your app's bottom navigation appears unified with the Android system UI, creating the polished, edge-to-edge experience that Google recommends.

### What This Means for You

- **No Manual Calculations**: You don't need to worry about calculating insets or adjusting for different device configurations
- **Automatic Adaptation**: The bottom bar automatically adjusts its height based on whether the navigation bar is visible
- **Consistent Appearance**: Your app maintains a modern, cohesive look across all Android devices
- **Suggested** You may need to set certain height to your bottom bar if you've customized it.

## ⚠️ Important Warnings and Responsibilities

### Common Issues to Avoid

#### ❌ DO NOT: Place Buttons Near Android Navigation Bar

```
┌─────────────────────────┐
│   Your App Content      │
│                         │
│                         │
│   [Submit Button]       │ ← TOO CLOSE!
└─────────────────────────┘
  [◀  ⚫  ▢] Navigation Bar
```

**Problem**: Users may accidentally tap navigation buttons instead of your app buttons, or your buttons may be partially hidden.

**Solution**: Add sufficient padding/margin at the bottom of your layouts to account for the navigation bar height.

## Configuration via custom-variables.js

Find `// Navigation Styles` within custom-variables.js, where you can adjust the proper design according to your needs. You can find the navigation section here.

### Location
```
<your-native-template>/src/custom-variables.js
```

## Adjusting Bottom Bar Layout (If Needed)

### When You Might Need This

If you've customized your bottom navigation bar and notice layout issues after enabling edge-to-edge display (such as icons or labels appearing cut off or misaligned), you may need to adjust the bottom bar styling.

### Solution: Custom Navigation Styles

Add or modify the following configuration in your `main.js` file to fine-tune the bottom bar appearance:

```javascript
export const navigationStyle = {
    bottomBar: {
        container: {
            height: 60,
        },
        label: {
            fontSize: 18,
        },
        selectedLabel: {
            fontSize: 18,
        },
        icon: {
            size: 25          
        },
        selectedIcon: {
            size: 25    
        }
    },
};
```

### How to Use

1. **Copy** the configuration above into your `main.js` file
2. **Adjust** the values to match your needs:
   - `height`: Increase if your bottom bar content is too close to the Android navigation bar or decrease if it's far away.
   - `label`: Adjust label text size for cases where the Android navigation bar is close to your bottom bar (if height doesn't solve the problem)
   - `icon`: Adjust icon size for cases where the Android navigation bar is close to your bottom bar (if height doesn't solve the problem)

3. **Test** on various Android devices to ensure the layout works well across different screen sizes

### Why This Might Be Necessary

Because the navigation bar is now transparent and your app content extends behind it, any custom bottom bar configurations you previously had may need slight adjustments to account for the new layout. The transparent navigation bar means your bottom bar needs to ensure adequate spacing and sizing for all its elements to remain visible and accessible.