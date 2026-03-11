# 安卓应用到鸿蒙应用的UI布局映射报告

## 布局容器映射

| 安卓部分UI | 对应鸿蒙部分UI | 其他说明 |
|-----------|---------------|---------|
| `LinearLayout` (vertical) | `Column` 组件 | 纵向线性布局，支持 `.justifyContent()`、`.alignItems()` |
| `LinearLayout` (horizontal) | `Row` 组件 | 横向线性布局 |
| `FrameLayout` | `Stack` 组件 | 层叠布局，支持 `.alignContent()` |
| `RelativeLayout` | `RelativeContainer` 组件 | 相对布局，通过 `.alignRules()` 设置定位规则 |
| `ConstraintLayout` | `RelativeContainer` / 自定义布局 | 约束布局，复杂场景可用自定义 `onMeasureSize`/`onPlaceChildren` |
| `FlexboxLayout` | `Flex` 组件 | 弹性布局，支持 `FlexDirection`/`FlexWrap`/`FlexAlign` 标准 Flexbox |
| `GridLayout` | `Grid` 组件 | 网格布局，通过 `.columnsTemplate('1fr 1fr 1fr')` 定义列 |
| `CoordinatorLayout` | 组合 `Scroll` + `Column` + 动画 | 无直接对应，需组合模拟 AppBar 折叠效果 |
| `AppBarLayout` / `CollapsingToolbarLayout` | `Navigation` 标题栏 + `titleMode` | 折叠工具栏，`NavigationTitleMode.Full`→`.Mini` 自动切换 |
| `ScrollView` | `Scroll` 组件 | 滚动容器，通过 `Scroller` 控制器编程式滚动 |
| `HorizontalScrollView` | `Scroll` + `ScrollDirection.Horizontal` | 横向滚动 |
| `NestedScrollView` | `Scroll` + `.nestedScroll()` | 嵌套滚动策略配置 |
| `ViewPager` / `ViewPager2` | `Swiper` 组件 | 页面滑动切换，支持 `.autoPlay()`、`.indicator()`、`.loop()` |
| `SwipeRefreshLayout` | `Refresh` 组件 | 下拉刷新，通过 `.onRefreshing()` 回调处理刷新 |
| `RecyclerView` | `List` 组件 | 高性能列表，使用 `List` + `ForEach`/`LazyForEach` 渲染 `ListItem` |
| `RecyclerView` (GridLayoutManager) | `Grid` 组件 | 网格布局，通过 `.columnsTemplate()` 设置列数 |
| `RecyclerView` (StaggeredGridLayoutManager) | `WaterFlow` 组件 | 瀑布流布局 |
| `DrawerLayout` / `NavigationView` | `SideBarContainer` 组件 | 侧边栏/抽屉 |
| `TabLayout` + `ViewPager` | `Tabs` 组件 | 标签页，`Tabs` 整合标签栏和内容页切换 |
| `BottomNavigationView` | `Tabs` + `BarPosition.End` | 底部导航栏 |

## 布局属性映射

| 安卓部分UI | 对应鸿蒙部分UI | 其他说明 |
|-----------|---------------|---------|
| `layout_weight` | `.layoutWeight()` / `.flexGrow()` | 权重分配 |
| `padding` / `margin` | `.padding()` / `.margin()` | 支持统一或分别指定 `{ top, right, bottom, left }` |
| `View.GONE` / `View.INVISIBLE` | `Visibility.None` / `Visibility.Hidden` | `None`=不占位（GONE）；`Hidden`=占位不显示（INVISIBLE） |
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

## 页面结构映射

| 安卓部分UI | 对应鸿蒙部分UI | 其他说明 |
|-----------|---------------|---------|
| `Activity` | `PageAbility` | 主UI容器 |
| `Fragment` | `NavDestination` / `@Component` | 通过 Navigation + NavDestination 实现子页面路由 |
| `Toolbar` / `ActionBar` | `Navigation` 标题栏 | 通过 `.title()` 和 `.menus()` 配置 |
| `Intent` | `router` 模块 | 页面导航，使用 `router.push()`、`router.replace()` 等 |

## 列表与可滚动组件映射

| 安卓部分UI | 对应鸿蒙部分UI | 其他说明 |
|-----------|---------------|---------|
| `RecyclerView.Adapter` | `LazyForEach` + `IDataSource` | 数据适配器，实现 `IDataSource` 接口配合 `LazyForEach` 懒加载 |
| `RecyclerView.ViewHolder` | `@Builder` / `@Component` | 声明式 UI 无需 ViewHolder 模式 |
| `LinearLayoutManager` | `List` 默认布局 | 纵向列表，`.listDirection(Axis.Horizontal)` 切换横向 |
| `ItemTouchHelper` (swipe) | `ListItem.swipeAction()` | 列表项滑动操作菜单 |
| `DiffUtil` | `IDataSource` 的 `DataChangeListener` | 差异通知，`onDataAdd`/`onDataDelete`/`onDataChange` |
| `PagerAdapter` / `FragmentStateAdapter` | `Swiper` + `ForEach`/`LazyForEach` | 无需 Adapter 模式，直接循环渲染子页面 |

## 迁移注意事项

1. **范式转换**：Android 使用命令式 UI（XML 布局 + Java/Kotlin 操作）；ArkUI 使用声明式 UI（ArkTS 组件树 + 状态驱动渲染）
2. **Adapter 模式消除**：`RecyclerView.Adapter`、`PagerAdapter` 等被 `ForEach`/`LazyForEach` + `IDataSource` 替代
3. **Fragment → NavDestination**：Fragment 模块化 UI 能力映射到 `Navigation` + `NavDestination` 路由
4. **布局属性调整**：Android 的布局属性需要转换为 ArkUI 的链式调用语法
5. **性能优化**：使用 `LazyForEach` 进行懒加载，避免一次性渲染大量数据
6. **响应式布局**：使用 `Flex` 和 `Grid` 组件实现响应式布局，适应不同屏幕尺寸
