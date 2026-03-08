# Android to HarmonyOS UI Conversion Guide

## 1. Page Component Mapping

### Android Components
- **Activity**: Main UI container
- **Fragment**: Reusable UI component
- **View**: Basic UI element
- **ViewGroup**: Container for views
- **TextView**: Displays text
- **EditText**: Text input field
- **Button**: Clickable button
- **ImageView**: Displays images
- **RecyclerView**: Efficient list display
- **ListView**: Traditional list display
- **GridView**: Grid layout
- **CardView**: Card-like container
- **CoordinatorLayout**: Advanced layout with child coordination
- **AppBarLayout**: App bar container
- **Toolbar**: Action bar replacement
- **DrawerLayout**: Navigation drawer
- **BottomNavigationView**: Bottom navigation
- **TabLayout**: Tabbed interface
- **Chip**: Small interactive element
- **Switch**: On/off toggle
- **Checkbox**: Checkable box
- **RadioButton**: Radio selection
- **ProgressBar**: Progress indicator
- **SeekBar**: Slider control
- **Spinner**: Dropdown selection
- **AutoCompleteTextView**: Auto-complete input
- **DatePicker**: Date selection
- **TimePicker**: Time selection
- **CalendarView**: Calendar display
- **WebView**: Web content display

### HarmonyOS Equivalents
- **Ability**: Main UI container (similar to Activity)
- **PageAbility**: Page-based ability
- **Slice**: Reusable UI component (similar to Fragment)
- **Component**: Basic UI element (similar to View)
- **ComponentContainer**: Container for components (similar to ViewGroup)
- **Text**: Displays text (similar to TextView)
- **TextInput**: Text input field (similar to EditText)
- **Button**: Clickable button
- **Image**: Displays images (similar to ImageView)
- **List**: Efficient list display (similar to RecyclerView)
- **Grid**: Grid layout (similar to GridView)
- **ListContainer**: Traditional list display (similar to ListView)
- **Card**: Card-like container (similar to CardView)
- **StackLayout**: Stack-based layout
- **DirectionalLayout**: Directional layout (similar to LinearLayout)
- **DependentLayout**: Constraint-based layout (similar to RelativeLayout)
- **PositionLayout**: Absolute position layout
- **NavigationView**: Navigation drawer (similar to DrawerLayout)
- **TabList**: Tabbed interface (similar to TabLayout)
- **ToggleButton**: On/off toggle (similar to Switch)
- **Checkbox**: Checkable box
- **RadioButton**: Radio selection
- **ProgressBar**: Progress indicator
- **Slider**: Slider control (similar to SeekBar)
- **Picker**: Selection control (similar to Spinner)
- **DatePicker**: Date selection
- **TimePicker**: Time selection
- **Web**: Web content display (similar to WebView)

### Conversion Guidelines
- Map Android Activities to HarmonyOS PageAbilities
- Map Android Fragments to HarmonyOS Slices
- Map Android Views to HarmonyOS Components
- Use HarmonyOS layout components (DirectionalLayout, DependentLayout, etc.) instead of Android layout managers
- Adjust for different naming conventions and API differences

## 2. App Layout: Navigation Bar/Sidebar

### Android Implementation
- **ActionBar/Toolbar**: Top navigation bar
- **DrawerLayout**: Side navigation drawer
- **BottomNavigationView**: Bottom navigation bar
- **TabLayout**: Tab-based navigation
- **NavigationView**: Content for navigation drawer

### HarmonyOS Implementation
- **NavigationBar**: Top navigation bar
- **NavigationView**: Side navigation drawer
- **BottomNavigationBar**: Bottom navigation bar
- **TabList**: Tab-based navigation
- **ListContainer**: Content for navigation drawer

### Conversion Guidelines
- Use HarmonyOS NavigationBar instead of Android Toolbar
- Use HarmonyOS NavigationView instead of Android DrawerLayout
- Use HarmonyOS BottomNavigationBar instead of Android BottomNavigationView
- Use HarmonyOS TabList instead of Android TabLayout
- Adjust navigation item structures to match HarmonyOS patterns

