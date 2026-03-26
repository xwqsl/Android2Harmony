# UI Navigation Analysis

## Project: Simple Gallery Pro

> Auto-generated navigation analysis of all Activities in the Simple Gallery Android project.

---

## SplashActivity

### Functional Description

SplashActivity is the application's launch entry point. It extends `BaseSplashActivity` from the Simple Mobile Tools commons library and serves as a one-time initialization gate before the main UI is shown.

**Entry Points:** This is the LAUNCHER activity, invoked by the Android system when the user taps the app icon (through various color-themed activity-aliases).

**Key Behaviors:**
- On first launch (`appRunCount == 0`), it simply marks favorites as migrated and proceeds.
- On subsequent launches, it checks whether legacy favorite media items have been migrated from the old `Medium` table into the dedicated `Favorites` table. This migration runs on a background thread.
- After initialization is complete, it unconditionally navigates to `MainActivity` and calls `finish()` on itself.

**Layout Structure:** No custom layout. It inherits the splash/transparent theme from `BaseSplashActivity`.

### UI Components

| Activity | Component ID | UI Component | Functional Description | Targeted Activity |
|----------|-------------|-------------|----------------------|-------------------|
| SplashActivity | N/A (programmatic) | `launchActivity()` method | After favorite migration completes, launches the main screen and finishes itself | MainActivity |

---

## MainActivity

### Functional Description

MainActivity is the primary screen of the gallery application. It displays a grid (or list) of media directories (folders) fetched from the device's storage and internal database cache. The user browses folders, then taps into a folder to view its media.

**Entry Points:**
- Launched from `SplashActivity` as the normal app flow.
- Can be launched by external apps via implicit intents: `ACTION_PICK`, `ACTION_GET_CONTENT`, `ACTION_SET_WALLPAPER`.

**Intent Modes:**
When launched by a third-party intent, the activity enters "picker" mode (`mIsPickImageIntent`, `mIsPickVideoIntent`, `mIsGetImageContentIntent`, `mIsGetVideoContentIntent`, `mIsGetAnyContentIntent`, `mIsSetWallpaperIntent`). In picker mode a reduced menu (`menu_main_intent`) is inflated, and the selected media result is returned to the calling app.

**Key Behaviors:**
- Password protection gate on launch.
- Default folder auto-open to `MediaActivity`.
- Show All mode navigates to `MediaActivity` with all media.
- Grouped subfolder drill-down within the same Activity.
- Pull-to-refresh to reload directories.
- Inline search with optional redirect to `SearchActivity`.
- Pinch-to-zoom column count in grid view.
- Content observer for real-time media change detection.

**Layout Structure (`activity_main.xml`):**
- `CoordinatorLayout` > `MySearchMenu` (toolbar) + `RelativeLayout` containing switch-searching link, empty placeholders, `SwipeRefreshLayout` > `RecyclerViewFastScroller` > `MyRecyclerView` (directory grid/list).

### UI Components

| Activity | Component ID | UI Component | Functional Description | Targeted Activity |
|----------|-------------|-------------|----------------------|-------------------|
| MainActivity | directories_grid (item click) | DirectoryAdapter item | Taps a directory; if grouped subfolders drills down (same Activity), otherwise opens folder media | MediaActivity |
| MainActivity | open_camera | MenuItem | Launches the device camera app | N/A (external) |
| MainActivity | show_all | MenuItem | Sets showAll mode, opens all media view, finishes self | MediaActivity |
| MainActivity | sort | MenuItem | Opens ChangeSortingDialog for directory sort order | MainActivity (self) |
| MainActivity | filter | MenuItem | Opens FilterMediaDialog for media type filters | MainActivity (self) |
| MainActivity | change_view_type | MenuItem | Opens ChangeViewTypeDialog for grid/list toggle | MainActivity (self) |
| MainActivity | temporarily_show_hidden | MenuItem | Toggles hidden folder visibility (may show GrantAllFilesDialog) | MainActivity (self) |
| MainActivity | stop_showing_hidden | MenuItem | Reverts temporary hidden folder visibility | MainActivity (self) |
| MainActivity | temporarily_show_excluded | MenuItem | Toggles excluded folder visibility | MainActivity (self) |
| MainActivity | stop_showing_excluded | MenuItem | Reverts temporary excluded folder visibility | MainActivity (self) |
| MainActivity | create_new_folder | MenuItem | Opens FilePickerDialog + CreateNewFolderDialog | MainActivity (self) |
| MainActivity | open_recycle_bin | MenuItem | Opens recycle bin media listing | MediaActivity |
| MainActivity | column_count | MenuItem | Opens RadioGroupDialog for grid column count | MainActivity (self) |
| MainActivity | set_as_default_folder | MenuItem | Clears default folder config | MainActivity (self) |
| MainActivity | settings | MenuItem | Opens app settings screen | SettingsActivity |
| MainActivity | about | MenuItem | Opens About screen with FAQ and licenses | AboutActivity (commons) |
| MainActivity | more_apps_from_us | MenuItem | Opens Google Play Store listing | N/A (external) |
| MainActivity | directories_switch_searching | MyTextView | Launches dedicated file search screen | SearchActivity |
| MainActivity | directories_empty_placeholder_2 | MyTextView | Opens add-folder dialog or filter dialog | MainActivity (self) |
| MainActivity | directories_refresh_layout | SwipeRefreshLayout | Pull-to-refresh reloads directories | MainActivity (self) |
| MainActivity | N/A (search open) | MySearchMenu | If searchAllFilesByDefault, opens SearchActivity on search focus | SearchActivity |

#### DirectoryAdapter CAB Actions (long-press multi-select)

