# Android to HarmonyOS UI Conversion Skill

## Overview
This skill provides comprehensive knowledge and guidelines for converting Android UI components and patterns to HarmonyOS (OpenHarmony) UI system. It covers all major UI dimensions including component mapping, layout structures, routing, lifecycle management, and event handling.

## Directory Structure
```
android2harmonyos_ui_conversion/
├── SKILL.md              # Skill definition file
├── README.md             # This file
├── scripts/              # Helper scripts for conversion
│   └── conversion_helper.js  # Conversion utility functions
├── references/           # Reference documentation
│   └── ui_conversion_guide.md  # Detailed conversion guide
└── assets/               # Additional assets (if needed)
```

## Contents

### SKILL.md
Defines the skill's purpose, functionality, and usage. It provides an overview of what the skill covers and how it can be used to help with Android to HarmonyOS UI conversion.

### references/ui_conversion_guide.md
A comprehensive guide covering all 14 dimensions of UI conversion:
1. Page Component Mapping
2. App Layout: Navigation Bar/Sidebar
3. Page Routing
4. Page Lifecycle Management
5. Layout Containers
6. Lists/Grids
7. Custom Views
8. Resource Files (Icons, Text, Colors, Dimensions)
9. Theme/Style
10. Click Events
11. Gesture Handling (Swipe, Long Press, Pinch, Drag)
12. Input Handling
13. Dialogs/Popups
14. Pull-to-Refresh/Load More

Each section includes:
- Android implementation details
- HarmonyOS equivalent implementations
- Conversion guidelines
- Code examples (where applicable)

### scripts/conversion_helper.js
A Node.js script that provides utility functions for:
- Component mapping between Android and HarmonyOS
- Lifecycle method mapping
- Resource file conversion (XML to JSON)
- Layout conversion (Android XML to HarmonyOS ETS)
- Page generation from Android activities

## How to Use This Skill

### For AI Models
This skill is designed to be used by AI models to understand the differences between Android and HarmonyOS UI systems and provide accurate conversion recommendations. It can be referenced when:
- Converting Android apps to HarmonyOS
- Answering questions about UI conversion
- Generating code for HarmonyOS based on Android examples

### For Developers
Developers can use this skill as a reference guide when manually converting Android apps to HarmonyOS. The conversion helper script can also be used to automate some parts of the conversion process.

## Key Features

1. **Comprehensive Coverage**: Covers all major UI dimensions and components
2. **Detailed Mapping**: Provides exact mappings between Android and HarmonyOS components
3. **Code Examples**: Includes practical code examples for common conversion scenarios
4. **Conversion Guidelines**: Offers step-by-step guidance for each conversion task
5. **Helper Scripts**: Includes utility functions to automate parts of the conversion process

## Best Practices

When using this skill for UI conversion, follow these best practices:

1. **Understand the Architecture Differences**: Android uses a view-based system with XML layouts, while HarmonyOS uses a component-based system with ETS layouts
2. **Use HarmonyOS Native Components**: Leverage HarmonyOS-specific components for better performance
3. **Adapt to HarmonyOS Design Guidelines**: Follow HarmonyOS design principles and patterns
4. **Optimize for HarmonyOS Performance**: Use efficient layout components and proper lifecycle management
5. **Test on Multiple Devices**: Ensure UI elements scale appropriately across different screen sizes

## References

- [Android Developer Documentation](https://developer.android.com/docs)
- [HarmonyOS Developer Documentation](https://developer.harmonyos.com/en/docs)
- [OpenHarmony UI Development Guide](https://gitee.com/openharmony/docs/blob/master/en/application-dev/ui/ui-dev-guide.md)
- [Aegis App Conversion Case Study](https://github.com/beemdevelopment/Aegis)
