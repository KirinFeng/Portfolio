# Kirin Bilingual Online Résumé / Kirin 中英双语在线简历

A responsive bilingual résumé and portfolio built with native HTML, CSS, and JavaScript. It requires no framework, build process, package manager, database, or backend.

使用原生 HTML、CSS 和 JavaScript 构建的响应式中英双语在线简历与作品集，无需框架、构建流程、包管理器、数据库或后端。

---

## English

### Overview

The résumé is presented as a compact application-style interface rather than a conventional long page. The desktop version uses a fixed profile sidebar and a tabbed workspace. Five sections can be opened without reloading:

1. About
2. Skills
3. Projects
4. Experience
5. Credentials

The interface includes English/Chinese localization, light and dark themes, URL hash navigation, project dialogs, expandable experience entries, selectable skill chips, animated counters, pointer-responsive cards, and mobile layouts.

### Technology

- Semantic HTML5
- Native CSS: Grid, Flexbox, custom properties, media queries, `color-mix()`, and `clamp()`
- Vanilla JavaScript and standard browser APIs
- `localStorage` for language and theme preferences
- Native `<dialog>` for project details
- Static hosting compatible with GitHub Pages and Netlify

No installation or compilation is required.

### Directory structure

```text
.
├── .nojekyll
├── favicon.svg
├── index.html
├── README.md
├── script.js
├── styles.css
└── img/
    ├── icon-face.jpg
    ├── icon-title-in.png
    └── icon-title-out.png
```

The download controls reference `resume.pdf`, but that file is not included by default. Add it to the project root to enable PDF downloads.

### File reference

#### `index.html`

Defines the complete document structure and fallback content.

- Declares metadata, Open Graph fields, theme color, favicon, stylesheet, and deferred script.
- Provides a keyboard-accessible skip link.
- Builds `.profile-sidebar` with the brand, portrait, availability, identity, navigation, contact details, social links, and PDF download.
- Builds `.workspace` with the section indicator, language/theme controls, content panels, and footer.
- Connects five tabs and panels with `role="tablist"`, `role="tab"`, `role="tabpanel"`, `aria-controls`, and `aria-labelledby`.
- Defines the About introduction, personal facts, statement, and metrics.
- Defines expandable Experience entries, education, and current focus.
- Defines one featured and two compact Projects.
- Defines selectable Skills, proficiency bars, programming-language bars, and the résumé card.
- Defines the Credentials list.
- Provides the native project `<dialog>`, toast notification, and decorative cursor.
- Uses `data-i18n` for plain translated text, `data-i18n-html` for trusted translated markup, and `data-i18n-aria` for translated accessible labels.
- Uses `data-tab`, `data-panel`, `data-project`, `data-count`, and `data-suffix` as JavaScript hooks.

#### `styles.css`

Controls the visual system, layout, responsive behavior, and animation layer.

- Defines light-theme tokens in `:root` and dark-theme overrides in `html.dark`.
- Centralizes colors, surfaces, typography, borders, shadows, radii, and accent colors with CSS custom properties.
- Creates the desktop shell with a fixed-width sidebar and flexible workspace.
- Uses a 12-column, two-row card grid for the main panels.
- Assigns panel-specific card spans and internal layouts.
- Keeps the About headline on one line with responsive sizing.
- Styles navigation state, language/theme controls, footer, dialog, toast, and custom cursor.
- Uses JavaScript-updated variables `--px`, `--py`, `--rx`, and `--ry` for card lighting and 3D tilt.
- Animates panel entry, progress bars, and dialog appearance.
- Provides visible keyboard focus states.
- Responsive breakpoints:
  - `max-width: 1050px`: narrower sidebar and spacing.
  - `max-height: 680px` on desktop: compressed vertical layout.
  - `max-width: 800px`: sidebar becomes compact top navigation; secondary profile/contact details are hidden.
  - `max-width: 580px`: cards stack into one column and the active panel becomes internally scrollable.