| Activity | Component ID | UI Component | Functional Description | Targeted Activity |
|----------|-------------|-------------|----------------------|-------------------|
| MainActivity (DirectoryAdapter) | cab_properties | CAB MenuItem | Shows PropertiesDialog for selected directory | MainActivity (self) |
| MainActivity (DirectoryAdapter) | cab_rename | CAB MenuItem | Opens RenameItemDialog/RenameItemsDialog | MainActivity (self) |
| MainActivity (DirectoryAdapter) | cab_pin / cab_unpin | CAB MenuItem | Pins/unpins selected folders to top | MainActivity (self) |
| MainActivity (DirectoryAdapter) | cab_change_order | CAB MenuItem | Enables drag-and-drop reordering | MainActivity (self) |
| MainActivity (DirectoryAdapter) | cab_hide / cab_unhide | CAB MenuItem | Adds/removes .nomedia file in folder | MainActivity (self) |
| MainActivity (DirectoryAdapter) | cab_exclude | CAB MenuItem | Opens ExcludeFolderDialog | MainActivity (self) |
| MainActivity (DirectoryAdapter) | cab_lock / cab_unlock | CAB MenuItem | Opens SecurityDialog for folder password | MainActivity (self) |
| MainActivity (DirectoryAdapter) | cab_copy_to | CAB MenuItem | Opens PickDirectoryDialog to copy media | MainActivity (self) |
| MainActivity (DirectoryAdapter) | cab_move_to | CAB MenuItem | Opens PickDirectoryDialog to move media | MainActivity (self) |
| MainActivity (DirectoryAdapter) | cab_select_all | CAB MenuItem | Selects all directories | MainActivity (self) |
| MainActivity (DirectoryAdapter) | cab_create_shortcut | CAB MenuItem | Creates home-screen shortcut for directory | MediaActivity (via shortcut) |
| MainActivity (DirectoryAdapter) | cab_delete | CAB MenuItem | Deletes selected folders or empties recycle bin | MainActivity (self) |
| MainActivity (DirectoryAdapter) | cab_empty_recycle_bin | CAB MenuItem | Empties recycle bin | MainActivity (self) |
| MainActivity (DirectoryAdapter) | cab_empty_disable_recycle_bin | CAB MenuItem | Empties and disables recycle bin | MainActivity (self) |
| MainActivity (DirectoryAdapter) | cab_select_photo / cab_use_default | CAB MenuItem | Changes album cover image | MainActivity (self) |
| MainActivity (DirectoryAdapter) | cab_move_to_top / cab_move_to_bottom | CAB MenuItem | Moves items during reorder mode | MainActivity (self) |

---

## MediaActivity

### Functional Description

MediaActivity displays a grid or list of media items (photos, videos, GIFs, RAWs, SVGs, portraits) belonging to a specific folder or, when in "Show All" mode, all media on the device. It also serves as the pick/get intent handler and wallpaper-setting screen.

**Entry Points:**
- Launched from `MainActivity` or `DirectoryAdapter` with a `DIRECTORY` extra.
- Launched from `openRecycleBin()` with `DIRECTORY = RECYCLE_BIN`.
- Can be root activity when `config.showAll` is true.
- Launched via GET_IMAGE_INTENT, GET_VIDEO_INTENT, GET_ANY_INTENT, or SET_WALLPAPER_INTENT flags.

**Key Behaviors:**
- Pull-to-refresh, pinch-to-zoom columns, inline search filtering.
- Password-protected/locked folder handling.
- Periodic polling for new/deleted media.
- Supports grid/list view with horizontal scrolling option.

**Layout Structure (`activity_media.xml`):**
- `CoordinatorLayout` > `MySearchMenu` + `RelativeLayout` > empty placeholders, `SwipeRefreshLayout` > `RecyclerViewFastScroller` > `MyRecyclerView` (media grid).

### UI Components

| Activity | Component ID | UI Component | Functional Description | Targeted Activity |
|----------|-------------|-------------|----------------------|-------------------|
| MediaActivity | media_grid (item click, image) | RecyclerView item | Opens image in fullscreen pager | ViewPagerActivity |
| MediaActivity | media_grid (item click, video) | RecyclerView item | Opens video with system/default handler | N/A (external) |
| MediaActivity | media_grid (item click, pick) | RecyclerView item | Returns selected URI as result, finishes | [Back] |
| MediaActivity | media_grid (item click, wallpaper) | RecyclerView item | Sets image as wallpaper, finishes | [Back] |
| MediaActivity | slideshow | MenuItem | Starts slideshow from first medium | ViewPagerActivity |
| MediaActivity | folder_view | MenuItem (Show All mode) | Switches to folder browsing view | MainActivity |
| MediaActivity | open_camera | MenuItem (Show All mode) | Launches device camera | N/A (external) |
| MediaActivity | settings | MenuItem | Opens settings screen | SettingsActivity |
| MediaActivity | about | MenuItem (Show All mode) | Opens About screen | AboutActivity (commons) |
| MediaActivity | open_recycle_bin | MenuItem | Opens recycle bin listing | MediaActivity (self) |
| MediaActivity | sort | MenuItem | Opens ChangeSortingDialog | MediaActivity (self) |
| MediaActivity | filter | MenuItem | Opens FilterMediaDialog | MediaActivity (self) |
| MediaActivity | change_view_type | MenuItem | Opens ChangeViewTypeDialog | MediaActivity (self) |
| MediaActivity | group | MenuItem (grid only) | Opens ChangeGroupingDialog | MediaActivity (self) |
| MediaActivity | toggle_filename | MenuItem (grid only) | Toggles filename display | MediaActivity (self) |
| MediaActivity | column_count | MenuItem (grid only) | Opens column count dialog | MediaActivity (self) |
| MediaActivity | empty_recycle_bin | MenuItem (Recycle Bin) | Empties recycle bin, finishes | [Back] |
| MediaActivity | empty_disable_recycle_bin | MenuItem (Recycle Bin) | Empties/disables recycle bin, finishes | [Back] |
| MediaActivity | restore_all_files | MenuItem (Recycle Bin) | Restores all deleted files, finishes | [Back] |
| MediaActivity | create_new_folder | MenuItem | Creates subfolder | MediaActivity (self) |
| MediaActivity | temporarily_show_hidden | MenuItem | Toggles hidden files visibility | MediaActivity (self) |
| MediaActivity | stop_showing_hidden | MenuItem | Reverts hidden visibility | MediaActivity (self) |
| MediaActivity | set_as_default_folder | MenuItem | Sets folder as default startup | MediaActivity (self) |
| MediaActivity | unset_as_default_folder | MenuItem | Removes folder as default | MediaActivity (self) |
| MediaActivity | media_empty_text_placeholder_2 | TextView | Opens FilterMediaDialog | MediaActivity (self) |
| MediaActivity | media_refresh_layout | SwipeRefreshLayout | Pull-to-refresh | MediaActivity (self) |

#### MediaAdapter CAB Actions

