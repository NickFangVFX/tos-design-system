# tOS 组件移植规格（供改造 m3e-canvas 用）

默认主题：HiOS 浅色。品牌色 HiOS `#0077FF` / XOS `#00C763`。字体 TransSans SC。
文本色 text-icon-primary `#000000` / secondary `#48494D` / tertiary `#545559` / info `#939599` / disable `#B9BBBF`。
层色 layer 浅 `#FFFFFF` / bg-primary `#F0F1F2` / bg-secondary `#F5F6F7`；深色 bg `#000000` / `#18181A` / `#222325` / `#2F3033`。
描边 border-default `#DCDDE0` / stroke-weak `#E9EAEB`。警示 `#FF3430`。遮罩 `#00000033`。
圆角 scale：xs 8 / s 12 / m 16 / l 20 / xl 24 / xxl 28。间距 space：4/6/8/12/16/20/24/28/32/40/56。

## 已完成（勿重复改）
- button → OSBigButton：Big 296×48 / Middle 150×48 / Tiny 72-84×32，胶囊圆角，label-semibold 15/550
- switch → OSLiquidSwitch：轨道 44×24，滑块 18（上下留 3）
- topAppBar → TitleBar/OSLiquidToolBar：内容高 56dp

## 待改（本批）—— 每个组件成对改三处：tokens.ts 尺寸 + prompt.ts zh 描述（含 Android 代码名）+ 必要时渲染层
tokens.ts KindSpec 行号见 grep；prompt.ts zh 描述在 STYLE zh 段（约 1195-1240）+ 备用段（约 1420-1440）。

| Kind (m3e) | tOS 组件 | 关键尺寸 | Android 代码名 |
|---|---|---|---|
| iconButton | 图标按钮 | 44×44 外板 / 图标 24 | OSBigButton(icon) |
| fab | 悬浮按钮 FloatingButton | 圆形，OSLiquidSpringFloatingOvalButton | OSLiquidFob |
| extendedFab | 拓展悬浮按钮 | 高 48，胶囊 | OSLiquidBigButton |
| chip | 标签 | 高 32，圆角 8 | OSBigButton(tiny)/自定义 |
| bottomNav | 底部导航 TabBar | 移动 360×100，条高 60，item 52，图标 24，动作 60 | OSLiquidFootOperationBar |
| navRail | 侧边导航 | 沿用，改配色为 tOS | — |
| searchBar | 搜索框 SearchBar | 页面搜索 360×56，输入区 328×40，圆角随；底部搜索 360×104，输入 48，图标 20 | OSSearchBarCompose / OSLiquidSearchBarCompose |
| card | 卡片 | 圆角 16（list-card）；容器色 layer #FFFFFF | — |
| listItem | 列表项 ListItem | 行高 52，左右 padding 16，行内 padding 12/16，title 16/465，body 16/400 | OSListItem/OSListItemView |
| dialog | 弹窗 Dialog | 宽 328，圆角 28，横向 padding 24，内容宽 280，顶 padding 24，段间 16，按钮行 319×76，按钮高 44，按钮间距 12 | OSPromptDialog |
| snackbar | Snackbar | OSSnackbar，圆角 14，背景 90% #48494D | OSSnackbar/OSSnackbarHost |
| textField | 输入框 TextInput | 卡片输入 328×60 圆角16 / 通栏 360×39 / 多行 296×100；line-default #DCDDE0 聚焦 #939599；错误 #FF3430；光标绿 #00C763 | OSMaterialEditTextCompose |
| select | 下拉/选择 | 沿用 textField 风格 | OSSelectImageView |
| checkbox | 多选 Checkbox | 盒 18，圆角 4，选中 brand | OSCheckBox |
| slider | 滑块 Slider | 宽 296，轨道高 10 圆角 12，滑块 22（内 16），端点 4/选中 7 | OSSeekbar/OSSectionSeekbar |
| radio | 单选 Radio | 形 18（留 3），选中 brand 圆环+点 | OSRadioButton |
| divider | 分割线 | 1px，色 stroke-weak #E9EAEB | — |
| loadingIndicator | 加载 Loading | 图标 16/24/36，点 3/5/8，色 brand #0077FF | OSLoadingView |
| linearProgress | 线性进度 | 轨道高 4 圆角 2，页面顶 2，色 brand，轨道 #DCDDE0 | OSLoadingView |
| circularProgress | 环形进度 | 沿用，色 brand | OSLoadingView |
| carousel | 轮播 | 卡片圆角 16 | — |
| datePicker | 日期选择 | 用 tOS DateTimeDialog：宽 328 圆角 28，日期弹窗高 334 | OSDateTimePickerDialog |
| timePicker | 时间选择 | 同上 DateTimeDialog | OSDateTimePickerDialog |
| toolbar | 底部操作栏 ToolBar | 360×104，内容 y32/底28，组高 44，单图标 44，单文本宽 62，选项 56×36，图标 24 | OsLiqBottomToolBar |
| tabs | 顶部分段/标签 SegmentedTabs/LabelTabs | 分段 328×54，轨道高 38 圆角 24，item 高 34 圆角 28；标签 tab 高 54；超流体分段高 44 | OSSegmentedTab/OSTabLayout |
| splitButton | 拆分按钮 | 沿用按钮胶囊 + 箭头段 | OSBigButton 组合 |
| fabMenu | 悬浮菜单 | 沿用 fab | OSLiquidFob 组合 |
| bottomSheet | 底部面板 Panel | 宽 360，圆角顶 28，拖拽条圆角 2.5，list-card 16，标题高 56，底部按钮区 112，大按钮 296×48 | OSBottomSheetPanelCompose |
| text | 文本 | 用 tOS 字号层级：hero 32/550 h1 26/550 h2/h3；body1 16/400 title 16/500 caption 12 | — |
| box/image/camera/map | 容器/媒体 | 沿用，容器色对齐 tOS layer | — |

## 改造原则
1. 保留引擎逻辑（size-scale、connect、动画机制），只改尺寸常量、配色引用、prompt 描述文案。
2. prompt zh 描述统一格式：「组件（tOS XXX / Android 代码名）：尺寸…，配色用 brand/text-icon-*/layer/fill 等 token…」。
3. 改完 `npx tsc --noEmit` + `npx vitest run` 必须全绿（734 测试基线）；若因改默认值导致断言失败，同步更新对应测试。
4. 最后 `npx next build --webpack` 出 out/ 成功。
