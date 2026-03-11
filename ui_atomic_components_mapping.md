# 安卓应用到鸿蒙应用的UI原子组件映射报告

## 基础UI组件映射

| 安卓部分UI | 对应鸿蒙部分UI | 其他说明 |
|-----------|---------------|---------|
| `TextView` | `Text` 组件 | 文本显示，ArkUI 通过 `Text('内容')` 创建，支持 `.fontSize()`、`.fontColor()`、`.fontWeight()` 链式属性 |
| `TextView` (spannable) | `Text` + `Span` / `RichText` | 富文本，ArkUI 用 `Text` 内嵌 `Span` 子组件，或 `RichText` 渲染 HTML |
| `Button` | `Button` 组件 | 按钮，支持 `ButtonType.Capsule`/`Circle`/`Normal` 三种形态 |
| `ImageButton` | `Button` + `Image` 组合 | ArkUI 中用 `Button` 包裹 `Image` 子组件实现图标按钮 |
| `FloatingActionButton` | `Button` + `ButtonType.Circle` + `.position()` | 圆形 Button + 定位实现悬浮效果 |
| `ImageView` | `Image` 组件 | 图片显示，支持网络 URL、本地资源 `$r('app.media.xxx')`、PixelMap |
| `ImageView.ScaleType` | `Image.objectFit(ImageFit.xxx)` | `CENTER_CROP`→`Cover`；`FIT_CENTER`→`Contain`；`FIT_XY`→`Fill`；`CENTER`→`None` |
| `EditText` | `TextInput` / `TextArea` | 单行用 `TextInput`，多行用 `TextArea`，通过 `.onChange()` 监听输入 |
| `EditText` (inputType) | `TextInput.type(InputType.xxx)` | `NUMBER`→`InputType.Number`；`PASSWORD`→`InputType.Password`；`EMAIL`→`InputType.Email` |
| `SearchView` | `Search` 组件 | 搜索栏，支持 `.onSubmit()`、`.onChange()` |
| `CheckBox` | `Checkbox` 组件 | 复选框 |
| `RadioButton` / `RadioGroup` | `Radio` 组件 | 单选按钮，通过 `group` 参数分组 |
| `Switch` / `ToggleButton` | `Toggle` 组件 | 开关，支持 `ToggleType.Switch`/`Checkbox`/`Button` |
| `ProgressBar` | `Progress` 组件 | 进度条，支持 `Linear`/`Ring`/`Eclipse`/`ScaleRing`/`Capsule` 样式 |
| `SeekBar` | `Slider` 组件 | 滑动条 |
| `RatingBar` | `Rating` 组件 | 评分条 |
| `Spinner` | `Select` 组件 | 下拉选择 |
| `AutoCompleteTextView` | `TextInput` + 自定义下拉 | 自动完成输入，需自定义实现下拉建议 |
| `MultiAutoCompleteTextView` | `TextInput` + 自定义下拉 | 多自动完成输入，需自定义实现 |
| `DatePicker` / `TimePicker` | `DatePicker` / `TimePicker` | 日期/时间选择器，还有 `DatePickerDialog`/`TimePickerDialog` 弹窗式 |
| `CalendarView` | `CalendarPicker` 组件 | 日历视图 |
| `WebView` | `Web` 组件 | 网页加载，使用 `WebviewController` 控制 |
| `VideoView` | `Video` 组件 | 视频播放，支持 `.autoPlay()`、`.controls()`、`.loop()` |
| `AudioRecord` | `AudioRecorder` 组件 | 音频录制 |
| `MediaPlayer` | `Audio` 组件 | 音频播放 |
| `Divider` (Material) | `Divider` 组件 | 分隔线 |
| `Badge` (Material) | `Badge` 组件 | 角标/徽章 |
| `Marquee` (ellipsize) | `Marquee` 组件 | 跑马灯，ArkUI 提供独立 Marquee 组件 |
| `Chip` (Material) | `Chip` 组件 | 芯片/标签 |
| `TextClock` | `Text` + 时间格式化 | 文本时钟，使用 `Text` 组件显示格式化的时间 |
| `AnalogClock` | 自定义组件 | 模拟时钟，需自定义实现 |
| `DigitalClock` | `Text` + 时间格式化 | 数字时钟，使用 `Text` 组件显示格式化的时间 |
| `View` | `Component` / `Container` | 基础视图，使用 `Component` 或 `Container` 实现 |
| `SurfaceView` | `TextureView` |  surface 视图，使用 `TextureView` 实现 |
| `TextureView` | `TextureView` | 纹理视图 |
| `MaterialToolbar` | `Navigation` 标题栏 | Material 工具栏，映射到 `Navigation` 组件 |
| `MaterialButton` | `Button` 组件 | Material 按钮，使用 `Button` 组件并设置样式 |
| `MaterialCardView` | `Card` 组件 | Material 卡片，使用 `Card` 组件实现 |
| `MaterialSwitch` | `Toggle` 组件 | Material 开关，使用 `Toggle` 组件并设置 `ToggleType.Switch` |
| `TextInputLayout` | `TextInput` + 自定义容器 | 文本输入布局，需自定义实现标签和错误提示 |
| `TextInputEditText` | `TextInput` 组件 | 文本输入编辑框，使用 `TextInput` 组件 |
| `AppBarLayout` | `Navigation` 标题栏 | 应用栏布局，映射到 `Navigation` 组件 |
| `BottomSheetDragHandleView` | 自定义组件 | 底部抽屉拖动句柄，需自定义实现 |
| `Space` | `Blank` 组件 | 空白间隔，使用 `Blank` 组件实现 |
| `CropImageView` (第三方) | 自定义组件 | 图片裁剪视图，需自定义实现 |
| `RecyclerViewFastScroller` (第三方) | 自定义组件 | 列表快速滚动条，需自定义实现 |
| `VrPanoramaView` (Google VR) | 自定义组件 | VR 全景视图，需自定义实现 |
| `VrVideoView` (Google VR) | 自定义组件 | VR 视频视图，需自定义实现 |
| 自定义组件 (如 ChapterSeekBar, TotpProgressBar, EditorDrawCanvas 等) | 自定义组件 | 自定义组件需要重新实现，保持功能和外观一致 |