| Activity | Component ID | UI Component | Functional Description | Targeted Activity |
|----------|-------------|-------------|----------------------|-------------------|
| MediaActivity (MediaAdapter) | cab_edit | CAB MenuItem | Opens external image editor | N/A (external) |
| MediaActivity (MediaAdapter) | cab_open_with | CAB MenuItem | Opens file with app chooser | N/A (external) |
| MediaActivity (MediaAdapter) | cab_set_as | CAB MenuItem | Opens "Set as" chooser | N/A (external) |
| MediaActivity (MediaAdapter) | cab_share | CAB MenuItem | Shares via Android share sheet | N/A (external) |
| MediaActivity (MediaAdapter) | cab_copy_to | CAB MenuItem | Opens directory picker to copy | MediaActivity (self) |
| MediaActivity (MediaAdapter) | cab_move_to | CAB MenuItem | Opens directory picker to move | MediaActivity (self) |
| MediaActivity (MediaAdapter) | cab_rename | CAB MenuItem | Opens rename dialog | MediaActivity (self) |
| MediaActivity (MediaAdapter) | cab_delete | CAB MenuItem | Deletes or moves to recycle bin | MediaActivity (self) |
| MediaActivity (MediaAdapter) | cab_properties | CAB MenuItem | Shows file properties dialog | MediaActivity (self) |
| MediaActivity (MediaAdapter) | cab_create_shortcut | CAB MenuItem | Creates launcher shortcut | ViewPagerActivity (via shortcut) |
| MediaActivity (MediaAdapter) | cab_confirm_selection | CAB MenuItem | Confirms multi-pick, returns paths | [Back] |
| MediaActivity (MediaAdapter) | cab_hide / cab_unhide | CAB MenuItem | Toggles file hidden state | MediaActivity (self) |
| MediaActivity (MediaAdapter) | cab_add_to_favorites / cab_remove_from_favorites | CAB MenuItem | Toggles favorite status | MediaActivity (self) |
| MediaActivity (MediaAdapter) | cab_restore_recycle_bin_files | CAB MenuItem | Restores deleted files | MediaActivity (self) |
| MediaActivity (MediaAdapter) | cab_rotate_right / left / one_eighty | CAB MenuItem | Rotates images | MediaActivity (self) |
| MediaActivity (MediaAdapter) | cab_fix_date_taken | CAB MenuItem | Fixes date metadata from EXIF | MediaActivity (self) |
| MediaActivity (MediaAdapter) | cab_resize | CAB MenuItem | Opens resize dialog | MediaActivity (self) |
| MediaActivity (MediaAdapter) | cab_select_all | CAB MenuItem | Selects all items | MediaActivity (self) |

---

## SearchActivity

### Functional Description

SearchActivity provides a global full-text search across all media files on the device. It loads all media into memory, then filters in real-time as the user types.

**Entry Points:**
- Launched from `MainActivity` via the search action or "Switch to file search" link.

**Key Behaviors:**
- Auto-focused search bar on launch.
- Real-time case-insensitive filename filtering, with query-starting results sorted first.
- Uses `MediaAdapter` with same CAB operations as MediaActivity.
- Back arrow clears search first, then finishes activity.

**Layout Structure (`activity_search.xml`):**
- `CoordinatorLayout` > `MySearchMenu` + `RelativeLayout` > empty placeholder, `RecyclerViewFastScroller` > `MyRecyclerView`.

### UI Components

| Activity | Component ID | UI Component | Functional Description | Targeted Activity |
|----------|-------------|-------------|----------------------|-------------------|
| SearchActivity | search_grid (item click, image) | RecyclerView item | Opens image in fullscreen pager | ViewPagerActivity |
| SearchActivity | search_grid (item click, video) | RecyclerView item | Opens video with system handler | N/A (external) |
| SearchActivity | search_menu (back, empty) | MySearchMenu back | Finishes activity | [Back] |
| SearchActivity | search_menu (back, non-empty) | MySearchMenu back | Clears search text | SearchActivity (self) |
| SearchActivity | toggle_filename | MenuItem | Toggles filename display | SearchActivity (self) |
| SearchActivity | search_menu (text input) | MySearchMenu | Filters media by name as user types | SearchActivity (self) |
| SearchActivity | (CAB actions) | MediaAdapter CAB | Same CAB actions as MediaActivity | (same as MediaActivity CAB) |

---

## ViewPagerActivity

### Functional Description

ViewPagerActivity is a full-screen media viewer allowing horizontal swiping through images and videos. It serves as the single-item detail view for the gallery.

**Entry Points:**
- Launched from `MediaActivity`, `SearchActivity`, or other activities with a `PATH` extra.
- External `ACTION_VIEW` / camera review intents with `IS_VIEW_INTENT`.
- Homescreen shortcuts.
- Can auto-start slideshow with `SLIDESHOW_START_ON_ENTER`.

**Key Behaviors:**
- Uses `MyPagerAdapter` (`FragmentStatePagerAdapter`) hosting `PhotoFragment` or `VideoFragment`.
- Fullscreen toggle on tap.
- Slideshow mode with configurable interval, animation, direction, looping.
- Orientation locking and auto-rotation by aspect ratio.
- Comprehensive file operations (delete, copy, move, rename, hide, share, set as, edit, rotate, resize, save as, print, map, favorite, restore, shortcut).
- Bottom action bar with configurable visible buttons.

**Layout Structure (`activity_medium.xml`):**
- `RelativeLayout` > `MyViewPager` + included `bottom_actions.xml` (ConstraintLayout with action ImageViews) + `top_shadow` + `AppBarLayout` > `MaterialToolbar`.

### UI Components

