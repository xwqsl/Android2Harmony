---
name: android_ui_navigation_analyzer
description: Hierarchically analyze all UI components and their navigation targets for a given Activity in an Android app project. For the specified Activity, identifies every actionable UI component (buttons, links, clickable views, menu items, list items) and determines what happens when clicked — whether it navigates to another Activity, switches to a Fragment, opens a dialog that returns to the same Activity, or triggers an in-screen state change. Outputs a markdown file with an Activity functional overview followed by a five-column table (Activity, Component ID, UI Component, Functional Description, Targeted Activity). Use this skill whenever the user wants to understand Android app navigation flow, map out screen transitions, find which buttons lead where, analyze click handlers and their destinations, or generate a navigation map of an Android project. Also triggers when the user asks about "what happens when I click X" or "where does this button go" in an Android context.
---

# Android UI Navigation Analyzer

Hierarchically analyze a given Activity in an Android app project to extract every actionable UI component and determine the action triggered by clicking each one. The result is a markdown file that begins with a functional description of the Activity, followed by a table mapping all interactive components to their navigation/action targets.

## Inputs

- **Android Project Path**: `$ARGUMENTS[0]` — root directory of the Android project (e.g., `/path/to/MyApp`)
- **Output File Path**: `$ARGUMENTS[1]` — where to write the result markdown file (e.g., `/path/to/output/ui-navigation.md`)
- **Target Activity**: extracted from the user's prompt — the specific Activity class to analyze (e.g., `MainActivity`). If not explicitly stated, analyze all Activities discovered from the manifest.

## Output

A single markdown file written to the output path, containing:
1. A **functional description section** for the analyzed Activity (purpose, role in the app, key behaviors)
2. A **five-column table** listing every actionable UI component

| Activity | Component ID | UI Component | Functional Description | Targeted Activity |
|----------|-------------|-------------|----------------------|-------------------|

---

## Step 1 — Locate the Target Activity and Its Full Code Surface

1. Parse `AndroidManifest.xml` to confirm the Activity exists and gather its metadata (theme, parent, intent filters, launch mode).
2. Resolve the Activity to its `.java` or `.kt` source file under `app/src/main/java/` or `app/src/main/kotlin/`.
3. **Identify the complete code surface** — the analysis must cover ALL of the following:
   - The Activity source file itself (including inner classes, companion objects, anonymous classes)
   - The Activity's **parent/base classes** — trace the inheritance chain (e.g., `MainActivity → SimpleActivity → BaseSimpleActivity`). Any click handler, menu setup, or navigation method defined in a parent class that the Activity inherits or overrides MUST be included.
   - **Extension functions** — search for Kotlin extension functions on the Activity class or its parent classes (e.g., `fun SimpleActivity.launchSettings()`, `fun Activity.openRecycleBin()`). These are often defined in separate files like `extensions/Activity.kt`.
   - **Adapter classes** — if the Activity uses a RecyclerView/ListView with an adapter, locate the adapter class and analyze its item click handlers (e.g., `onItemClick`, `onBindViewHolder` with click listeners).
   - **Fragment classes** hosted by this Activity
   - The primary layout XML via `setContentView(R.layout.xxx)` or DataBinding/ViewBinding inflation
   - Any additionally inflated layouts (dialogs, included layouts via `<include>`, custom views)
   - Menu resources (via `onCreateOptionsMenu`, `MenuInflater`, toolbar inflation)
   - Navigation graph XML if `NavHostFragment` or `NavController` is used

This step is critical: missing an extension function file or a parent class method will cause components to be omitted from the final table. Be thorough.

---

## Step 2 — Write the Activity Functional Description

Before building the table, compose a clear functional description of the Activity. This section should appear at the top of the output file and cover:

