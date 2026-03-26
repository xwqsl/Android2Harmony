# UI Navigation Analysis

## Project: Simple Gallery Pro

> Auto-generated navigation analysis of MainActivity and its UI components.

| Activity | UI Component | Functional Description | Targeted Activity |
|----------|-------------|----------------------|-------------------|
| MainActivity | MySearchMenu main_menu | Provides search functionality for filtering folders; when search opens with `searchAllFilesByDefault` enabled, launches file search | SearchActivity |
| MainActivity | MyTextView directories_switch_searching | Switches from folder search to file-based search | SearchActivity |
| MainActivity | MyTextView directories_empty_placeholder_2 | When no media with filters: opens filter media dialog; when no media at all: opens file picker to add included folder | N/A (dialog) |
| MainActivity | SwipeRefreshLayout directories_refresh_layout | Pull-to-refresh to reload directory listing | N/A |
| MainActivity | MyRecyclerView directories_grid | Tapping a directory folder item opens that folder's media | MediaActivity |
| MainActivity | MenuItem open_camera | Opens the device camera app | N/A (external) |
| MainActivity | MenuItem show_all | Shows all media files in a single flat view | MediaActivity |
| MainActivity | MenuItem sort | Opens sorting options dialog | N/A (dialog) |
| MainActivity | MenuItem filter | Opens filter media type dialog | N/A (dialog) |
| MainActivity | MenuItem change_view_type | Opens change view type dialog (grid/list) | N/A (dialog) |
| MainActivity | MenuItem temporarily_show_hidden | Toggles temporarily showing hidden folders (may show grant-all-files dialog on Android 11+) | N/A (dialog) |
| MainActivity | MenuItem stop_showing_hidden | Stops temporarily showing hidden folders | N/A |
| MainActivity | MenuItem temporarily_show_excluded | Toggles temporarily showing excluded folders | N/A |
| MainActivity | MenuItem stop_showing_excluded | Stops temporarily showing excluded folders | N/A |
| MainActivity | MenuItem create_new_folder | Opens file picker then new folder creation dialog | N/A (dialog) |
| MainActivity | MenuItem open_recycle_bin | Opens the recycle bin media view | MediaActivity |
| MainActivity | MenuItem column_count | Opens column count selection dialog | N/A (dialog) |
| MainActivity | MenuItem set_as_default_folder | Clears the default folder setting | N/A |
| MainActivity | MenuItem more_apps_from_us | Opens Google Play to show more apps from the developer | N/A (external) |
| MainActivity | MenuItem settings | Opens the settings screen | SettingsActivity |
| MainActivity | MenuItem about | Opens the about screen | AboutActivity |
