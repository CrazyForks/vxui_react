# VXUI 2

**官网**：[ui.vx.link](https://ui.vx.link) &nbsp;|&nbsp; **GitHub**：[tmplink/vxui_react](https://github.com/tmplink/vxui_react) &nbsp;|&nbsp; [English Version](README.md)

一套适用于后台、运营台、仪表盘和内部工具的通用 React UI 组件库。基于 CSS 自定义属性（设计令牌）做主题化，基于 Radix UI 基元做无障碍支持。版本 v2.0.0。

> **安全公告**：v2.0.0 是彻底清除供应链攻击痕迹后的安全版本。git 历史中发现的恶意 `postinstall` 脚本（提交 `9c15fc0` / `fd8700d`）和自引用依赖（`c2bc8b9`）均已清除。本版本同时将包名从 `vxui-react` 更名为 `vxui-2`。详见 [CHANGELOG.md](CHANGELOG.md)。

## 安装

```bash
npm install vxui-2
```

`react` 和 `react-dom`（均要求 ^19.0.0）需要由宿主应用提供。

## 快速开始

```tsx
import ReactDOM from 'react-dom/client';
import {
  ThemeProvider,
  ToastProvider,
  themePresets,
  AppShell,
  Button,
  Card,
  CardContent,
  CardHeader,
  CardTitle,
  Input,
} from 'vxui-2';
import 'vxui-2/styles.css';

ReactDOM.createRoot(document.getElementById('root')!).render(
  <ThemeProvider themes={themePresets} defaultTheme="light">
    <ToastProvider>
      <AppShell
        brand="Acme Ops"
        title="概览"
        navSections={[
          {
            title: '工作空间',
            items: [{ key: 'overview', label: '概览', active: true }],
          },
        ]}
        headerActions={<Button size="sm">新建项目</Button>}
      >
        <Card>
          <CardHeader>
            <CardTitle>队列健康</CardTitle>
          </CardHeader>
          <CardContent>
            <Input label="搜索订单" placeholder="PO-1024" />
          </CardContent>
        </Card>
      </AppShell>
    </ToastProvider>
  </ThemeProvider>,
);
```

## 主题定制

VXUI 2 使用 CSS 自定义属性。只需覆盖少量变量即可几分钟内完成品牌化改造：

```css
:root {
  --vx-primary: #7c3aed;
  --vx-primary-strong: #6d28d9;
  --vx-primary-soft: rgba(124, 58, 237, 0.1);
  --vx-bg: #f8fafc;
  --vx-surface: #ffffff;
  --vx-border: #e2e8f0;
  --vx-text: #0f172a;
  --vx-radius: 10px;
  --vx-sidebar-width: 240px;
}
```

当 `<html>` 带有 `class="dark"` 或 `data-theme="dark"` 时，深色模式令牌会自动生效。

运行时创建自定义主题：

```tsx
import { ThemeProvider, createTheme, themePresets } from 'vxui-2';

const themes = {
  ...themePresets,
  ocean: createTheme('dark', {
    label: 'Ocean',
    tokens: {
      '--vx-primary': '#38bdf8',
      '--vx-primary-strong': '#0ea5e9',
      '--vx-primary-soft': 'rgba(56, 189, 248, 0.16)',
      '--vx-bg': '#06131f',
      '--vx-surface': '#0d2236',
    },
  }),
};

<ThemeProvider themes={themes} defaultTheme="ocean">
  <App />
</ThemeProvider>
```

通过 `useTheme()` 读取当前主题并调用 `setTheme('ocean')` 在线切换。

## 组件列表

### 布局与导航
**AppShell**（应用框架）、**Shell** / ShellSidebar / ShellContent（底层布局）、**NavigationMenu**（导航菜单）、**Breadcrumb**（面包屑）、**Pagination**（分页）、**Tabs**（标签页）、**Menubar**（菜单栏）、**Stepper**（步骤）、**Resizable**（可调整面板）、**ScrollArea**（滚动区域）、**Separator**（分隔线）

### 按钮与操作
**Button**（按钮）、**Toggle**（切换）、**SegmentedControl**（分段控制器）、**DropdownMenu**（下拉菜单）、**ContextMenu**（右键菜单）

### 表单与输入
**Input**（输入框）、**Textarea**（多行文本）、**Checkbox**（复选框）、**Radio**（单选按钮）、**Switch**（开关）、**Slider**（滑块）、**NumberInput**（数字输入）、**TagInput**（标签输入）、**Rating**（星级评分）、**DatePicker**（日期选择）、**ColorPicker**（颜色选择）、**Select**（单选下拉）、**MultiSelect**（多选下拉）、**PinInput**（PIN 输入）、**TimePicker**（时间选择）、**Form**（表单）、**FileUpload**（文件上传）

### 浮层与弹出
**Dialog**（模态框）、**Sheet**（侧边面板）、**Popover**（弹出内容）、**Tooltip**（提示）、**HoverCard**（悬浮卡片）、**CommandPalette**（命令面板）

### 反馈与状态
**Toast**（轻通知）、**Notification**（全局通知）、**Alert**（内联警告）、**Progress**（进度条）、**Spinner**（加载指示器）、**Skeleton**（骨架屏）、**EmptyState**（空状态）、**Result**（结果页）

### 数据展示
**Table**（表格）、**Card**（卡片）、**Badge**（徽章）、**Avatar**（头像）、**Accordion**（折叠面板）、**Calendar**（日历）、**Image**（图片）、**Carousel**（轮播）、**TreeView**（树形列表）、**Timeline**（时间线）、**Descriptions**（描述列表）

### 排版
**Heading**（标题）、**Text**（文本）、**Label**（标签）、**CodeBlock**（代码块）、**Article**（文章布局）

### 移动端 (`vxui-2/mobile`)
**MobileShell**（移动端框架）、**MobileDrawer**（底部抽屉）、**BottomNav**（底部导航）、**MobileList**（移动端列表）、**ActionSheet**（操作面板）

### 工具
**ThemeProvider**（主题提供者）、**useTheme**（主题钩子）、**Responsive**（响应式条件渲染）、**LanguageSwitcher**（语言切换）、**VXUIProvider**（顶层组合提供者）

## 设计原则

- **令牌优先主题化** — 所有视觉决策都使用 CSS 变量。不依赖 Tailwind，不使用 CSS-in-JS。
- **默认无障碍** — 浮层和交互组件基于 Radix UI 基元构建，开箱支持键盘导航和 ARIA 语义。
- **无全局副作用** — CSS 都限定在 `.vx-*` 类名中，不会覆盖你的样式。
- **移动优先响应式** — 窄视口下侧边栏自动折叠为浮层。移动端组件单独导入。
- **无运行时 CSS 注入** — 样式作为单个静态样式表发布，没有未样式内容闪烁（FOUC）。

## 本地开发

```bash
npm install
npm run dev          # 启动开发服务器
npm run typecheck    # TypeScript 类型检查
npm run build        # 构建组件库（用于 npm publish）
npm run test         # 运行测试
npm run security:audit  # 安全审计
```

## 许可证

Apache-2.0