- **Purpose**: What is this Activity's primary role? (e.g., "Main gallery screen showing folder directories")
- **Entry points**: How does the user reach this Activity? (launcher, intent filter, navigation from another Activity)
- **Key behaviors**: What are the major functions? (e.g., "Displays photo/video folders in grid/list view, supports search, pull-to-refresh, folder management")
- **Intent modes**: Does the Activity behave differently based on intent type? (e.g., pick image intent vs. normal launch)
- **Layout structure**: Brief overview of the layout hierarchy (e.g., "CoordinatorLayout with a search menu bar, a RecyclerView grid for directories, and empty-state placeholder views")

---

## Step 3 — Hierarchically Analyze UI Components

Perform a hierarchical, depth-first analysis of the Activity's UI tree. "Hierarchical" means you analyze the layout XML top-down, preserving parent-child nesting, and for each level you combine XML attributes with source-code click handlers.

### 3a — Layout XML Hierarchical Analysis

Parse the layout XML file(s) associated with this Activity in document order (top to bottom). For each XML element:

1. Record the **view type** (e.g., `Button`, `ImageButton`, `TextView`, `ImageView`, `RecyclerView`, `CardView`)
2. Record the **view ID** from `android:id="@+id/xxx"` — strip the `@+id/` or `@id/` prefix
3. Record the **nesting level** — which parent container the view belongs to (for hierarchical context)
4. Check if the view is inherently clickable (Button, ImageButton, FloatingActionButton, Chip, etc.) or has `android:clickable="true"` or `android:onClick="methodName"` set in XML
5. Resolve `android:text` or `android:contentDescription` via `@string/` references in `strings.xml` to get human-readable labels
6. Recurse into `<include>` and `<merge>` tags to capture included layouts
7. For `<fragment>` tags, record the fragment class from `android:name` or `class` attribute

### 3b — Source Code Analysis (Click Handlers & Navigation)

Scan ALL source files identified in Step 1 (the Activity, its parent classes, extension functions, adapters) for click handlers bound to views in this Activity:

**Click listeners to look for:**
- `setOnClickListener` / `setOnLongClickListener`
- `android:onClick="methodName"` XML attribute → find the corresponding method in the Activity or parent class
- `OnItemClickListener` for list/grid views
- `OnNavigationItemSelectedListener` for bottom navigation
- `OnMenuItemClickListener` for toolbar/menu items
- Lambda-based and method-reference listeners in Kotlin
- View binding click assignments (e.g., `binding.btnLogin.setOnClickListener { ... }`)
- Adapter item click callbacks (e.g., lambda passed to adapter constructor, interface callbacks)
- `OnRefreshListener` for SwipeRefreshLayout

**Navigation/action patterns to detect:**
- `Intent(this, TargetActivity::class.java)` followed by `startActivity()` or `startActivityForResult()` → **navigates to TargetActivity**
- `NavController.navigate(R.id.destination)` → resolve from `nav_graph.xml` → **navigates to destination**
- `FragmentTransaction.replace()` / `.add()` → **switches to Fragment** (stays in same Activity)
- `finish()` → **returns to previous Activity** (back navigation)
- `DialogFragment.show()` / `AlertDialog.Builder` / custom dialog → **opens dialog** (stays in same Activity)
- `startService()` / `bindService()` → record as service interaction (stays in same Activity)
- Deep link intents with explicit URIs → **opens external app/browser**
- `ActivityResultLauncher` / `registerForActivityResult` patterns
- State-change actions (toggling visibility, refreshing data, updating adapter) → **stays in same Activity**
- Callbacks that call methods on the same Activity (e.g., `getDirectories()`, `setupAdapter()`) → **stays in same Activity** (self-action)

**For each detected click handler, determine the action category:**
- **Same Activity (self-action)**: The click triggers a state change, data reload, UI update, or dialog within the current Activity. The Targeted Activity column shows the current Activity name itself (e.g., `MainActivity`).
- **Another Activity**: The click navigates to a different Activity. The Targeted Activity column shows the target Activity name.
- **Fragment switch**: The click replaces/adds a Fragment within the current Activity. The Targeted Activity column shows `FragmentName (Fragment)`.
- **External**: The click launches an external app. The Targeted Activity column shows `N/A (external)`.
- **Back**: The click calls `finish()` or navigates back. The Targeted Activity column shows `[Back]`.