| Activity | Component ID | UI Component | Functional Description | Targeted Activity |
|----------|-------------|-------------|----------------------|-------------------|
| ViewPagerActivity | medium_viewer_toolbar (back) | Toolbar back arrow | Finishes activity | [Back] |
| ViewPagerActivity | view_pager | MyViewPager | Swipeable photo/video fragments | ViewPagerActivity (self) |
| ViewPagerActivity | fragment tap | PhotoFragment / VideoFragment | Toggles fullscreen mode | ViewPagerActivity (self) |
| ViewPagerActivity | menu_set_as | MenuItem | Opens system "Set as" chooser | N/A (external) |
| ViewPagerActivity | menu_slideshow | MenuItem | Opens SlideshowDialog, starts slideshow | ViewPagerActivity (self) |
| ViewPagerActivity | menu_copy_to | MenuItem | Opens PickDirectoryDialog, copies file | ViewPagerActivity (self) |
| ViewPagerActivity | menu_move_to | MenuItem | Opens PickDirectoryDialog, moves file | ViewPagerActivity (self) |
| ViewPagerActivity | menu_open_with | MenuItem | Opens file with external app chooser | N/A (external) |
| ViewPagerActivity | menu_hide | MenuItem | Hides file (dot-prefix rename) | ViewPagerActivity (self) |
| ViewPagerActivity | menu_unhide | MenuItem | Unhides file (remove dot prefix) | ViewPagerActivity (self) |
| ViewPagerActivity | menu_share | MenuItem | Shares via system share sheet | N/A (external) |
| ViewPagerActivity | menu_delete | MenuItem | Deletes file; finishes if none remain | ViewPagerActivity (self) / [Back] |
| ViewPagerActivity | menu_rename | MenuItem | Opens RenameItemDialog | ViewPagerActivity (self) |
| ViewPagerActivity | menu_print | MenuItem | Sends to Android print service | N/A (external) |
| ViewPagerActivity | menu_edit | MenuItem | Opens external image editor | N/A (external) |
| ViewPagerActivity | menu_properties | MenuItem | Shows PropertiesDialog | ViewPagerActivity (self) |
| ViewPagerActivity | menu_show_on_map | MenuItem | Opens map app with GPS coords | N/A (external) |
| ViewPagerActivity | menu_rotate_right | Submenu MenuItem | Rotates image 90 degrees CW | ViewPagerActivity (self) |
| ViewPagerActivity | menu_rotate_left | Submenu MenuItem | Rotates image 90 degrees CCW | ViewPagerActivity (self) |
| ViewPagerActivity | menu_rotate_one_eighty | Submenu MenuItem | Rotates image 180 degrees | ViewPagerActivity (self) |
| ViewPagerActivity | menu_add_to_favorites | MenuItem | Adds to favorites database | ViewPagerActivity (self) |
| ViewPagerActivity | menu_remove_from_favorites | MenuItem | Removes from favorites database | ViewPagerActivity (self) |
| ViewPagerActivity | menu_restore_file | MenuItem | Restores file from recycle bin | ViewPagerActivity (self) |
| ViewPagerActivity | menu_force_portrait | Submenu MenuItem | Locks to portrait orientation | ViewPagerActivity (self) |
| ViewPagerActivity | menu_force_landscape | Submenu MenuItem | Locks to landscape orientation | ViewPagerActivity (self) |
| ViewPagerActivity | menu_default_orientation | Submenu MenuItem | Unlocks orientation | ViewPagerActivity (self) |
| ViewPagerActivity | menu_save_as | MenuItem | Opens SaveAsDialog | ViewPagerActivity (self) |
| ViewPagerActivity | menu_create_shortcut | MenuItem | Creates homescreen shortcut | ViewPagerActivity (via shortcut) |
| ViewPagerActivity | menu_resize | MenuItem | Opens ResizeWithPathDialog | ViewPagerActivity (self) |
| ViewPagerActivity | menu_settings | MenuItem | Opens settings | SettingsActivity |
| ViewPagerActivity | bottom_favorite | ImageView | Toggles favorite status | ViewPagerActivity (self) |
| ViewPagerActivity | bottom_edit | ImageView | Opens external image editor | N/A (external) |
| ViewPagerActivity | bottom_share | ImageView | Shares via share sheet | N/A (external) |
| ViewPagerActivity | bottom_delete | ImageView | Deletes current file | ViewPagerActivity (self) / [Back] |
| ViewPagerActivity | bottom_rotate | ImageView | Rotates image 90 degrees CW | ViewPagerActivity (self) |
| ViewPagerActivity | bottom_properties | ImageView | Shows PropertiesDialog | ViewPagerActivity (self) |
| ViewPagerActivity | bottom_change_orientation | ImageView | Cycles orientation lock | ViewPagerActivity (self) |
| ViewPagerActivity | bottom_slideshow | ImageView | Opens SlideshowDialog, starts slideshow | ViewPagerActivity (self) |
| ViewPagerActivity | bottom_show_on_map | ImageView | Opens map app | N/A (external) |
| ViewPagerActivity | bottom_toggle_file_visibility | ImageView | Toggles file visibility | ViewPagerActivity (self) |
| ViewPagerActivity | bottom_rename | ImageView | Opens RenameItemDialog | ViewPagerActivity (self) |
| ViewPagerActivity | bottom_set_as | ImageView | Opens "Set as" chooser | N/A (external) |
| ViewPagerActivity | bottom_copy | ImageView | Opens PickDirectoryDialog, copies | ViewPagerActivity (self) |
| ViewPagerActivity | bottom_move | ImageView | Opens PickDirectoryDialog, moves | ViewPagerActivity (self) |
| ViewPagerActivity | bottom_resize | ImageView | Opens ResizeWithPathDialog | ViewPagerActivity (self) |
| ViewPagerActivity | launchViewVideoIntent() | FragmentListener callback | Launches external video player | N/A (external) |

---

## SettingsActivity

### Functional Description

SettingsActivity is the central configuration screen. It extends `SimpleActivity` and contains a scrollable list of categorized settings organized into sections: Color Customization, General Settings, Videos, Thumbnails, Scrolling, Fullscreen Media, Deep Zoomable Images, Extended Details, Security, File Operations, Bottom Actions, Recycle Bin, and Migrating.

**Entry Points:**
- Launched from `MainActivity`, `MediaActivity`, or `ViewPagerActivity` via `launchSettings()` extension.

**Layout Structure (`activity_settings.xml`):**
- `CoordinatorLayout` > `MaterialToolbar` + `NestedScrollView` > `LinearLayout` (settings_holder) with labeled sections and clickable rows.

### UI Components