- Disables pointer transforms on coarse/touch pointers.
- Honors `prefers-reduced-motion: reduce`.

#### `script.js`

Provides localization, navigation state, interactions, animation logic, and preference persistence.

Data structures:

- `translations.zh` and `translations.en`: Chinese and English UI dictionaries.
- `projectDetails.zh` and `projectDetails.en`: localized project-dialog content.
- `panelMeta`: panel identifiers, display indices, and translated labels.

Core behaviors:

- `setLanguage(language)` updates `<html lang>`, metadata, all localization bindings, active panel text, skill status, and any open project dialog. It saves `resume-language` in `localStorage`.
- `setTheme(theme)` toggles `html.dark`, updates the browser theme color, and saves `resume-theme`.
- `activatePanel(name, updateHistory)` updates active classes, `aria-selected`, tab order, panel visibility, the section indicator, entry animation, and URL hash.
- Tab controls support pointer input, Arrow keys, Home, and End. Browser Back/Forward is handled through `popstate`.
- The timeline accordion keeps at most one entry expanded and synchronizes `aria-expanded` with its plus/minus icon.
- Skill buttons toggle `.is-selected` and `aria-pressed`; the localized selected count updates immediately.
- Project buttons load localized data and open the native modal with `showModal()`. The close button and backdrop can dismiss it.
- Elements with `data-count` animate for 750 ms; reduced-motion users receive the final value immediately.
- Fine-pointer devices receive card tilt, lighting, internal parallax, and a custom cursor using `requestAnimationFrame`.
- Placeholder `#` links are prevented from navigating.
- PDF links opened through `file:` display a localized reminder toast. Test actual downloads over HTTP.
- Initialization restores saved preferences, otherwise uses English and the operating-system color preference; it also reads the URL hash and inserts the current year.

#### `favicon.svg`

Alternative scalable favicon with a rounded dark background and blue geometric monogram. The active favicon in `index.html` currently points to `img/icon-title-out.png`; change the link to `favicon.svg` to use this SVG.

#### `.nojekyll`

An intentionally empty GitHub Pages control file. It publishes the directory as a plain static site without Jekyll processing.

#### `img/icon-face.jpg`

Profile portrait shown in the sidebar. CSS crops it with `object-fit: cover` and rounded corners.

#### `img/icon-title-in.png`

Brand image inside the sidebar monogram link. Selecting it returns to About.

#### `img/icon-title-out.png`

PNG currently used as the browser-tab favicon.

#### `README.md`

Bilingual documentation for architecture, interactions, customization, accessibility, and deployment.

### Layout architecture

On desktop, `.resume-app` uses two columns: the profile/navigation sidebar and a flexible workspace. The workspace has a header, active panel, and footer. Each panel contains a heading and card grid. Only one `.tab-panel` is visible; inactive panels use the native `hidden` attribute.

At 800 px and below, the vertical sidebar becomes a top navigation area. At 580 px and below, multi-column grids become a single-column stream and the active panel can scroll internally.

### Localization model

Every translated element must reference a key present in both language dictionaries.

```html
<span data-i18n="locationLabel">Based in</span>
<p data-i18n-html="intro">Fallback content</p>
<button data-i18n-aria="themeLabel" aria-label="Toggle theme"></button>
```

When adding translated content:

1. Add the same key to `translations.zh` and `translations.en`.
2. Bind the HTML element with the correct localization attribute.
3. Use `data-i18n-html` only for intentional, trusted HTML.
4. Test both languages on every panel.

### Customization

#### Personal information

Update `name`, `role`, `locationValue`, availability, focus, languages, and introduction in both dictionaries. Update the phone number, email, LinkedIn URL, and GitHub URL directly in `index.html`.

#### Images

Replace files in `img/` while retaining their filenames, or update their `src` paths. A clear square or portrait image is recommended for the profile photo.

#### Experience and education

Edit dates in `index.html`. Edit localized job titles, organizations, achievements, degree, school, and focus text in both dictionaries.

#### Projects