### 3c — Menu Analysis

For each menu XML resource inflated by the Activity:

1. Parse `res/menu/*.xml` to extract all `<item>` elements with their `android:id` and `android:title`
2. Find the corresponding `onOptionsItemSelected` / `onNavigationItemSelected` / `setOnMenuItemClickListener` handler in the Activity source or parent class
3. Trace each menu item ID to its handler method and determine the action/target
4. Note: the Activity may inflate different menus based on runtime state (e.g., `mIsThirdPartyIntent`). Analyze ALL menu variants.

### 3d — Fragment Analysis

For each Fragment hosted by this Activity:

1. Locate the Fragment's source file and layout XML
2. Apply the same hierarchical analysis (Steps 3a and 3b) to discover the Fragment's actionable components
3. Record the Fragment's components under the hosting Activity in the output table, with the Activity column showing `ActivityName > FragmentName` to indicate the hierarchy

### 3e — Adapter / RecyclerView Item Analysis

If the Activity uses a RecyclerView or ListView:

1. Identify the adapter class (from source code where the adapter is instantiated)
2. Analyze the adapter for item click handlers — both in `onBindViewHolder`/`getView` and via constructor-passed click callbacks
3. Each adapter item click is a valid row in the table. Use the view type from the item layout (e.g., `DirectoryItem (list item)`) and record what happens on click.

---

## Step 4 — Build the Functional Description for Each Component

For each actionable UI component, compose a concise functional description that explains what happens when the user interacts with it:

- **Navigation actions**: "Navigates to [screen name]", "Opens [feature] screen"
- **Self-actions**: "Refreshes directory listing", "Filters displayed media types", "Changes column count"
- **Dialog actions**: "Shows sorting options dialog", "Opens folder creation dialog"
- **System actions**: "Shares content via intent", "Opens camera app", "Launches external browser"
- **State changes**: "Toggles hidden folder visibility", "Switches view type between grid and list"
- **Back navigation**: "Returns to previous screen", "Closes current screen"

Every component in the table MUST have an action. Do NOT include components that have no click handler and no action. The table should only contain components that do something when the user interacts with them.

---

## Step 5 — Write the Markdown Output

Write the result to the specified output file path with this structure:

```markdown
# UI Navigation Analysis

## Project: [App Name from AndroidManifest or project directory name]

> Auto-generated navigation analysis.

---

## [ActivityName]

### Functional Description

[2-4 paragraphs describing the Activity's purpose, entry points, key behaviors, intent modes, and layout structure. This should give a reader a complete understanding of what this screen does without reading the source code.]

### UI Components

| Activity | Component ID | UI Component | Functional Description | Targeted Activity |
|----------|-------------|-------------|----------------------|-------------------|
| MainActivity | main_menu | MySearchMenu | Provides search bar for filtering folders; launches file search when searchAllFilesByDefault is enabled | SearchActivity / MainActivity |
| MainActivity | directories_switch_searching | MyTextView | Switches from folder search to file-based search | SearchActivity |
| MainActivity | directories_grid | MyRecyclerView > DirectoryItem | Tapping a directory opens its media content | MediaActivity |
| MainActivity | open_camera | MenuItem | Opens the device camera app | N/A (external) |
| MainActivity | settings | MenuItem | Opens the settings screen | SettingsActivity |
| MainActivity > SignUpFragment | btn_register | Button | Submits registration form | VerificationActivity |
| ... | ... | ... | ... | ... |
```