| Activity | Component ID | UI Component | Functional Description | Targeted Activity |
|----------|-------------|-------------|----------------------|-------------------|
| SettingsActivity | settings_toolbar | MaterialToolbar (Arrow) | Back navigation | [Back] |
| SettingsActivity | settings_color_customization_holder | ConstraintLayout | Opens color/theme customization | CustomizationActivity (commons) |
| SettingsActivity | settings_use_english_holder | RelativeLayout (checkbox) | Toggles English language, restarts app | SettingsActivity (self) |
| SettingsActivity | settings_language_holder | RelativeLayout | Opens system language settings (Android 13+) | N/A (external) |
| SettingsActivity | settings_change_date_time_format_holder | RelativeLayout | Opens date/time format dialog | SettingsActivity (self) |
| SettingsActivity | settings_file_loading_priority_holder | RelativeLayout | Opens file loading priority radio dialog | SettingsActivity (self) |
| SettingsActivity | settings_manage_included_folders_holder | RelativeLayout | Navigates to included folders management | IncludedFoldersActivity |
| SettingsActivity | settings_manage_excluded_folders_holder | RelativeLayout | Navigates to excluded folders (password-guarded) | ExcludedFoldersActivity |
| SettingsActivity | settings_manage_hidden_folders_holder | RelativeLayout | Navigates to hidden folders (pre-Q, password-guarded) | HiddenFoldersActivity |
| SettingsActivity | settings_show_hidden_items_holder | RelativeLayout (checkbox) | Toggles hidden media visibility | SettingsActivity (self) |
| SettingsActivity | settings_search_all_files_holder | RelativeLayout (checkbox) | Toggles search-all-files-by-default | SettingsActivity (self) |
| SettingsActivity | settings_autoplay_videos_holder | RelativeLayout (checkbox) | Toggles video autoplay | SettingsActivity (self) |
| SettingsActivity | settings_remember_last_video_position_holder | RelativeLayout (checkbox) | Toggles remembering video position | SettingsActivity (self) |
| SettingsActivity | settings_loop_videos_holder | RelativeLayout (checkbox) | Toggles video looping | SettingsActivity (self) |
| SettingsActivity | settings_open_videos_on_separate_screen_holder | RelativeLayout (checkbox) | Toggles separate video screen | SettingsActivity (self) |
| SettingsActivity | settings_allow_video_gestures_holder | RelativeLayout (checkbox) | Toggles video gestures | SettingsActivity (self) |
| SettingsActivity | settings_crop_thumbnails_holder | RelativeLayout (checkbox) | Toggles thumbnail cropping | SettingsActivity (self) |
| SettingsActivity | settings_animate_gifs_holder | RelativeLayout (checkbox) | Toggles GIF animation in thumbnails | SettingsActivity (self) |
| SettingsActivity | settings_file_thumbnail_style_holder | RelativeLayout | Opens file thumbnail style dialog | SettingsActivity (self) |
| SettingsActivity | settings_folder_thumbnail_style_holder | RelativeLayout | Opens folder thumbnail style dialog | SettingsActivity (self) |
| SettingsActivity | settings_scroll_horizontally_holder | RelativeLayout (checkbox) | Toggles horizontal scrolling | SettingsActivity (self) |
| SettingsActivity | settings_enable_pull_to_refresh_holder | RelativeLayout (checkbox) | Toggles pull-to-refresh | SettingsActivity (self) |
| SettingsActivity | settings_max_brightness_holder | RelativeLayout (checkbox) | Toggles max brightness in fullscreen | SettingsActivity (self) |
| SettingsActivity | settings_black_background_holder | RelativeLayout (checkbox) | Toggles black background | SettingsActivity (self) |
| SettingsActivity | settings_hide_system_ui_holder | RelativeLayout (checkbox) | Toggles system UI hiding | SettingsActivity (self) |
| SettingsActivity | settings_allow_instant_change_holder | RelativeLayout (checkbox) | Toggles instant media change | SettingsActivity (self) |
| SettingsActivity | settings_allow_photo_gestures_holder | RelativeLayout (checkbox) | Toggles photo gestures | SettingsActivity (self) |
| SettingsActivity | settings_allow_down_gesture_holder | RelativeLayout (checkbox) | Toggles swipe-down-to-close | SettingsActivity (self) |
| SettingsActivity | settings_show_notch_holder | RelativeLayout (checkbox) | Toggles notch rendering (Pie+) | SettingsActivity (self) |
| SettingsActivity | settings_screen_rotation_holder | RelativeLayout | Opens screen rotation radio dialog | SettingsActivity (self) |
| SettingsActivity | settings_allow_zooming_images_holder | RelativeLayout (checkbox) | Toggles deep zoom for images | SettingsActivity (self) |
| SettingsActivity | settings_allow_rotating_with_gestures_holder | RelativeLayout (checkbox) | Toggles rotate-with-gestures | SettingsActivity (self) |
| SettingsActivity | settings_show_highest_quality_holder | RelativeLayout (checkbox) | Toggles highest quality images | SettingsActivity (self) |
| SettingsActivity | settings_allow_one_to_one_zoom_holder | RelativeLayout (checkbox) | Toggles 1:1 pixel zoom | SettingsActivity (self) |
| SettingsActivity | settings_show_extended_details_holder | RelativeLayout (checkbox) | Toggles extended details overlay | SettingsActivity (self) |
| SettingsActivity | settings_hide_extended_details_holder | RelativeLayout (checkbox) | Toggles auto-hiding extended details | SettingsActivity (self) |
| SettingsActivity | settings_manage_extended_details_holder | RelativeLayout | Opens ManageExtendedDetailsDialog | SettingsActivity (self) |
| SettingsActivity | settings_app_password_protection_holder | RelativeLayout (checkbox) | Toggles app password protection | SettingsActivity (self) |
| SettingsActivity | settings_hidden_item_password_protection_holder | RelativeLayout (checkbox) | Toggles hidden items password | SettingsActivity (self) |
| SettingsActivity | settings_excluded_item_password_protection_holder | RelativeLayout (checkbox) | Toggles excluded folders password | SettingsActivity (self) |
| SettingsActivity | settings_file_deletion_password_protection_holder | RelativeLayout (checkbox) | Toggles deletion password | SettingsActivity (self) |
| SettingsActivity | settings_delete_empty_folders_holder | RelativeLayout (checkbox) | Toggles auto-delete empty folders | SettingsActivity (self) |
| SettingsActivity | settings_keep_last_modified_holder | RelativeLayout (checkbox) | Toggles preserving timestamps | SettingsActivity (self) |
| SettingsActivity | settings_skip_delete_confirmation_holder | RelativeLayout (checkbox) | Toggles skip delete confirmation | SettingsActivity (self) |
| SettingsActivity | settings_bottom_actions_checkbox_holder | RelativeLayout (checkbox) | Toggles bottom action bar | SettingsActivity (self) |
| SettingsActivity | settings_manage_bottom_actions_holder | RelativeLayout | Opens ManageBottomActionsDialog | SettingsActivity (self) |
| SettingsActivity | settings_use_recycle_bin_holder | RelativeLayout (checkbox) | Toggles recycle bin usage | SettingsActivity (self) |
| SettingsActivity | settings_show_recycle_bin_holder | RelativeLayout (checkbox) | Toggles recycle bin in folder list | SettingsActivity (self) |
| SettingsActivity | settings_show_recycle_bin_last_holder | RelativeLayout (checkbox) | Toggles recycle bin shown last | SettingsActivity (self) |
| SettingsActivity | settings_empty_recycle_bin_holder | RelativeLayout | Empties recycle bin after confirmation | SettingsActivity (self) |
| SettingsActivity | settings_clear_cache_holder | RelativeLayout | Clears app image cache | SettingsActivity (self) |
| SettingsActivity | settings_export_favorites_holder | RelativeLayout | Exports favorites to file | N/A (external, system file picker) |
| SettingsActivity | settings_import_favorites_holder | RelativeLayout | Imports favorites from file | N/A (external, system file picker) |
| SettingsActivity | settings_export_holder | RelativeLayout | Exports all settings to file | N/A (external, system file picker) |
| SettingsActivity | settings_import_holder | RelativeLayout | Imports settings from file | N/A (external, system file picker) |

---

## IncludedFoldersActivity

### Functional Description

Manages the list of folders explicitly included in the gallery scan. Uses shared `activity_manage_folders.xml` layout with a toolbar and RecyclerView.

**Entry Points:** Launched from `SettingsActivity` ("Manage included folders"), requires all-files-access on Android R+.

**Key Behaviors:** Toolbar "Add folder" opens FilePickerDialog. Items can be removed via CAB or per-row overflow popup.

### UI Components

| Activity | Component ID | UI Component | Functional Description | Targeted Activity |
|----------|-------------|-------------|----------------------|-------------------|
| IncludedFoldersActivity | manage_folders_toolbar | MaterialToolbar (Arrow) | Back navigation | [Back] |
| IncludedFoldersActivity | add_folder | MenuItem | Opens FilePickerDialog to add folder | IncludedFoldersActivity (self) |
| IncludedFoldersActivity | overflow_menu_icon (adapter) | ImageView (per-row) | Opens popup with "Remove" option | IncludedFoldersActivity (self) |
| IncludedFoldersActivity | cab_remove | CAB MenuItem | Removes selected folders from included list | IncludedFoldersActivity (self) |

---

## ExcludedFoldersActivity

### Functional Description

Manages the list of folders excluded from the gallery scan. Uses shared `activity_manage_folders.xml` layout.

**Entry Points:** Launched from `SettingsActivity` ("Manage excluded folders"), guarded by `handleExcludedFolderPasswordProtection`.