## 弹窗与提示组件映射

| 安卓部分UI | 对应鸿蒙部分UI | 其他说明 |
|-----------|---------------|---------|
| `AlertDialog` | `AlertDialog.show()` | 警告对话框 |
| `Dialog` / `DialogFragment` | `@CustomDialog` 装饰器 | 自定义对话框，通过 `CustomDialogController` 控制显隐 |
| `BottomSheetDialog` | `ActionSheet.show()` / `.bindSheet()` | 底部弹出面板 |
| `Toast.makeText().show()` | `promptAction.showToast()` | 轻提示 |
| `Snackbar` | `promptAction.showToast()` + 自定义 | 无直接对应，需自定义实现带操作按钮的效果 |
| `PopupWindow` | `.bindPopup()` | 气泡弹窗 |
| `PopupMenu` / `ContextMenu` | `.bindMenu()` / `.bindContextMenu()` | 菜单弹窗 |
| `DatePickerDialog` | `DatePickerDialog.show()` | 日期选择对话框 |
| `TimePickerDialog` | `TimePickerDialog.show()` | 时间选择对话框 |
| `NumberPicker` | `Stepper` 组件 / 自定义 | 数字选择器，使用 `Stepper` 组件或自定义实现 |
| `ProgressDialog` | `@CustomDialog` + `Progress` | 进度对话框，自定义实现 |
| `AlertDialog.Builder` | `AlertDialog.show()` 参数配置 | 对话框构建器 |

## 动画API映射

