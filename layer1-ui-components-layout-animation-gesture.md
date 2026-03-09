# Layer 1: SDK/API — UI 组件、布局、动画、手势、无障碍映射表

> Android → HarmonyOS (ArkUI) 迁移参考

---

## 一、基础 UI 组件

| Android (API/类) | HarmonyOS (API/类) | 说明 | Android 文档参考 | HarmonyOS 文档参考 |
|---|---|---|---|---|
| `TextView` | `Text` 组件 | 文本显示。ArkUI 通过 `Text('内容')` 创建，支持 `.fontSize()`、`.fontColor()`、`.fontWeight()` 链式属性 | [TextView](https://developer.android.com/reference/android/widget/TextView) | [Text](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-components-text-V5) |
| `TextView` (spannable) | `Span` / `RichText` | 富文本。ArkUI 用 `Text` 内嵌 `Span`、`ImageSpan` 子组件，或 `RichText` 渲染 HTML | [SpannableString](https://developer.android.com/reference/android/text/SpannableString) | [Span](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-components-span-V5) |
| `Button` | `Button` 组件 | 按钮。支持 `ButtonType.Capsule`/`Circle`/`Normal` 三种形态 | [Button](https://developer.android.com/reference/android/widget/Button) | [Button](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-components-button-V5) |
| `ImageButton` | `Button` + `Image` 组合 | ArkUI 中用 `Button` 包裹 `Image` 子组件实现图标按钮 | [ImageButton](https://developer.android.com/reference/android/widget/ImageButton) | 同上 |
| `FloatingActionButton` | `Button` + `ButtonType.Circle` + `.position()` | 圆形 Button + 定位实现悬浮效果 | [FAB](https://developer.android.com/reference/com/google/android/material/floatingactionbutton/FloatingActionButton) | 同上 |
| `ImageView` | `Image` 组件 | 图片显示。支持网络 URL、本地资源 `$r('app.media.xxx')`、PixelMap | [ImageView](https://developer.android.com/reference/android/widget/ImageView) | [Image](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-components-image-V5) |
| `ImageView.ScaleType` | `Image.objectFit(ImageFit.xxx)` | `CENTER_CROP`→`Cover`；`FIT_CENTER`→`Contain`；`FIT_XY`→`Fill`；`CENTER`→`None` | [ScaleType](https://developer.android.com/reference/android/widget/ImageView.ScaleType) | [ImageFit](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-appendix-enums-V5) |
| `EditText` | `TextInput` / `TextArea` | 单行用 `TextInput`，多行用 `TextArea`。通过 `.onChange()` 监听输入 | [EditText](https://developer.android.com/reference/android/widget/EditText) | [TextInput](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-components-textinput-V5) |
| `EditText` (inputType) | `TextInput.type(InputType.xxx)` | `NUMBER`→`InputType.Number`；`PASSWORD`→`InputType.Password`；`EMAIL`→`InputType.Email` | [InputType](https://developer.android.com/reference/android/text/InputType) | 同上 |
| `SearchView` | `Search` 组件 | 搜索栏。支持 `.onSubmit()`、`.onChange()` | [SearchView](https://developer.android.com/reference/android/widget/SearchView) | [Search](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-components-search-V5) |
| `CheckBox` | `Checkbox` 组件 | 复选框 | [CheckBox](https://developer.android.com/reference/android/widget/CheckBox) | [Checkbox](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-components-checkbox-V5) |
| `RadioButton` / `RadioGroup` | `Radio` 组件 | 单选按钮。通过 `group` 参数分组 | [RadioButton](https://developer.android.com/reference/android/widget/RadioButton) | [Radio](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-components-radio-V5) |
| `Switch` / `ToggleButton` | `Toggle` 组件 | 开关。支持 `ToggleType.Switch`/`Checkbox`/`Button` | [Switch](https://developer.android.com/reference/android/widget/Switch) | [Toggle](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-components-toggle-V5) |
| `ProgressBar` | `Progress` 组件 | 进度条。支持 `Linear`/`Ring`/`Eclipse`/`ScaleRing`/`Capsule` 样式 | [ProgressBar](https://developer.android.com/reference/android/widget/ProgressBar) | [Progress](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-components-progress-V5) |
| `SeekBar` | `Slider` 组件 | 滑动条 | [SeekBar](https://developer.android.com/reference/android/widget/SeekBar) | [Slider](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-components-slider-V5) |
| `RatingBar` | `Rating` 组件 | 评分条 | [RatingBar](https://developer.android.com/reference/android/widget/RatingBar) | [Rating](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-components-rating-V5) |
| `Spinner` | `Select` 组件 | 下拉选择 | [Spinner](https://developer.android.com/reference/android/widget/Spinner) | [Select](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-components-select-V5) |
| `DatePicker` / `TimePicker` | `DatePicker` / `TimePicker` | 日期/时间选择器。还有 `DatePickerDialog`/`TimePickerDialog` 弹窗式 | [DatePicker](https://developer.android.com/reference/android/widget/DatePicker) | [DatePicker](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-components-datepicker-V5) |
| `CalendarView` | `CalendarPicker` 组件 | 日历视图 | [CalendarView](https://developer.android.com/reference/android/widget/CalendarView) | [CalendarPicker](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-components-calendarpicker-V5) |
| `WebView` | `Web` 组件 | 网页加载。使用 `WebviewController` 控制 | [WebView](https://developer.android.com/reference/android/webkit/WebView) | [Web](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-components-web-V5) |
| `VideoView` | `Video` 组件 | 视频播放。支持 `.autoPlay()`、`.controls()`、`.loop()` | [VideoView](https://developer.android.com/reference/android/widget/VideoView) | [Video](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-media-components-video-V5) |
| `Divider` (Material) | `Divider` 组件 | 分隔线 | Material Divider | [Divider](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-components-divider-V5) |
| `Badge` (Material) | `Badge` 组件 | 角标/徽章 | Material Badge | [Badge](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-container-badge-V5) |
| `Marquee` (ellipsize) | `Marquee` 组件 | 跑马灯。ArkUI 提供独立 Marquee 组件 | TextView marquee 属性 | [Marquee](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-components-marquee-V5) |

---

## 二、列表与可滚动组件

| Android (API/类) | HarmonyOS (API/类) | 说明 | Android 文档参考 | HarmonyOS 文档参考 |
|---|---|---|---|---|
| `RecyclerView` | `List` 组件 | 高性能列表。使用 `List` + `ForEach`/`LazyForEach` 渲染 `ListItem` | [RecyclerView](https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView) | [List](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-container-list-V5) |
| `RecyclerView.Adapter` | `LazyForEach` + `IDataSource` | 数据适配器。实现 `IDataSource` 接口配合 `LazyForEach` 懒加载 | [Adapter](https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView.Adapter) | [LazyForEach](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/arkts-rendering-control-lazyforeach-V5) |
| `RecyclerView.ViewHolder` | `@Builder` / `@Component` | 声明式 UI 无需 ViewHolder 模式 | [ViewHolder](https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView.ViewHolder) | 无需 |
| `LinearLayoutManager` | `List` 默认布局 | 纵向列表。`.listDirection(Axis.Horizontal)` 切换横向 | [LinearLayoutManager](https://developer.android.com/reference/androidx/recyclerview/widget/LinearLayoutManager) | [List](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-container-list-V5) |
| `GridLayoutManager` | `Grid` 组件 | 网格布局。`Grid` + `GridItem`，通过 `.columnsTemplate()` 设置列 | [GridLayoutManager](https://developer.android.com/reference/androidx/recyclerview/widget/GridLayoutManager) | [Grid](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-container-grid-V5) |
| `StaggeredGridLayoutManager` | `WaterFlow` 组件 | 瀑布流布局 | [StaggeredGrid](https://developer.android.com/reference/androidx/recyclerview/widget/StaggeredGridLayoutManager) | [WaterFlow](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-container-waterflow-V5) |
| `ItemTouchHelper` (swipe) | `ListItem.swipeAction()` | 列表项滑动操作菜单 | [ItemTouchHelper](https://developer.android.com/reference/androidx/recyclerview/widget/ItemTouchHelper) | [ListItem swipeAction](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-container-listitem-V5) |
| `DiffUtil` | `IDataSource` 的 `DataChangeListener` | 差异通知。`onDataAdd`/`onDataDelete`/`onDataChange` | [DiffUtil](https://developer.android.com/reference/androidx/recyclerview/widget/DiffUtil) | [IDataSource](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/arkts-rendering-control-lazyforeach-V5) |
| `ScrollView` | `Scroll` 组件 | 滚动容器。通过 `Scroller` 控制器编程式滚动 | [ScrollView](https://developer.android.com/reference/android/widget/ScrollView) | [Scroll](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-container-scroll-V5) |
| `HorizontalScrollView` | `Scroll` + `ScrollDirection.Horizontal` | 横向滚动 | [HorizontalScrollView](https://developer.android.com/reference/android/widget/HorizontalScrollView) | 同上 |
| `NestedScrollView` | `Scroll` + `.nestedScroll()` | 嵌套滚动策略配置 | [NestedScrollView](https://developer.android.com/reference/androidx/core/widget/NestedScrollView) | 同上 |
| `ViewPager` / `ViewPager2` | `Swiper` 组件 | 页面滑动切换。支持 `.autoPlay()`、`.indicator()`、`.loop()` | [ViewPager2](https://developer.android.com/reference/androidx/viewpager2/widget/ViewPager2) | [Swiper](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-container-swiper-V5) |
| `PagerAdapter` / `FragmentStateAdapter` | `Swiper` + `ForEach`/`LazyForEach` | 无需 Adapter 模式，直接循环渲染子页面 | [FragmentStateAdapter](https://developer.android.com/reference/androidx/viewpager2/adapter/FragmentStateAdapter) | 同上 |
| `SwipeRefreshLayout` | `Refresh` 组件 | 下拉刷新。通过 `.onRefreshing()` 回调处理刷新 | [SwipeRefreshLayout](https://developer.android.com/reference/androidx/swiperefreshlayout/widget/SwipeRefreshLayout) | [Refresh](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-container-refresh-V5) |

---

## 三、布局容器

| Android (API/类) | HarmonyOS (API/类) | 说明 | Android 文档参考 | HarmonyOS 文档参考 |
|---|---|---|---|---|
| `LinearLayout` (vertical) | `Column` 组件 | 纵向线性布局。支持 `.justifyContent()`、`.alignItems()` | [LinearLayout](https://developer.android.com/reference/android/widget/LinearLayout) | [Column](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-container-column-V5) |
| `LinearLayout` (horizontal) | `Row` 组件 | 横向线性布局 | 同上 | [Row](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-container-row-V5) |
| `FrameLayout` | `Stack` 组件 | 层叠布局。支持 `.alignContent()` | [FrameLayout](https://developer.android.com/reference/android/widget/FrameLayout) | [Stack](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-container-stack-V5) |
| `RelativeLayout` | `RelativeContainer` 组件 | 相对布局。通过 `.alignRules()` 设置定位规则 | [RelativeLayout](https://developer.android.com/reference/android/widget/RelativeLayout) | [RelativeContainer](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-container-relativecontainer-V5) |
| `ConstraintLayout` | `RelativeContainer` / 自定义布局 | 约束布局。复杂场景可用自定义 `onMeasureSize`/`onPlaceChildren` | [ConstraintLayout](https://developer.android.com/reference/androidx/constraintlayout/widget/ConstraintLayout) | 同上 |
| `FlexboxLayout` | `Flex` 组件 | 弹性布局。支持 `FlexDirection`/`FlexWrap`/`FlexAlign` 标准 Flexbox | [FlexboxLayout](https://github.com/nicklin/flexbox-layout) | [Flex](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-container-flex-V5) |
| `GridLayout` | `Grid` 组件 | 网格布局。通过 `.columnsTemplate('1fr 1fr 1fr')` 定义列 | [GridLayout](https://developer.android.com/reference/android/widget/GridLayout) | [Grid](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-container-grid-V5) |
| `CoordinatorLayout` | 组合 `Scroll` + `Column` + 动画 | 无直接对应。需组合模拟 AppBar 折叠效果 | [CoordinatorLayout](https://developer.android.com/reference/androidx/coordinatorlayout/widget/CoordinatorLayout) | 组合实现 |
| `AppBarLayout` / `CollapsingToolbarLayout` | `Navigation` 标题栏 + `titleMode` | 折叠工具栏。`NavigationTitleMode.Full`→`.Mini` 自动切换 | [CollapsingToolbarLayout](https://developer.android.com/reference/com/google/android/material/appbar/CollapsingToolbarLayout) | [Navigation](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-components-navigation-V5) |
| `layout_weight` | `.layoutWeight()` / `.flexGrow()` | 权重分配 | [layout_weight](https://developer.android.com/reference/android/widget/LinearLayout.LayoutParams) | [layoutWeight](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-universal-attributes-size-V5) |
| `padding` / `margin` | `.padding()` / `.margin()` | 支持统一或分别指定 `{ top, right, bottom, left }` | XML 布局属性 | [通用属性-尺寸](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-universal-attributes-size-V5) |
| `View.GONE` / `View.INVISIBLE` | `Visibility.None` / `Visibility.Hidden` | `None`=不占位（GONE）；`Hidden`=占位不显示（INVISIBLE） | [setVisibility](https://developer.android.com/reference/android/view/View#setVisibility(int)) | [visibility](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-universal-attributes-visibility-V5) |

---

## 四、导航与页面结构

| Android (API/类) | HarmonyOS (API/类) | 说明 | Android 文档参考 | HarmonyOS 文档参考 |
|---|---|---|---|---|
| `Toolbar` / `ActionBar` | `Navigation` 标题栏 | 通过 `.title()` 和 `.menus()` 配置 | [Toolbar](https://developer.android.com/reference/android/widget/Toolbar) | [Navigation](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-components-navigation-V5) |
| `TabLayout` + `ViewPager` | `Tabs` 组件 | 标签页。`Tabs` 整合标签栏和内容页切换 | [TabLayout](https://developer.android.com/reference/com/google/android/material/tabs/TabLayout) | [Tabs](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-container-tabs-V5) |
| `BottomNavigationView` | `Tabs` + `BarPosition.End` | 底部导航栏 | [BottomNavigationView](https://developer.android.com/reference/com/google/android/material/bottomnavigation/BottomNavigationView) | 同上 |
| `DrawerLayout` / `NavigationView` | `SideBarContainer` 组件 | 侧边栏/抽屉 | [DrawerLayout](https://developer.android.com/reference/androidx/drawerlayout/widget/DrawerLayout) | [SideBarContainer](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-container-sidebarcontainer-V5) |
| `Fragment` | `NavDestination` / `@Component` | ArkUI 通过 Navigation + NavDestination 实现子页面路由 | [Fragment](https://developer.android.com/reference/androidx/fragment/app/Fragment) | [NavDestination](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-components-navdestination-V5) |

---

## 五、弹窗与提示组件

| Android (API/类) | HarmonyOS (API/类) | 说明 | Android 文档参考 | HarmonyOS 文档参考 |
|---|---|---|---|---|
| `AlertDialog` | `AlertDialog.show()` | 警告对话框 | [AlertDialog](https://developer.android.com/reference/android/app/AlertDialog) | [AlertDialog](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-methods-alert-dialog-box-V5) |
| `Dialog` / `DialogFragment` | `@CustomDialog` 装饰器 | 自定义对话框。通过 `CustomDialogController` 控制显隐 | [DialogFragment](https://developer.android.com/reference/androidx/fragment/app/DialogFragment) | [CustomDialog](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-methods-custom-dialog-box-V5) |
| `BottomSheetDialog` | `ActionSheet.show()` / `.bindSheet()` | 底部弹出面板 | [BottomSheetDialog](https://developer.android.com/reference/com/google/android/material/bottomsheet/BottomSheetDialog) | [ActionSheet](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-methods-action-sheet-V5) |
| `Toast.makeText().show()` | `promptAction.showToast()` | 轻提示 | [Toast](https://developer.android.com/reference/android/widget/Toast) | [promptAction](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/js-apis-promptaction-V5) |
| `Snackbar` | `promptAction.showToast()` + 自定义 | 无直接对应，需自定义实现带操作按钮的效果 | [Snackbar](https://developer.android.com/reference/com/google/android/material/snackbar/Snackbar) | 自定义实现 |
| `PopupWindow` | `.bindPopup()` | 气泡弹窗 | [PopupWindow](https://developer.android.com/reference/android/widget/PopupWindow) | [bindPopup](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-universal-attributes-popup-V5) |
| `PopupMenu` / `ContextMenu` | `.bindMenu()` / `.bindContextMenu()` | 菜单弹窗 | [PopupMenu](https://developer.android.com/reference/android/widget/PopupMenu) | [bindMenu](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-universal-attributes-menu-V5) |

---

## 六、动画 API

| Android (API/类) | HarmonyOS (API/类) | 说明 | Android 文档参考 | HarmonyOS 文档参考 |
|---|---|---|---|---|
| `ObjectAnimator` | `animateTo()` | 显式属性动画。`animateTo({ duration, curve }, () => { this.xxx = newValue })` | [ObjectAnimator](https://developer.android.com/reference/android/animation/ObjectAnimator) | [animateTo](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-explicit-animation-V5) |
| `ValueAnimator` | `animator.create()` (`@ohos.animator`) | 值动画 | [ValueAnimator](https://developer.android.com/reference/android/animation/ValueAnimator) | [animator](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/js-apis-animator-V5) |
| `AnimatorSet` | `animateTo` 嵌套 / `keyframeAnimateTo` | 动画集合。多个 `animateTo` 配合延时，或关键帧动画 | [AnimatorSet](https://developer.android.com/reference/android/animation/AnimatorSet) | [keyframeAnimateTo](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-keyframeanimateto-V5) |
| `ViewPropertyAnimator` (view.animate()) | `.animation()` 属性动画 | 隐式动画。在属性后链式调用 `.animation({ duration, curve })` | [ViewPropertyAnimator](https://developer.android.com/reference/android/view/ViewPropertyAnimator) | [animation](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-animatorproperty-V5) |
| `TimeInterpolator` 系列 | `Curve` / `ICurve` | `AccelerateInterpolator`→`Curve.EaseIn`；`DecelerateInterpolator`→`Curve.EaseOut`；`Linear`→`Curve.Linear`；自定义→`curves.cubicBezierCurve()` | [Interpolator](https://developer.android.com/reference/android/view/animation/Interpolator) | [Curve](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-appendix-enums-V5) |
| `SpringAnimation` (AndroidX) | `curves.springCurve()` / `curves.springMotion()` | 弹簧动画 | [SpringAnimation](https://developer.android.com/reference/androidx/dynamicanimation/animation/SpringAnimation) | [curves.springCurve](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/js-apis-curve-V5) |
| `Transition` / `TransitionManager` | `.transition()` 属性 | 组件转场动画（插入/删除时的过渡） | [Transition](https://developer.android.com/reference/android/transition/Transition) | [transition](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-transition-animation-component-V5) |
| `makeSceneTransitionAnimation()` | `pageTransition()` | 页面转场动画。配置 `PageTransitionEnter`/`PageTransitionExit` | [Scene Transition](https://developer.android.com/guide/fragments/animate) | [pageTransition](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-page-transition-animation-V5) |
| Shared Element Transition | `.geometryTransition(id)` | 共享元素转场。标识两个页面中的共享元素，自动形变过渡 | [Shared Element](https://developer.android.com/guide/fragments/animate#shared) | [geometryTransition](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-transition-animation-geometrytransition-V5) |
| `MotionLayout` | 组合 `animateTo` + `@State` 状态切换 | 无直接对应。通过状态变量控制多组属性 + animateTo 过渡 | [MotionLayout](https://developer.android.com/reference/androidx/constraintlayout/motion/widget/MotionLayout) | 组合实现 |
| `Lottie` (第三方) | `@ohos/lottie` (三方库) | Lottie 动画。加载 JSON 动画文件 | [Lottie Android](https://github.com/airbnb/lottie-android) | [@ohos/lottie](https://ohpm.openharmony.cn/#/cn/detail/@ohos%2Flottie) |
| `AnimatedVectorDrawable` | `ImageAnimator` 组件 | 帧动画。逐帧播放图片序列 | [AnimatedVectorDrawable](https://developer.android.com/reference/android/graphics/drawable/AnimatedVectorDrawable) | [ImageAnimator](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-components-imageanimator-V5) |

---

## 七、手势 API

| Android (API/类) | HarmonyOS (API/类) | 说明 | Android 文档参考 | HarmonyOS 文档参考 |
|---|---|---|---|---|
| `View.OnClickListener` | `.onClick(() => {})` | 点击事件 | [OnClickListener](https://developer.android.com/reference/android/view/View.OnClickListener) | [onClick](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-universal-events-click-V5) |
| `View.OnLongClickListener` | `LongPressGesture` | 长按。`.gesture(LongPressGesture().onAction(() => {}))` | [OnLongClickListener](https://developer.android.com/reference/android/view/View.OnLongClickListener) | [LongPressGesture](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-gestures-longpressgesture-V5) |
| `View.OnTouchListener` / `MotionEvent` | `.onTouch((event: TouchEvent) => {})` | 触摸事件。包含 `type`（Down/Up/Move/Cancel）和 `touches` | [OnTouchListener](https://developer.android.com/reference/android/view/View.OnTouchListener) | [onTouch](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-universal-events-touch-V5) |
| `GestureDetector` | `GestureGroup` | 手势检测器。`GestureGroup(GestureMode.xxx, gesture1, gesture2)` | [GestureDetector](https://developer.android.com/reference/android/view/GestureDetector) | [GestureGroup](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-combined-gestures-V5) |
| `onFling()` | `SwipeGesture` | 快划手势。回调中可获取速度和角度 | [onFling](https://developer.android.com/reference/android/view/GestureDetector.OnGestureListener) | [SwipeGesture](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-gestures-swipegesture-V5) |
| `ScaleGestureDetector` | `PinchGesture` | 捏合/缩放。双指缩放，回调提供 `scale` 比例 | [ScaleGestureDetector](https://developer.android.com/reference/android/view/ScaleGestureDetector) | [PinchGesture](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-gestures-pinchgesture-V5) |
| `RotationGestureDetector` (自定义) | `RotationGesture` | 旋转手势。回调提供 `angle` 角度 | 自定义实现 | [RotationGesture](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-gestures-rotationgesture-V5) |
| 拖拽 (`View.OnDragListener`) | `PanGesture` / `.onDragStart()`/`.onDrop()` | `PanGesture` 自由拖动；`.onDragStart()`/`.onDrop()` 拖放操作 | [OnDragListener](https://developer.android.com/reference/android/view/View.OnDragListener) | [PanGesture](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-gestures-pangesture-V5) |
| `OnDoubleTapListener` | `TapGesture({ count: 2 })` | 双击手势 | [OnDoubleTapListener](https://developer.android.com/reference/android/view/GestureDetector.OnDoubleTapListener) | [TapGesture](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-gestures-tapgesture-V5) |
| 手势冲突 (`requestDisallowInterceptTouchEvent`) | `GestureMode.Exclusive`/`Parallel`/`Sequence` | 手势竞争策略。互斥/并行/顺序 | [requestDisallowInterceptTouchEvent](https://developer.android.com/reference/android/view/ViewGroup) | [GestureMode](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-combined-gestures-V5) |
| `OnFocusChangeListener` | `.onFocus()` / `.onBlur()` | 焦点事件 | [OnFocusChangeListener](https://developer.android.com/reference/android/view/View.OnFocusChangeListener) | [onFocus/onBlur](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-universal-events-focus-V5) |

---

## 八、无障碍 API

| Android (API/类) | HarmonyOS (API/类) | 说明 | Android 文档参考 | HarmonyOS 文档参考 |
|---|---|---|---|---|
| `android:contentDescription` | `.accessibilityText()` | 无障碍文本描述 | [contentDescription](https://developer.android.com/reference/android/view/View#attr_android:contentDescription) | [accessibilityText](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-universal-attributes-accessibility-V5) |
| `android:importantForAccessibility` | `.accessibilityLevel()` | 无障碍重要性。`"yes"`/`"no"`/`"no-hide-descendants"` | [importantForAccessibility](https://developer.android.com/reference/android/view/View#setImportantForAccessibility(int)) | 同上 |
| `AccessibilityNodeInfo` | `AccessibilityExtensionContext` | 无障碍节点信息 | [AccessibilityNodeInfo](https://developer.android.com/reference/android/view/accessibility/AccessibilityNodeInfo) | [AccessibilityExtensionContext](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/js-apis-inner-application-accessibilityextensioncontext-V5) |
| `AccessibilityService` | `AccessibilityExtensionAbility` | 无障碍服务 | [AccessibilityService](https://developer.android.com/reference/android/accessibilityservice/AccessibilityService) | [AccessibilityExtensionAbility](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/js-apis-application-accessibilityextensionability-V5) |
| `AccessibilityEvent` | `AccessibilityEvent` | 无障碍事件 | [AccessibilityEvent](https://developer.android.com/reference/android/view/accessibility/AccessibilityEvent) | [AccessibilityEvent](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/js-apis-accessibility-V5) |
| `AccessibilityDelegate` | `.accessibilityGroup(true)` + `.accessibilityDescription()` | 组件分组。合并多个子组件为一个无障碍焦点 | [AccessibilityDelegate](https://developer.android.com/reference/android/view/View.AccessibilityDelegate) | [accessibilityGroup](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-universal-attributes-accessibility-V5) |
| `AccessibilityManager` | `@ohos.accessibility` 模块 | 无障碍管理器 | [AccessibilityManager](https://developer.android.com/reference/android/view/accessibility/AccessibilityManager) | [accessibility 模块](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/js-apis-accessibility-V5) |
| TalkBack | 屏幕朗读 (系统辅助功能) | 屏幕阅读器 | [TalkBack](https://developer.android.com/guide/topics/ui/accessibility) | [无障碍开发指南](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/accessibility-development-overview-V5) |

---

## 九、View 通用属性映射

| Android 属性 | HarmonyOS 属性 | 说明 |
|---|---|---|
| `android:id` | `.id('xxx')` / 组件引用变量 | 组件标识 |
| `android:background` | `.backgroundColor()` / `.backgroundImage()` | 背景 |
| `android:alpha` | `.opacity()` | 透明度 0~1 |
| `android:elevation` | `.shadow({ radius, color, offsetX, offsetY })` | 阴影 |
| `android:rotation` | `.rotate({ angle: xxx })` | 旋转 |
| `android:scaleX/Y` | `.scale({ x: xxx, y: xxx })` | 缩放 |
| `android:translationX/Y` | `.translate({ x: xxx, y: xxx })` | 平移 |
| `android:clipToOutline` | `.clip(true)` / `.clipShape()` | 裁剪 |
| `setEnabled(false)` | `.enabled(false)` | 禁用状态 |
| `android:focusable` | `.focusable(true)` | 可聚焦 |
| `GradientDrawable` | `.linearGradient()` / `.radialGradient()` | 渐变背景 |
| `CardView` radius | `.borderRadius()` | 圆角 |
| `GradientDrawable.setStroke()` | `.border({ width, color, radius })` | 边框 |

---

## 迁移关键要点

1. **范式转换**: Android 使用命令式 UI（XML 布局 + Java/Kotlin 操作）；ArkUI 使用声明式 UI（ArkTS 组件树 + 状态驱动渲染）
2. **Adapter 模式消除**: `RecyclerView.Adapter`、`PagerAdapter` 等被 `ForEach`/`LazyForEach` + `IDataSource` 替代
3. **Fragment → NavDestination**: Fragment 模块化 UI 能力映射到 `Navigation` + `NavDestination` 路由
4. **动画统一**: Android 分散的动画 API（ObjectAnimator、ViewPropertyAnimator、Transition 等）在 ArkUI 中统一为 `animateTo()`（显式）和 `.animation()`（隐式）两种核心方式
5. **手势原生化**: ArkUI 将常见手势（点击、长按、拖拽、捏合、旋转）直接内置为组件属性，无需单独创建 GestureDetector
6. **声明式无障碍**: 通过组件属性 `.accessibilityText()`、`.accessibilityLevel()` 等配置，比 Android 命令式方式更简洁