Edit project-card copy in the translation dictionaries and dialog content in `projectDetails.zh` and `projectDetails.en`. Each button's `data-project` value must match its object key.

#### Skills and bars

Edit skill buttons in `index.html`. Add translation keys and `data-i18n` for localized skills. Use `--level` for skill bars and `--progress` for programming-language bars.

```html
<i style="--level:88%"></i>
<i style="--progress:90%"></i>
```

#### Metrics

Use `data-count` for the target and optional `data-suffix` for a suffix.

```html
<strong data-count="3" data-suffix="+">0+</strong>
```

#### PDF résumé

Place `resume.pdf` in the project root. Both download links already point there. Use a local HTTP server or deployed site to test the download.

### Run locally

Open `index.html` directly, or run a local server for production-like behavior:

```bash
python -m http.server 8000
```

Open `http://localhost:8000`.

### Deployment

#### GitHub Pages

1. Commit the project to a GitHub repository.
2. Open **Settings → Pages**.
3. Select **Deploy from a branch**.
4. Select `main` and `/root`.
5. Save and wait for the public URL.

#### Netlify or another static host

Upload the entire directory. No build command is required; the publish directory is the project root.

### Accessibility and compatibility

- Semantic landmarks and ARIA tab relationships.
- Arrow, Home, and End keyboard navigation.
- ARIA selected, expanded, and pressed states.
- Visible keyboard focus indicators.
- `aria-live="polite"` for the section indicator.
- Decorative graphics hidden from assistive technology where appropriate.
- Reduced-motion support.
- Native modal focus behavior through `<dialog>`.

Modern evergreen browsers are recommended because the site uses `<dialog>`, `color-mix()`, Grid, custom properties, and `dvh` units.

---

## 中文

### 项目概述

本项目将传统简历设计为紧凑型应用界面，而不是连续滚动的长页面。桌面端使用固定个人信息侧栏和标签式工作区，共有五个无需刷新即可切换的栏目：

1. 关于
2. 技能与证书
3. 项目
4. 经历
5. 资质认证

页面支持中英文切换、深浅色主题、URL Hash 导航、项目弹窗、经历展开、技能选择、数字动画、指针响应卡片和移动端布局。

### 技术栈

- 语义化 HTML5
- 原生 CSS：Grid、Flexbox、自定义属性、媒体查询、`color-mix()` 和 `clamp()`
- 原生 JavaScript 与标准浏览器 API
- 使用 `localStorage` 保存语言和主题偏好
- 使用原生 `<dialog>` 显示项目详情
- 支持 GitHub Pages、Netlify 等静态托管平台

项目无需安装依赖或执行编译。

### 目录结构

```text
.
├── .nojekyll
├── favicon.svg
├── index.html
├── README.md
├── script.js
├── styles.css
└── img/
    ├── icon-face.jpg
    ├── icon-title-in.png
    └── icon-title-out.png
```

下载按钮引用了 `resume.pdf`，但项目默认未包含该文件。如需 PDF 下载，请将其放入根目录。

### 文件详细说明

#### `index.html`

负责完整页面结构和脚本加载前的默认内容。

- 声明元数据、Open Graph 信息、主题颜色、站点图标、样式表和延迟脚本。
- 提供可由键盘访问的“跳到主要内容”链接。
- `.profile-sidebar` 包含品牌、头像、求职状态、姓名、职位、地点、导航、联系方式、社交链接和 PDF 下载。
- `.workspace` 包含栏目状态、语言与主题按钮、内容面板和页脚。
- 使用 `role="tablist"`、`role="tab"`、`role="tabpanel"`、`aria-controls` 和 `aria-labelledby` 连接五组标签页。
- About 包含简介、个人信息、当前标语和统计指标。
- Experience 包含可展开时间线、教育背景和当前关注方向。
- Projects 包含一个重点项目和两个紧凑项目。
- Skills 包含可选技能标签、能力条、编程语言条和 PDF 简历卡片。
- Credentials 包含证书与荣誉列表。
- 定义项目 `<dialog>`、Toast 提示和装饰性光标。
- `data-i18n` 用于纯文本，`data-i18n-html` 用于可信 HTML，`data-i18n-aria` 用于无障碍标签。
- `data-tab`、`data-panel`、`data-project`、`data-count` 和 `data-suffix` 用于连接脚本行为。