| 安卓部分UI | 对应鸿蒙部分UI | 其他说明 |
|-----------|---------------|---------|
| `ObjectAnimator` | `animateTo()` | 显式属性动画，`animateTo({ duration, curve }, () => { this.xxx = newValue })` |
| `ValueAnimator` | `animator.create()` (`@ohos.animator`) | 值动画 |
| `AnimatorSet` | `animateTo` 嵌套 / `keyframeAnimateTo` | 动画集合，多个 `animateTo` 配合延时，或关键帧动画 |
| `ViewPropertyAnimator` (view.animate()) | `.animation()` 属性动画 | 隐式动画，在属性后链式调用 `.animation({ duration, curve })` |
| `TimeInterpolator` 系列 | `Curve` / `ICurve` | `AccelerateInterpolator`→`Curve.EaseIn`；`DecelerateInterpolator`→`Curve.EaseOut`；`Linear`→`Curve.Linear`；自定义→`curves.cubicBezierCurve()` |
| `SpringAnimation` (AndroidX) | `curves.springCurve()` / `curves.springMotion()` | 弹簧动画 |
| `Transition` / `TransitionManager` | `.transition()` 属性 | 组件转场动画（插入/删除时的过渡） |
| `makeSceneTransitionAnimation()` | `pageTransition()` | 页面转场动画，配置 `PageTransitionEnter`/`PageTransitionExit` |
| `Shared Element Transition` | `.geometryTransition(id)` | 共享元素转场，标识两个页面中的共享元素，自动形变过渡 |
| `MotionLayout` | 组合 `animateTo` + `@State` 状态切换 | 无直接对应，通过状态变量控制多组属性 + animateTo 过渡 |
| `Lottie` (第三方) | `@ohos/lottie` (三方库) | Lottie 动画，加载 JSON 动画文件 |
| `AnimatedVectorDrawable` | `ImageAnimator` 组件 | 帧动画，逐帧播放图片序列 |
| `AnimationDrawable` | `ImageAnimator` 组件 | 帧动画，逐帧播放图片序列 |
| `Interpolator` | `Curve` | 插值器，控制动画速度变化 |
| `AccelerateInterpolator` | `Curve.EaseIn` | 加速插值器 |
| `DecelerateInterpolator` | `Curve.EaseOut` | 减速插值器 |
| `AccelerateDecelerateInterpolator` | `Curve.EaseInOut` | 加速减速插值器 |
| `LinearInterpolator` | `Curve.Linear` | 线性插值器 |
| `BounceInterpolator` | `Curve.Bounce` | 弹跳插值器 |
| `OvershootInterpolator` | `Curve.Overshoot` | 过冲插值器 |
| `AnticipateInterpolator` | `Curve.Anticipate` | 预期插值器 |
| `AnticipateOvershootInterpolator` | `Curve.AnticipateOvershoot` | 预期过冲插值器 |

## 手势API映射

| 安卓部分UI | 对应鸿蒙部分UI | 其他说明 |
|-----------|---------------|---------|
| `View.OnClickListener` | `.onClick(() => {})` | 点击事件 |
| `View.OnLongClickListener` | `LongPressGesture` | 长按，`.gesture(LongPressGesture().onAction(() => {}))` |
| `View.OnTouchListener` / `MotionEvent` | `.onTouch((event: TouchEvent) => {})` | 触摸事件，包含 `type`（Down/Up/Move/Cancel）和 `touches` |
| `GestureDetector` | `GestureGroup` | 手势检测器，`GestureGroup(GestureMode.xxx, gesture1, gesture2)` |
| `onFling()` | `SwipeGesture` | 快划手势，回调中可获取速度和角度 |
| `ScaleGestureDetector` | `PinchGesture` | 捏合/缩放，双指缩放，回调提供 `scale` 比例 |
| `RotationGestureDetector` (自定义) | `RotationGesture` | 旋转手势，回调提供 `angle` 角度 |
| 拖拽 (`View.OnDragListener`) | `PanGesture` / `.onDragStart()`/`.onDrop()` | `PanGesture` 自由拖动；`.onDragStart()`/`.onDrop()` 拖放操作 |
| `OnDoubleTapListener` | `TapGesture({ count: 2 })` | 双击手势 |
| `OnTripleTapListener` | `TapGesture({ count: 3 })` | 三击手势 |
| 手势冲突 (`requestDisallowInterceptTouchEvent`) | `GestureMode.Exclusive`/`Parallel`/`Sequence` | 手势竞争策略，互斥/并行/顺序 |
| `OnFocusChangeListener` | `.onFocus()` / `.onBlur()` | 焦点事件 |
| `OnGenericMotionListener` | `.onGenericMotion()` | 通用运动事件 |
| `OnHoverListener` | `.onHover()` | 悬停事件 |
| `OnKeyListener` | `.onKeyEvent()` | 按键事件 |
| `OnTouchModeChangeListener` | `.onTouchModeChange()` | 触摸模式变化事件 |