**Key Behaviors:** Toolbar "Add folder" opens FilePickerDialog. Items can be removed via CAB or per-row overflow popup.

### UI Components

| Activity | Component ID | UI Component | Functional Description | Targeted Activity |
|----------|-------------|-------------|----------------------|-------------------|
| ExcludedFoldersActivity | manage_folders_toolbar | MaterialToolbar (Arrow) | Back navigation | [Back] |
| ExcludedFoldersActivity | add_folder | MenuItem | Opens FilePickerDialog to add folder | ExcludedFoldersActivity (self) |
| ExcludedFoldersActivity | overflow_menu_icon (adapter) | ImageView (per-row) | Opens popup with "Remove" option | ExcludedFoldersActivity (self) |
| ExcludedFoldersActivity | cab_remove | CAB MenuItem | Removes selected folders from excluded list | ExcludedFoldersActivity (self) |

---

## HiddenFoldersActivity

### Functional Description

Manages folders hidden via `.nomedia` files. Uses shared `activity_manage_folders.xml` layout with `ManageHiddenFoldersAdapter`.

**Entry Points:** Launched from `SettingsActivity` ("Manage hidden folders"), only visible on pre-Android Q, password-guarded.

**Key Behaviors:** Scans for directories with `.nomedia` files. "Add folder" creates `.nomedia` in selected directory. CAB provides "Unhide" action (removes `.nomedia`).

### UI Components

| Activity | Component ID | UI Component | Functional Description | Targeted Activity |
|----------|-------------|-------------|----------------------|-------------------|
| HiddenFoldersActivity | manage_folders_toolbar | MaterialToolbar (Arrow) | Back navigation | [Back] |
| HiddenFoldersActivity | add_folder | MenuItem | Opens FilePickerDialog to hide a folder | HiddenFoldersActivity (self) |
| HiddenFoldersActivity | cab_unhide | CAB MenuItem | Unhides selected folders (removes .nomedia) | HiddenFoldersActivity (self) |

---

## EditActivity

### Functional Description

A full-featured image editor providing photo filtering, cropping/rotating/resizing, and freehand drawing capabilities.

**Entry Points:**
- Launched via `Intent` with image URI data. Extra `REAL_FILE_PATH` can override the URI path.
- Crop-intent mode when extras contain `crop=true`.
- Can receive `MediaStore.EXTRA_OUTPUT` for custom save destination.
- Called from within the app via `openEditor()` / `openEditorIntent()` extensions.

**Key Behaviors:**
- Three editing modes: Filter, Crop/Rotate, Draw (mutually exclusive).
- Filter mode: horizontal RecyclerView of filter thumbnails with real-time preview.
- Crop/Rotate mode: CropImageView with guidelines, rotate, resize, flip, aspect ratio controls.
- Draw mode: EditorDrawCanvas with color picker, brush width seekbar, undo.
- Toolbar: Save As, Edit With (external editor), Share.
- EXIF data preserved on Nougat+.

**Layout Structure (`activity_edit.xml`):**
- `CoordinatorLayout` > `AppBarLayout`/`MaterialToolbar` + `RelativeLayout` body with three content views (ImageView, CropImageView, EditorDrawCanvas) + stacked bottom action bars.

### UI Components

| Activity | Component ID | UI Component | Functional Description | Targeted Activity |
|----------|-------------|-------------|----------------------|-------------------|
| EditActivity | save_as | MenuItem | Saves edited image via SaveAsDialog or directly for crop intents | EditActivity (self) |
| EditActivity | edit | MenuItem | Opens image in external editor app | N/A (external) |
| EditActivity | share | MenuItem | Shares edited bitmap | N/A (external) |
| EditActivity | bottom_primary_filter | ImageView | Toggles Filter mode | EditActivity (self) |
| EditActivity | bottom_primary_crop_rotate | ImageView | Toggles Crop/Rotate mode | EditActivity (self) |
| EditActivity | bottom_primary_draw | ImageView | Toggles Draw mode | EditActivity (self) |
| EditActivity | bottom_rotate | ImageView | Rotates crop view 90 degrees CW | EditActivity (self) |
| EditActivity | bottom_resize | ImageView | Opens ResizeDialog | EditActivity (self) |
| EditActivity | bottom_aspect_ratio | ImageView | Toggles aspect ratio sub-bar | EditActivity (self) |
| EditActivity | bottom_flip_horizontally | ImageView | Flips image horizontally | EditActivity (self) |
| EditActivity | bottom_flip_vertically | ImageView | Flips image vertically | EditActivity (self) |
| EditActivity | bottom_aspect_ratio_free | TextView | Sets free aspect ratio | EditActivity (self) |
| EditActivity | bottom_aspect_ratio_one_one | TextView | Sets 1:1 aspect ratio | EditActivity (self) |
| EditActivity | bottom_aspect_ratio_four_three | TextView | Sets 4:3 aspect ratio | EditActivity (self) |
| EditActivity | bottom_aspect_ratio_sixteen_nine | TextView | Sets 16:9 aspect ratio | EditActivity (self) |
| EditActivity | bottom_aspect_ratio_other | TextView | Opens OtherAspectRatioDialog for custom ratio | EditActivity (self) |
| EditActivity | bottom_draw_color_clickable | ImageView | Opens ColorPickerDialog for brush color | EditActivity (self) |
| EditActivity | bottom_draw_width | MySeekBar | Adjusts brush size | EditActivity (self) |
| EditActivity | bottom_draw_undo | ImageView | Undoes last drawing stroke | EditActivity (self) |
| EditActivity | bottom_actions_filter_list | MyRecyclerView | Horizontal filter thumbnails; tapping applies filter | EditActivity (self) |

---

## SetWallpaperActivity

### Functional Description

Allows the user to crop and set an image as the device wallpaper (home screen, lock screen, or both).

**Entry Points:**
- Launched via `Intent` with image URI data.
- If no image data, starts `MainActivity` with `ACTION_PICK` to let user select one first.

**Key Behaviors:**
- CropImageView with wallpaper-dimension aspect ratio.
- Bottom bar: cycle aspect ratio (portrait/landscape/square) and rotate.
- On Nougat+, RadioGroupDialog for home/lock/both screen choice.
- Scales cropped bitmap and sets via WallpaperManager.

### UI Components

| Activity | Component ID | UI Component | Functional Description | Targeted Activity |
|----------|-------------|-------------|----------------------|-------------------|
| SetWallpaperActivity | save | MenuItem | Initiates wallpaper setting (Nougat+: home/lock/both dialog) | SetWallpaperActivity (self) |
| SetWallpaperActivity | allow_changing_aspect_ratio | MenuItem | Clears fixed aspect ratio for free cropping | SetWallpaperActivity (self) |
| SetWallpaperActivity | bottom_set_wallpaper_aspect_ratio | ImageView | Cycles through portrait/landscape/square ratios | SetWallpaperActivity (self) |
| SetWallpaperActivity | bottom_set_wallpaper_rotate | ImageView | Rotates crop view 90 degrees CW | SetWallpaperActivity (self) |
| SetWallpaperActivity | (implicit redirect) | Programmatic | When intent.data is null, launches MainActivity for image picking | MainActivity |