#### `styles.css`

负责视觉系统、布局、响应式规则和动画。

- `:root` 定义浅色主题变量，`html.dark` 覆盖为深色主题。
- 通过 CSS 自定义属性统一管理颜色、表面、文字、边框、阴影、圆角和强调色。
- 桌面端使用固定宽度侧栏与自适应工作区。
- 主要面板使用 12 列、2 行卡片网格，并分别定义各面板的卡片跨度。
- 使用响应式字号保持 About 大标题单行。
- 设置导航、语言/主题按钮、页脚、弹窗、Toast 和自定义光标样式。
- 使用脚本更新的 `--px`、`--py`、`--rx` 和 `--ry` 实现卡片光效与 3D 倾斜。
- 提供面板进入、进度条和弹窗动画，以及键盘焦点样式。
- 响应式断点：
  - `max-width: 1050px`：缩窄侧栏和间距。
  - 桌面宽度且 `max-height: 680px`：压缩矮屏垂直空间。
  - `max-width: 800px`：侧栏转换为顶部导航，并隐藏次要个人信息与联系方式。
  - `max-width: 580px`：卡片改为单列，当前面板允许内部滚动。
- 在触摸设备上关闭指针变换，并响应 `prefers-reduced-motion: reduce`。

#### `script.js`

负责本地化、导航状态、交互、动画和偏好保存。

主要数据结构：

- `translations.zh` 与 `translations.en`：中英文界面字典。
- `projectDetails.zh` 与 `projectDetails.en`：中英文项目弹窗内容。
- `panelMeta`：面板 ID、编号和翻译标签映射。

主要功能：

- `setLanguage(language)` 更新页面语言、元数据、本地化绑定、当前栏目、技能状态和已打开的项目弹窗，并保存 `resume-language`。
- `setTheme(theme)` 切换 `html.dark`、更新浏览器主题色，并保存 `resume-theme`。
- `activatePanel(name, updateHistory)` 更新激活类、`aria-selected`、Tab 顺序、面板可见性、栏目状态、动画和 URL Hash。
- 标签页支持鼠标、触摸、方向键、Home、End 和浏览器前进/后退。
- 经历折叠确保同一时间最多展开一项，并同步 `aria-expanded` 和加减号。
- 技能按钮切换 `.is-selected` 与 `aria-pressed`，并实时显示本地化的选中数量。
- 项目按钮加载对应语言数据并通过 `showModal()` 打开弹窗；关闭按钮和背景均可关闭弹窗。
- `data-count` 数字执行 750 毫秒动画；减少动态效果模式下直接显示最终值。
- 精确指针设备通过 `requestAnimationFrame` 获得卡片光效、倾斜、视差和自定义光标。
- 阻止 `href="#"` 占位链接跳转。
- 使用 `file:` 打开页面时，PDF 按钮显示本地化 Toast；真实下载应通过 HTTP 测试。
- 初始化时恢复偏好，否则默认英文并读取系统配色，同时读取 URL Hash 和写入当前年份。

#### `favicon.svg`

备用的可缩放站点图标，包含深色圆角背景和蓝色几何图形。`index.html` 当前实际引用 `img/icon-title-out.png`；如需使用 SVG，请修改 `<link rel="icon">`。

#### `.nojekyll`

内容有意为空的 GitHub Pages 控制文件，用于关闭 Jekyll 处理并按普通静态文件发布。

#### `img/icon-face.jpg`

侧栏头像，由 CSS 使用 `object-fit: cover` 和圆角裁切。

#### `img/icon-title-in.png`

侧栏品牌链接中的图片，点击后返回 About。