## 无障碍API映射

| 安卓部分UI | 对应鸿蒙部分UI | 其他说明 |
|-----------|---------------|---------|
| `android:contentDescription` | `.accessibilityText()` | 无障碍文本描述 |
| `android:importantForAccessibility` | `.accessibilityLevel()` | 无障碍重要性，`"yes"`/`"no"`/`"no-hide-descendants"` |
| `android:labelFor` | `.accessibilityLabelFor()` | 标签关联 |
| `android:hint` | `.placeholder()` | 输入提示 |
| `AccessibilityNodeInfo` | `AccessibilityExtensionContext` | 无障碍节点信息 |
| `AccessibilityService` | `AccessibilityExtensionAbility` | 无障碍服务 |
| `AccessibilityEvent` | `AccessibilityEvent` | 无障碍事件 |
| `AccessibilityDelegate` | `.accessibilityGroup(true)` + `.accessibilityDescription()` | 组件分组，合并多个子组件为一个无障碍焦点 |
| `AccessibilityManager` | `@ohos.accessibility` 模块 | 无障碍管理器 |
| `View.announceForAccessibility()` | `accessibility.announce()` | 无障碍播报 |
| `ExploreByTouchHelper` | 自定义无障碍实现 | 触摸探索辅助 |
| `AccessibilityEvent.TYPE_VIEW_CLICKED` | `AccessibilityEvent.Type.CLICK` | 点击事件 |
| `AccessibilityEvent.TYPE_VIEW_FOCUSED` | `AccessibilityEvent.Type.FOCUS` | 焦点事件 |
| `AccessibilityEvent.TYPE_VIEW_TEXT_CHANGED` | `AccessibilityEvent.Type.TEXT_CHANGE` | 文本变化事件 |

## 迁移注意事项

1. **组件命名差异**：部分组件在鸿蒙中名称不同，如 `TextView` 对应 `Text`，`ImageView` 对应 `Image`
2. **API调用方式**：安卓使用方法调用，鸿蒙使用链式调用，如 `textView.setText()` 对应 `Text('内容')`
3. **属性设置**：安卓使用XML属性或setter方法，鸿蒙使用链式调用设置属性
4. **事件处理**：安卓使用监听器接口，鸿蒙使用回调函数
5. **状态管理**：鸿蒙使用 `@State`、`@Prop` 等装饰器进行状态管理，替代安卓的手动UI更新
6. **生命周期**：鸿蒙组件有自己的生命周期，需要调整代码适应
7. **性能优化**：使用 `LazyForEach` 进行列表懒加载，使用 `@Builder` 优化组件渲染
8. **兼容性**：部分安卓特有组件需要使用鸿蒙组件组合实现
9. **主题与样式**：鸿蒙使用 `@Style` 装饰器定义样式，替代安卓的XML样式
10. **资源管理**：鸿蒙使用 `$r('app.resource.xxx')` 引用资源，替代安卓的 `R.xxx`