## 3. Page Routing

### Android Implementation
- **Intent**: Used to start activities and pass data
- **startActivity()**: Launches a new activity
- **startActivityForResult()**: Launches activity and receives result
- **onActivityResult()**: Handles results from activities
- **FragmentManager**: Manages fragment transactions
- **FragmentTransaction**: Performs fragment operations

### HarmonyOS Implementation
- **router**: Module for page navigation
- **router.push()**: Navigates to a new page
- **router.replace()**: Replaces current page
- **router.back()**: Returns to previous page
- **router.clear()**: Clears navigation stack
- **router.setParams()**: Passes parameters between pages
- **onBackPressed()**: Handles back button press

### Conversion Guidelines
- Replace Android Intents with HarmonyOS router module
- Adjust parameter passing to use router.setParams()
- Implement back navigation using router.back()
- Handle page results using callback mechanisms instead of onActivityResult()

## 4. Page Lifecycle Management

### Android Lifecycle (Activity)
- **onCreate()**: Called when activity is first created
- **onStart()**: Called when activity becomes visible
- **onResume()**: Called when activity becomes interactive
- **onPause()**: Called when activity loses focus
- **onStop()**: Called when activity is no longer visible
- **onDestroy()**: Called when activity is being destroyed
- **onRestart()**: Called when activity is restarted

### Android Lifecycle (Fragment)
- **onAttach()**: Called when fragment is attached to activity
- **onCreate()**: Called when fragment is created
- **onCreateView()**: Called to create fragment's UI
- **onActivityCreated()**: Called when activity's onCreate() completes
- **onStart()**: Called when fragment becomes visible
- **onResume()**: Called when fragment becomes interactive
- **onPause()**: Called when fragment loses focus
- **onStop()**: Called when fragment is no longer visible
- **onDestroyView()**: Called when fragment's UI is destroyed
- **onDestroy()**: Called when fragment is being destroyed
- **onDetach()**: Called when fragment is detached from activity

### HarmonyOS Lifecycle (PageAbility)
- **onStart()**: Called when ability is started
- **onInactive()**: Called when ability becomes inactive
- **onActive()**: Called when ability becomes active
- **onBackground()**: Called when ability moves to background
- **onStop()**: Called when ability is stopped

### HarmonyOS Lifecycle (Slice)
- **onStart()**: Called when slice is started
- **onInactive()**: Called when slice becomes inactive
- **onActive()**: Called when slice becomes active
- **onBackground()**: Called when slice moves to background
- **onStop()**: Called when slice is stopped
- **aboutToAppear()**: Called before slice becomes visible
- **aboutToDisappear()**: Called before slice becomes invisible

### Conversion Guidelines
- Map Android activity lifecycle methods to HarmonyOS PageAbility lifecycle methods
- Map Android fragment lifecycle methods to HarmonyOS Slice lifecycle methods
- Use aboutToAppear() and aboutToDisappear() for slice-specific initialization and cleanup
- Adjust timing-dependent operations to match HarmonyOS lifecycle

## 5. Layout Containers

### Android Layouts
- **LinearLayout**: Arranges views in a single direction
- **RelativeLayout**: Arranges views relative to each other
- **ConstraintLayout**: Flexible layout with constraints
- **FrameLayout**: Simple container for a single view
- **TableLayout**: Arranges views in a table
- **GridLayout**: Arranges views in a grid
- **CoordinatorLayout**: Coordinates child views
- **AppBarLayout**: Container for app bar
- **DrawerLayout**: Container with navigation drawer
- **NestedScrollView**: Scrollable container for nested scrolling
- **ScrollView**: Scrollable container

### HarmonyOS Layouts
- **DirectionalLayout**: Arranges components in a single direction (similar to LinearLayout)
- **DependentLayout**: Arranges components relative to each other (similar to RelativeLayout)
- **StackLayout**: Stacks components on top of each other (similar to FrameLayout)
- **PositionLayout**: Arranges components at absolute positions
- **ListContainer**: Container for list items
- **GridContainer**: Container for grid items
- **ScrollView**: Scrollable container