---

## WidgetConfigureActivity

### Functional Description

Configuration screen for the gallery home screen widget. Allows selecting a folder, customizing background/text colors, and folder name display.

**Entry Points:**
- Launched by the Android system when user adds a Simple Gallery widget.

**Key Behaviors:**
- Folder picker via `PickDirectoryDialog`.
- Color pickers for background and text.
- Background transparency seekbar.
- "Show folder name" checkbox.
- Save button persists config and finishes with RESULT_OK.

### UI Components

| Activity | Component ID | UI Component | Functional Description | Targeted Activity |
|----------|-------------|-------------|----------------------|-------------------|
| WidgetConfigureActivity | config_save | Button | Saves widget config, broadcasts update, finishes | [Back] |
| WidgetConfigureActivity | config_bg_color | ImageView | Opens ColorPickerDialog for background color | WidgetConfigureActivity (self) |
| WidgetConfigureActivity | config_text_color | ImageView | Opens ColorPickerDialog for text color | WidgetConfigureActivity (self) |
| WidgetConfigureActivity | folder_picker_value | MyTextView | Opens PickDirectoryDialog for folder selection | WidgetConfigureActivity (self) |
| WidgetConfigureActivity | config_image_holder | RelativeLayout | Opens PickDirectoryDialog for folder selection | WidgetConfigureActivity (self) |
| WidgetConfigureActivity | folder_picker_show_folder_name_holder | RelativeLayout | Toggles folder name display on widget | WidgetConfigureActivity (self) |
| WidgetConfigureActivity | config_bg_seekbar | MySeekBar | Controls background transparency | WidgetConfigureActivity (self) |

---

## PhotoVideoActivity

### Functional Description

Primary entry point for viewing individual photos/videos received through external `ACTION_VIEW` intents. It hosts a `PhotoFragment` or `VideoFragment` and may redirect to specialized viewers.

**Entry Points:**
- External `ACTION_VIEW` intents with a media URI.

**Key Behaviors:**
- Redirects to `ViewPagerActivity` when a real file path is available.
- Redirects to `VideoPlayerActivity` or `PanoramaVideoActivity` for videos with separate-screen enabled.
- Falls back to hosting a fragment when no redirect is possible.
- Supports configurable bottom action buttons and toolbar menu.

**Layout Structure (`fragment_holder.xml`):**
- `RelativeLayout` > `fragment_placeholder` (FrameLayout) + `top_shadow` + `bottom_actions` + `AppBarLayout` > `MaterialToolbar`.

### UI Components

| Activity | Component ID | UI Component | Functional Description | Targeted Activity |
|----------|-------------|-------------|----------------------|-------------------|
| PhotoVideoActivity | fragment_viewer_toolbar (back) | Toolbar back arrow | Finishes activity | [Back] |
| PhotoVideoActivity | menu_set_as | MenuItem | Opens system "Set as" chooser | N/A (external) |
| PhotoVideoActivity | menu_open_with | MenuItem | Opens file with app chooser | N/A (external) |
| PhotoVideoActivity | menu_share | MenuItem | Shares media file | N/A (external) |
| PhotoVideoActivity | menu_edit | MenuItem | Opens external image editor | N/A (external) |
| PhotoVideoActivity | menu_properties | MenuItem | Shows PropertiesDialog | PhotoVideoActivity (self) |
| PhotoVideoActivity | menu_show_on_map | MenuItem | Opens map app with GPS coords | N/A (external) |
| PhotoVideoActivity | bottom_edit | ImageView | Opens external image editor | N/A (external) |
| PhotoVideoActivity | bottom_share | ImageView | Shares media file | N/A (external) |
| PhotoVideoActivity | bottom_set_as | ImageView | Opens "Set as" chooser | N/A (external) |
| PhotoVideoActivity | bottom_show_on_map | ImageView | Opens map app | N/A (external) |
| PhotoVideoActivity | fragment_placeholder (tap) | Fragment content | Toggles fullscreen mode | PhotoVideoActivity (self) |
| PhotoVideoActivity | (redirect) sendViewPagerIntent | Programmatic | Redirects to ViewPagerActivity when file path available | ViewPagerActivity |
| PhotoVideoActivity | (redirect) launchVideoPlayer | Programmatic | Redirects for videos with separate screen | VideoPlayerActivity / PanoramaVideoActivity |
| PhotoVideoActivity | (redirect) checkIntent fallback | Programmatic | Redirects to MainActivity when intent.data is null | MainActivity |

---

## PhotoActivity

### Functional Description

A trivial subclass of `PhotoVideoActivity` that forces `mIsVideo = false`. Declared separately in the manifest to handle image-only `ACTION_VIEW` intents (`image/*` MIME types). All UI components and navigation are identical to `PhotoVideoActivity`.

### UI Components

| Activity | Component ID | UI Component | Functional Description | Targeted Activity |
|----------|-------------|-------------|----------------------|-------------------|
| PhotoActivity | (all) | (all) | Identical to PhotoVideoActivity | (same as PhotoVideoActivity) |

---

## VideoActivity

### Functional Description

A trivial subclass of `PhotoVideoActivity` that forces `mIsVideo = true`. Declared separately in the manifest to handle video-only `ACTION_VIEW` intents (`video/*` MIME types). All UI components and navigation are identical to `PhotoVideoActivity`.

### UI Components

| Activity | Component ID | UI Component | Functional Description | Targeted Activity |
|----------|-------------|-------------|----------------------|-------------------|
| VideoActivity | (all) | (all) | Identical to PhotoVideoActivity | (same as PhotoVideoActivity) |

---

## VideoPlayerActivity

### Functional Description

A dedicated full-featured video player using ExoPlayer with playback controls, seek, orientation locking, and gesture-based brightness/volume control.

**Entry Points:**
- Launched from `PhotoVideoActivity.launchVideoPlayer()` for non-panoramic videos.
- Launched from `ViewPagerActivity` for videos with separate-screen enabled.

**Key Behaviors:**
- ExoPlayer with play/pause, seekbar, skip forward/backward (10s).
- Double-tap skip, horizontal drag-to-seek.
- Brightness/volume gesture controls on left/right edges.
- Swipe-down to close. Fullscreen toggle on tap.
- Remembers last position, supports looping and autoplay.
- Previous/Next file buttons return results to calling activity.

**Layout Structure (`activity_video_player.xml`):**
- `RelativeLayout` > `video_surface_frame` (GestureFrameLayout + TextureView) + gesture controllers + gradients + `bottom_video_time_holder` + `AppBarLayout` > `MaterialToolbar`.

### UI Components