#### `img/icon-title-out.png`

当前用于浏览器标签页的 PNG 图标。

#### `README.md`

当前中英文项目文档，覆盖架构、交互、定制、无障碍和部署。

### 布局架构

桌面端 `.resume-app` 使用双列结构：左侧为个人信息与导航，右侧为自适应工作区。工作区纵向分为页头、当前面板和页脚。每个面板由标题和卡片网格组成。任何时刻仅显示一个 `.tab-panel`，未激活面板使用原生 `hidden` 属性隐藏。

宽度不超过 800 px 时，垂直侧栏转换为顶部导航。宽度不超过 580 px 时，多列卡片转换为单列，当前面板允许内部滚动。

### 中英文切换机制

每个翻译元素都必须引用同时存在于两个语言字典中的键。

```html
<span data-i18n="locationLabel">Based in</span>
<p data-i18n-html="intro">Fallback content</p>
<button data-i18n-aria="themeLabel" aria-label="切换主题"></button>
```

新增双语内容时：

1. 在 `translations.zh` 和 `translations.en` 中添加相同键名。
2. 为 HTML 元素添加对应的本地化属性。
3. 仅在内容确实包含可信 HTML 时使用 `data-i18n-html`。
4. 在每个面板中测试两种语言。

### 内容定制

#### 个人信息

在两个翻译字典中同步修改 `name`、`role`、`locationValue`、求职状态、方向、语言和简介。在 `index.html` 中修改电话、邮箱、LinkedIn 和 GitHub 地址。

#### 图片

直接替换 `img/` 中的同名文件，或在 `index.html` 中修改 `src`。头像建议使用清晰的正方形或竖版图片。

#### 经历与教育

在 `index.html` 中修改日期；在两个翻译字典中修改职位、机构、成果、学位、学校和关注方向。

#### 项目

在翻译字典中修改项目卡片文案，在 `projectDetails.zh` 和 `projectDetails.en` 中修改弹窗内容。按钮的 `data-project` 必须与对象键名一致。

#### 技能与进度条

在 `index.html` 中修改技能按钮。需要翻译的技能应添加字典键和 `data-i18n`。能力条使用 `--level`，编程语言条使用 `--progress`。

```html
<i style="--level:88%"></i>
<i style="--progress:90%"></i>
```

#### 指标数字

使用 `data-count` 设置目标值，使用可选的 `data-suffix` 设置后缀。

```html
<strong data-count="3" data-suffix="+">0+</strong>
```

#### PDF 简历

将文件命名为 `resume.pdf` 并放入项目根目录。两个下载入口已指向该路径。请通过本地 HTTP 服务或已部署站点测试下载。

### 本地运行

可以直接打开 `index.html`，或启动本地服务：

```bash
python -m http.server 8000
```

然后访问 `http://localhost:8000`。

### 部署

#### GitHub Pages

1. 将项目提交到 GitHub 仓库。
2. 打开 **Settings → Pages**。
3. 选择 **Deploy from a branch**。
4. 选择 `main` 和 `/root`。
5. 保存并等待公开网址生成。

#### Netlify 或其他静态托管

上传整个项目目录即可。无需构建命令，发布目录为项目根目录。

### 无障碍与兼容性

- 使用语义化区域和 ARIA 标签页关系。
- 支持方向键、Home 和 End 导航。
- 使用 ARIA 表达选中、展开和按下状态。
- 提供可见键盘焦点。
- 栏目状态使用 `aria-live="polite"`。
- 适当隐藏装饰性图形。
- 支持减少动态效果偏好。
- 原生 `<dialog>` 提供模态焦点行为。

由于使用了 `<dialog>`、`color-mix()`、Grid、自定义属性和 `dvh`，建议使用最新版 Chrome、Edge、Firefox 或 Safari。

---

## License / 许可

No license file is currently included. Unless a license is added, copyright remains with the project owner.

项目当前未包含许可证文件。在正式添加许可证之前，项目版权归项目所有者保留。