### Conversion Guidelines
- Replace LinearLayout with DirectionalLayout
- Replace RelativeLayout with DependentLayout
- Replace FrameLayout with StackLayout
- Use PositionLayout for absolute positioning
- Adjust layout parameters to match HarmonyOS syntax

## 6. Lists/Grids

### Android Implementation
- **RecyclerView**: Efficient list display with view holders
- **ListView**: Traditional list display
- **GridView**: Grid layout for items
- **Adapter**: Provides data to list/grid views
- **ViewHolder**: Caches view references in RecyclerView
- **LayoutManager**: Determines item positioning in RecyclerView

### HarmonyOS Implementation
- **List**: Efficient list display (similar to RecyclerView)
- **Grid**: Grid layout for items (similar to GridView)
- **ListContainer**: Traditional list display (similar to ListView)
- **ItemProvider**: Provides data to list/grid components
- **ComponentProvider**: Creates components for list/grid items
- **Scroller**: Handles scrolling behavior

### Conversion Guidelines
- Replace RecyclerView with List
- Replace GridView with Grid
- Replace ListView with ListContainer
- Use ItemProvider instead of Adapter
- Use ComponentProvider instead of ViewHolder
- Adjust item creation and binding to match HarmonyOS patterns

## 7. Custom Views

### Android Implementation
- **View**: Base class for custom views
- **onMeasure()**: Measures view dimensions
- **onLayout()**: Lays out view children
- **onDraw()**: Draws view content
- **invalidate()**: Triggers view redraw
- **requestLayout()**: Triggers view layout

### HarmonyOS Implementation
- **Component**: Base class for custom components
- **onMeasure()**: Measures component dimensions
- **onLayout()**: Lays out component children
- **onDraw()**: Draws component content
- **invalidate()**: Triggers component redraw
- **requestLayout()**: Triggers component layout

### Conversion Guidelines
- Extend Component instead of View for custom UI elements
- Implement similar lifecycle methods (onMeasure, onLayout, onDraw)
- Adjust drawing APIs to use HarmonyOS graphics system
- Use HarmonyOS animation APIs for custom animations

## 8. Resource Files (Icons, Text, Colors, Dimensions)