| Activity | Component ID | UI Component | Functional Description | Targeted Activity |
|----------|-------------|-------------|----------------------|-------------------|
| VideoPlayerActivity | video_toolbar (back) | Toolbar back arrow | Finishes activity | [Back] |
| VideoPlayerActivity | menu_change_orientation | MenuItem | Toggles portrait/landscape lock | VideoPlayerActivity (self) |
| VideoPlayerActivity | menu_open_with | MenuItem | Opens video with app chooser | N/A (external) |
| VideoPlayerActivity | menu_share | MenuItem | Shares video file | N/A (external) |
| VideoPlayerActivity | video_toggle_play_pause | ImageView | Toggles play/pause | VideoPlayerActivity (self) |
| VideoPlayerActivity | video_curr_time | TextView | Tapping skips backward 10s | VideoPlayerActivity (self) |
| VideoPlayerActivity | video_duration | TextView | Tapping skips forward 10s | VideoPlayerActivity (self) |
| VideoPlayerActivity | video_seekbar | SeekBar | Scrubs through timeline | VideoPlayerActivity (self) |
| VideoPlayerActivity | video_prev_file | ImageView | Returns GO_TO_PREV_ITEM result, finishes | [Back] (result to caller) |
| VideoPlayerActivity | video_next_file | ImageView | Returns GO_TO_NEXT_ITEM result, finishes | [Back] (result to caller) |
| VideoPlayerActivity | video_surface_frame | GestureFrameLayout | Tap: fullscreen toggle; double-tap: skip/play; drag: seek | VideoPlayerActivity (self) |
| VideoPlayerActivity | video_brightness_controller | MediaSideScroll | Vertical swipe: brightness; tap: fullscreen; double-tap: skip back | VideoPlayerActivity (self) |
| VideoPlayerActivity | video_volume_controller | MediaSideScroll | Vertical swipe: volume; tap: fullscreen; double-tap: skip forward | VideoPlayerActivity (self) |
| VideoPlayerActivity | (gesture) swipe-down | Touch gesture | Closes activity with transition | [Back] |

---

## PanoramaPhotoActivity

### Functional Description

Displays 360-degree panoramic/equirectangular photos using Google VR SDK's `VrPanoramaView`. No toolbar -- only floating action buttons.

**Entry Points:** Launched from `ViewPagerActivity` via `openPanoramaPhoto()` when image is detected as panoramic.

### UI Components

| Activity | Component ID | UI Component | Functional Description | Targeted Activity |
|----------|-------------|-------------|----------------------|-------------------|
| PanoramaPhotoActivity | cardboard | ImageView | Switches to Google Cardboard VR mode | PanoramaPhotoActivity (self) |
| PanoramaPhotoActivity | explore | ImageView | Toggles touch-tracking panning on/off | PanoramaPhotoActivity (self) |
| PanoramaPhotoActivity | panorama_view | VrPanoramaView | Tap toggles fullscreen mode | PanoramaPhotoActivity (self) |

---

## PanoramaVideoActivity

### Functional Description

Plays 360-degree panoramic/equirectangular videos using Google VR SDK's `VrVideoView` with basic playback controls.

**Entry Points:**
- Launched from `PhotoVideoActivity.launchVideoPlayer()` for panoramic videos.
- Launched from `ViewPagerActivity` via `openPanoramaVideo()`.

### UI Components

| Activity | Component ID | UI Component | Functional Description | Targeted Activity |
|----------|-------------|-------------|----------------------|-------------------|
| PanoramaVideoActivity | cardboard | ImageView | Switches to Google Cardboard VR mode | PanoramaVideoActivity (self) |
| PanoramaVideoActivity | explore | ImageView | Toggles touch-tracking panning on/off | PanoramaVideoActivity (self) |
| PanoramaVideoActivity | vr_video_view | VrVideoView | Tap toggles fullscreen mode | PanoramaVideoActivity (self) |
| PanoramaVideoActivity | video_toggle_play_pause | ImageView | Toggles video playback | PanoramaVideoActivity (self) |
| PanoramaVideoActivity | video_curr_time | TextView | Tapping skips backward 2% of duration | PanoramaVideoActivity (self) |
| PanoramaVideoActivity | video_duration | TextView | Tapping skips forward 2% of duration | PanoramaVideoActivity (self) |
| PanoramaVideoActivity | video_seekbar | SeekBar | Scrubs through video timeline | PanoramaVideoActivity (self) |

---

## Cross-Activity Navigation Summary

```
SplashActivity ──────────────────────> MainActivity

MainActivity ─────────────────────────> MediaActivity (folder click, show all, recycle bin)
MainActivity ─────────────────────────> SearchActivity (file search)
MainActivity ─────────────────────────> SettingsActivity (settings menu)
MainActivity ─────────────────────────> AboutActivity [commons] (about menu)
MainActivity ─────────────────────────> External Camera (open camera)
MainActivity ─────────────────────────> External Google Play (more apps)

MediaActivity ────────────────────────> ViewPagerActivity (image click, slideshow)
MediaActivity ────────────────────────> MainActivity (folder view in Show All mode)
MediaActivity ────────────────────────> SettingsActivity (settings menu)
MediaActivity ────────────────────────> AboutActivity [commons] (about menu)
MediaActivity ────────────────────────> External Camera (open camera)
MediaActivity ────────────────────────> External Apps (edit, share, open with, set as)

SearchActivity ───────────────────────> ViewPagerActivity (image click)
SearchActivity ───────────────────────> External Apps (video click, CAB actions)

ViewPagerActivity ────────────────────> SettingsActivity (settings menu)
ViewPagerActivity ────────────────────> External Apps (edit, share, set as, open with, map, print)
ViewPagerActivity ────────────────────> External Video Player (video launch)

SettingsActivity ─────────────────────> CustomizationActivity [commons] (color customization)
SettingsActivity ─────────────────────> IncludedFoldersActivity (manage included)
SettingsActivity ─────────────────────> ExcludedFoldersActivity (manage excluded)
SettingsActivity ─────────────────────> HiddenFoldersActivity (manage hidden)

PhotoVideoActivity ───────────────────> ViewPagerActivity (redirect with file path)
PhotoVideoActivity ───────────────────> VideoPlayerActivity (non-panoramic video)
PhotoVideoActivity ───────────────────> PanoramaVideoActivity (panoramic video)
PhotoVideoActivity ───────────────────> MainActivity (fallback when no data)

SetWallpaperActivity ─────────────────> MainActivity (image pick when no data)

EditActivity ─────────────────────────> External Apps (edit with, share)

WidgetConfigureActivity ──────────────> (self-contained, finishes with result)
IncludedFoldersActivity ──────────────> (self-contained, finishes back)
ExcludedFoldersActivity ──────────────> (self-contained, finishes back)
HiddenFoldersActivity ────────────────> (self-contained, finishes back)
PanoramaPhotoActivity ────────────────> (self-contained, finishes back)
PanoramaVideoActivity ────────────────> (self-contained, finishes back)
```