**Table formatting rules:**
- **Ordering**: Sort rows hierarchically — layout views first (in XML document order, top to bottom), then menu items (in menu XML order), then adapter item clicks, then Fragment components
- **Activity column**: Use the simple Activity class name. For Fragment components, use `ActivityName > FragmentName`.
- **Component ID column**: The raw ID string (e.g., `main_menu`, `btn_login`, `open_camera`). For menu items, use the menu item ID. For adapter items with no direct ID, use a descriptive identifier like `directory_item_click`.
- **UI Component column**: `ViewType` (e.g., `Button`, `MyTextView`, `MenuItem`, `MyRecyclerView > DirectoryItem`). For adapter items, append the item type.
- **Functional Description column**: A concise description of what happens on click.
- **Targeted Activity column**: The destination, using these conventions:
  - `ActivityName` — navigates to that Activity
  - `ActivityName (self)` — stays in the same Activity (dialog, state change, data reload)
  - `FragmentName (Fragment)` — switches to a Fragment within the same Activity
  - `N/A (external)` — opens an external app or browser
  - `[Back]` — calls `finish()` or navigates back
  - For conditional navigation, list all possibilities separated by ` / ` (e.g., `LoginActivity / DashboardActivity`)

---

## Step 6 — Consistency Verification (Critical)

This step is mandatory. After generating the table, re-read ALL source files and verify every entry. The goal is zero false positives (components listed that don't actually exist or have wrong targets) and zero false negatives (real actionable components missing from the table).

### Verification Pass 1 — Completeness (no missed components)

```pseudocode
// Check layout XML
for each view in the layout XML hierarchy:
    if view has setOnClickListener in source OR android:onClick in XML OR is inherently clickable:
        assert view appears in the table

// Check source code click handlers
for each setOnClickListener / setOnLongClickListener call in Activity + parent classes + extensions:
    resolve the view ID
    assert that view ID appears in the table

// Check menu items
for each menu item ID handled in onOptionsItemSelected / onMenuItemClickListener:
    assert that menu item appears in the table

// Check adapter click handlers
for each adapter item click callback:
    assert it appears in the table

// Check Fragments
for each hosted Fragment:
    run the same checks on the Fragment's layout + source
```

### Verification Pass 2 — Accuracy (no wrong entries)

```pseudocode
for each row in the table:
    // Verify the component exists
    assert the Component ID matches a real view ID in XML or a real menu item ID

    // Verify the click handler exists
    assert there is actual source code that sets a click listener on this component

    // Verify the target
    trace the click handler code path:
        if it calls startActivity(Intent(..., Target::class.java)):
            assert Targeted Activity == Target
        if it calls finish():
            assert Targeted Activity == [Back]
        if it shows a dialog / changes state / reloads data:
            assert Targeted Activity == ActivityName (self)
        if it opens an external intent:
            assert Targeted Activity == N/A (external)

    // Verify no fabrication
    assert the functional description accurately matches the source code behavior
```

### Verification Pass 3 — Cross-reference

```pseudocode
// Count all unique actionable component IDs found in source
source_ids = collect all view IDs that have click handlers in source + XML
// Count all Component IDs in the table
table_ids = collect all Component IDs from the table

assert source_ids == table_ids  // no extras, no missing
if mismatch:
    list the differences and fix the table
```

Run all three passes. If any check fails, fix the table and re-run until clean.

---

## Iron Rules

1. **Only actionable components** — every row in the table must represent a UI component that has a real click handler or action. Never include static/decorative views that the user cannot interact with.
2. **Accurate targets** — navigation targets must be verified against actual source code. Never guess based on naming conventions. Trace the full code path from click handler to `startActivity()` / `finish()` / dialog / state change.
3. **Complete code surface** — analyze the Activity source, ALL parent classes, ALL extension function files, ALL adapter classes, and ALL Fragment classes. Missing any of these sources will cause false negatives.
4. **Resolve references** — look up `@string/` references to provide human-readable labels. Resolve view binding names to their XML IDs (e.g., `binding.directoriesSwitchSearching` → `directories_switch_searching`).
5. **Handle conditional behavior** — if a click handler's behavior depends on runtime state (e.g., different actions for third-party intent mode vs. normal mode), list all possible targets separated by ` / `.
6. **Hierarchical ordering** — present components in layout hierarchy order (XML document order), not alphabetical. This helps readers understand the spatial layout of the screen.
7. **No permission needed** — you have full read access to the Android project directory. Proceed without asking for confirmation.