### Android Resources
- **strings.xml**: String resources
- **colors.xml**: Color resources
- **dimens.xml**: Dimension resources
- **styles.xml**: Style resources
- **themes.xml**: Theme resources
- **drawable/**: Drawable resources
- **mipmap/**: App icon resources
- **layout/**: Layout resources

### HarmonyOS Resources
- **string.json**: String resources
- **color.json**: Color resources
- **dimen.json**: Dimension resources
- **style.json**: Style resources
- **theme.json**: Theme resources
- **media/**: Image resources
- **layout/**: Layout resources

### Conversion Guidelines
- Convert XML resource files to JSON format
- Adjust resource reference syntax
- Use HarmonyOS resource management APIs
- Ensure proper resource loading and localization

## 9. Theme/Style

### Android Implementation
- **Theme**: Defines overall app appearance
- **Style**: Defines component appearance
- **attrs.xml**: Defines custom attributes
- **Theme.AppCompat**: Compatible theme for older Android versions
- **Material Components**: Material Design components

### HarmonyOS Implementation
- **Theme**: Defines overall app appearance
- **Style**: Defines component appearance
- **Attribute**: Defines custom attributes
- **Material Design**: Supported through HarmonyOS components

### Conversion Guidelines
- Convert Android themes to HarmonyOS themes
- Convert Android styles to HarmonyOS styles
- Adjust attribute references to match HarmonyOS syntax
- Use HarmonyOS theme management APIs

## 10. Click Events

### Android Implementation
- **OnClickListener**: Interface for click events
- **setOnClickListener()**: Sets click listener for views
- **onClick()**: Handles click events
- **View.OnClickListener**: Anonymous inner class for click handling

### HarmonyOS Implementation
- **ClickedListener**: Interface for click events
- **setClickedListener()**: Sets click listener for components
- **onClick()**: Handles click events
- **lambda expressions**: Concise click handling

### Conversion Guidelines
- Replace OnClickListener with ClickedListener
- Replace setOnClickListener() with setClickedListener()
- Use lambda expressions for concise event handling
- Adjust event handling code to match HarmonyOS syntax

## 11. Gesture Handling (Swipe, Long Press, Pinch, Drag)

### Android Implementation
- **GestureDetector**: Detects common gestures
- **OnGestureListener**: Interface for gesture events
- **OnLongClickListener**: Interface for long press events
- **View.OnTouchListener**: Interface for touch events
- **MotionEvent**: Represents touch events
- **ScaleGestureDetector**: Detects pinch gestures
- **GestureDetector.SimpleOnGestureListener**: Convenient gesture listener implementation

### HarmonyOS Implementation
- **GestureDetector**: Detects common gestures
- **GestureListener**: Interface for gesture events
- **LongClickedListener**: Interface for long press events
- **TouchEventListener**: Interface for touch events
- **TouchEvent**: Represents touch events
- **PinchGestureDetector**: Detects pinch gestures
- **DragEvent**: Represents drag events

### Conversion Guidelines
- Replace Android gesture detectors with HarmonyOS equivalents
- Implement HarmonyOS gesture listeners
- Adjust touch event handling to match HarmonyOS patterns
- Use HarmonyOS drag and drop APIs

## 12. Input Handling

### Android Implementation
- **EditText**: Text input field
- **InputType**: Specifies input type (text, number, password, etc.)
- **TextWatcher**: Listens for text changes
- **InputMethodManager**: Manages input method (keyboard)
- **imeOptions**: Specifies keyboard behavior
- **onKeyDown()**: Handles hardware key events
- **onKeyUp()**: Handles hardware key release events

### HarmonyOS Implementation
- **TextInput**: Text input field
- **inputType**: Specifies input type
- **TextChangedListener**: Listens for text changes
- **InputMethodController**: Manages input method
- **imeOptions**: Specifies keyboard behavior
- **onKeyEvent()**: Handles hardware key events

### Conversion Guidelines
- Replace EditText with TextInput
- Use TextChangedListener instead of TextWatcher
- Adjust input type specifications
- Use InputMethodController instead of InputMethodManager

## 13. Dialogs/Popups

### Android Implementation
- **AlertDialog**: Alert dialog with buttons
- **Dialog**: Base dialog class
- **DialogFragment**: Dialog as fragment
- **PopupWindow**: Popup window
- **Toast**: Short message display
- **Snackbar**: Snackbar message
- **BottomSheetDialog**: Bottom sheet dialog

### HarmonyOS Implementation
- **AlertDialog**: Alert dialog with buttons
- **Dialog**: Base dialog class
- **PopupDialog**: Popup dialog
- **ToastDialog**: Short message display (similar to Toast)
- **CustomDialog**: Custom dialog implementation

### Conversion Guidelines
- Replace Android dialogs with HarmonyOS equivalents
- Adjust dialog creation and configuration
- Use HarmonyOS dialog APIs
- Implement custom dialogs using HarmonyOS components

## 14. Pull-to-Refresh/Load More

### Android Implementation
- **SwipeRefreshLayout**: Pull-to-refresh container
- **RecyclerView.OnScrollListener**: Detects scroll events for load more
- **EndlessRecyclerViewScrollListener**: Custom listener for load more
- **setRefreshing()**: Controls refresh state
- **onRefresh()**: Handles refresh event

### HarmonyOS Implementation
- **RefreshComponent**: Pull-to-refresh container
- **ScrollListener**: Detects scroll events for load more
- **setRefreshing()**: Controls refresh state
- **onRefreshing()**: Handles refresh event
- **onLoadMore()**: Handles load more event

### Conversion Guidelines
- Replace SwipeRefreshLayout with RefreshComponent
- Implement scroll listeners for load more functionality
- Adjust refresh and load more event handling
- Use HarmonyOS refresh component APIs

## Code Examples

### Android to HarmonyOS Component Mapping Example

#### Android (XML Layout)
```xml
<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
    
    <TextView
        android:id="@+id/title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World"
        android:textSize="18sp"
        android:textColor="@color/primary"
        android:layout_margin="16dp"/>
    
    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Click Me"
        android:layout_margin="16dp"/>
</LinearLayout>
```

#### HarmonyOS (ETS Layout)
```ets
DirectionalLayout({
    width: '100%',
    height: '100%',
    orientation: DirectionalLayout.Orientation.VERTICAL
}) {
    Text('Hello World')
        .id('title')
        .fontSize(18)
        .fontColor($r('app.color.primary'))
        .margin(16)
    
    Button('Click Me')
        .id('button')
        .margin(16)
}
```

### Android to HarmonyOS Event Handling Example

#### Android (Java)
```java
Button button = findViewById(R.id.button);
button.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        Toast.makeText(MainActivity.this, "Button clicked", Toast.LENGTH_SHORT).show();
    }
});
```

#### HarmonyOS (TypeScript/ETS)
```ets
Button('Click Me')
    .id('button')
    .onClick(() => {
        ToastDialog.show("Button clicked");
    })
```

### Android to HarmonyOS Page Routing Example

#### Android (Java)
```java
Intent intent = new Intent(MainActivity.this, DetailActivity.class);
intent.putExtra("itemId", 123);
startActivity(intent);
```

#### HarmonyOS (TypeScript/ETS)
```ets
import router from '@system.router';

router.push({
    uri: 'pages/DetailPage',
    params: {
        itemId: 123
    }
});
```

### Android to HarmonyOS Lifecycle Example

#### Android (Java)
```java
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        // Initialize UI components
    }
    
    @Override
    protected void onResume() {
        super.onResume();
        // Resume operations
    }
    
    @Override
    protected void onPause() {
        super.onPause();
        // Pause operations
    }
}
```

#### HarmonyOS (TypeScript/ETS)
```ets
export default class MainPage extends AbilitySlice {
    onStart() {
        super.onStart();
        this.setUIContent(ResourceTable.Layout_main_page);
        // Initialize UI components
    }
    
    onActive() {
        super.onActive();
        // Resume operations
    }
    
    onInactive() {
        super.onInactive();
        // Pause operations
    }
    
    aboutToAppear() {
        // Called before slice becomes visible
    }
    
    aboutToDisappear() {
        // Called before slice becomes invisible
    }
}
```

## Best Practices for UI Conversion

1. **Understand the Architecture Differences**:
   - Android uses a view-based system with XML layouts
   - HarmonyOS uses a component-based system with ETS layouts

2. **Use HarmonyOS Native Components**:
   - Leverage HarmonyOS-specific components for better performance
   - Avoid trying to replicate Android-specific UI patterns directly

3. **Adapt to HarmonyOS Design Guidelines**:
   - Follow HarmonyOS design principles
   - Use appropriate spacing, typography, and colors

4. **Optimize for HarmonyOS Performance**:
   - Use efficient layout components
   - Implement proper lifecycle management
   - Avoid unnecessary UI redraws

5. **Test on Multiple Devices**:
   - Test on different screen sizes and orientations
   - Ensure UI elements scale appropriately

6. **Handle Platform-Specific Features**:
   - Adapt to HarmonyOS-specific features
   - Handle differences in input methods and gestures

7. **Use Resource Management Effectively**:
   - Organize resources properly
   - Use theme and style systems effectively

8. **Implement Accessibility**:
   - Ensure UI components are accessible
   - Follow accessibility guidelines for both platforms

9. **Maintain Consistency**:
   - Keep UI consistent across both platforms
   - Ensure similar user experience

10. **Document the Conversion Process**:
    - Document component mappings
    - Record any platform-specific adaptations
    - Maintain conversion guidelines for future reference
