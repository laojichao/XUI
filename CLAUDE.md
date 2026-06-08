# CLAUDE.md

## 项目描述

XUI 是一个简洁优雅的 Android 原生 UI 框架，提供一整套统一风格的 UI 组件解决方案。项目库整体大小不足 1M（打包后约 644k），最低兼容 Android API 17。

作者：xuexiangjys  
GitHub：https://github.com/xuexiangjys/XUI  
当前版本：1.2.1（androidx）

## 技术栈

- 语言：Java
- 最低 SDK：API 17（minSdkVersion 17）
- 目标 SDK：API 29（targetSdkVersion 29）
- 构建工具版本：29.0.3
- Gradle 插件：3.6.1
- AndroidX 支持（已启用 Jetifier）
- 字体注入：Calligraphy3 + ViewPump 2.0.3
- 图片加载：Glide 4.11+
- 注解处理：ButterKnife 10.1.0（仅 demo 使用）

## 项目结构

```
XUI/
├── app/                    # Demo 应用模块（com.xuexiang.xuidemo）
├── xui_lib/                # 核心 UI 库模块
│   └── src/main/java/com/xuexiang/xui/
│       ├── XUI.java             # 全局初始化入口
│       ├── UIConfig.java        # UI 配置
│       ├── UIConsts.java        # 常量定义（含屏幕类型）
│       ├── XUIInitProvider.java # ContentProvider 自动初始化
│       ├── adapter/             # 适配器基类
│       ├── logs/                # 日志工具
│       ├── utils/               # 工具类集合
│       └── widget/              # UI 组件库
│           ├── actionbar/       # 自定义 ActionBar
│           ├── activity/        # Activity 相关
│           ├── alpha/           # 透明度控件
│           ├── banner/          # 轮播图
│           ├── behavior/        # 协调行为
│           ├── button/          # 按钮组件
│           ├── dialog/          # 对话框
│           ├── edittext/        # 输入框
│           ├── flowlayout/      # 流式布局
│           ├── grouplist/       # 列表分组
│           ├── guidview/        # 引导视图
│           ├── imageview/       # 图片控件
│           ├── layout/          # 布局组件
│           ├── picker/          # 选择器
│           ├── popupwindow/     # 弹出窗口
│           ├── progress/        # 进度条
│           ├── searchview/      # 搜索栏
│           ├── shadow/          # 阴影效果
│           ├── slideback/       # 侧滑返回
│           ├── spinner/         # 下拉选择
│           ├── statelayout/     # 状态布局（加载中/空/错误）
│           ├── tabbar/          # 底部导航栏
│           ├── textview/        # 文本控件
│           └── toast/           # Toast 提示
├── widget_compiler/        # 注解处理器模块
├── docs/                   # 文档资源
├── art/                    # 截图和资源图片
├── apk/                    # Demo APK 输出目录
├── build.gradle            # 根构建脚本
├── versions.gradle         # 统一版本管理
├── settings.gradle         # 模块配置（app, xui_lib, widget_compiler）
└── gradle.properties       # Gradle 配置
```

## 构建说明

### 环境要求

- Android Studio 3.6+
- Gradle 5.6.4+
- JDK 1.8

### 构建命令

```bash
# Debug 构建
./gradlew assembleDebug

# Release 构建（需要配置签名）
./gradlew assembleRelease

# 安装 Demo 到设备
./gradlew installDebug
```

### 签名配置

签名文件位于 `keystores/android.keystore`，密码为 `xuexiang`。仅在 `isNeedPackage=true` 时启用。

### 发布到 JitPack

库通过 JitPack 发布，配置见 `JitPackUpload.gradle`。

## 关键 API 用法

### 1. 初始化（必须）

在 Application 中初始化：

```java
XUI.init(this);
```

### 2. 主题设置（必须）

所有使用 XUI 组件的窗口必须继承 XUITheme 主题：

```xml
<!-- 手机（默认） -->
<style name="AppTheme" parent="XUITheme.Phone">
    <item name="colorPrimary">@color/colorPrimary</item>
    <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
    <item name="colorAccent">@color/colorAccent</item>
</style>

<!-- 小平板（7英寸） -->
<style name="AppTheme" parent="XUITheme.Tablet.Small">

<!-- 大平板（10英寸） -->
<style name="AppTheme" parent="XUITheme.Tablet.Big">
```

或在 Activity 中动态设置：

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    XUI.initTheme(this);
    super.onCreate(savedInstanceState);
}
```

### 3. 字体配置（可选）

```java
// 设置自定义字体路径（assets 目录下）
XUI.initFontStyle("fonts/custom.ttf");

// 在 Base Activity 中注入字体
@Override
protected void attachBaseContext(Context newBase) {
    super.attachBaseContext(ViewPumpContextWrapper.wrap(newBase));
}
```

### 4. 调试模式

```java
XUI.debug("XUI_TAG");  // 开启日志
XUI.debug(true);        // 开启调试模式
```

### 5. 常用工具类

| 工具类 | 功能 |
|--------|------|
| `DensityUtils` | dp/sp/px 转换 |
| `StatusBarUtils` | 状态栏沉浸式 |
| `KeyboardUtils` | 软键盘控制 |
| `SpanUtils` | 富文本构建 |
| `ColorUtils` | 颜色处理 |
| `DrawableUtils` | Drawable 操作 |
| `DeviceUtils` | 设备信息获取 |
| `ThemeUtils` | 主题属性获取 |
| `ViewUtils` | View 工具方法 |
| `XToastUtils` | Toast 快捷调用 |

### 6. 核心组件概览

- **对话框**：`com.xuexiang.xui.widget.dialog` - Material 风格对话框
- **选择器**：`com.xuexiang.xui.widget.picker` - 日期/时间/城市选择
- **流式布局**：`com.xuexiang.xui.widget.flowlayout` - 标签流式排列
- **状态布局**：`com.xuexiang.xui.widget.statelayout` - 加载中/空数据/错误状态切换
- **轮播图**：`com.xuexiang.xui.widget.banner` - 图片轮播
- **搜索栏**：`com.xuexiang.xui.widget.searchview` - 搜索组件
- **TabBar**：`com.xuexiang.xui.widget.tabbar` - 底部导航
- **下拉选择**：`com.xuexiang.xui.widget.spinner` - MaterialSpinner

## 逆向分析要点

### Hook 目标

1. **XUI.init()** - 全局初始化入口，Application.onCreate 中调用，可用于确认 XUI 版本
2. **XUI.initTheme()** - 主题初始化，Activity.onCreate 中调用
3. **XUI.initFontStyle()** - 字体加载，可拦截自定义字体路径
4. **XUITheme 系列** - 主题样式定义，位于 `res/values/styles.xml`

### 常见检测点

- XUI 自身无 Root/模拟器检测逻辑
- Demo 中使用了 XAOP（切面编程）和 XRouter（路由），Hook 时需注意
- 字体注入通过 ViewPump 拦截器实现，可 Hook `CalligraphyInterceptor` 拦截字体加载

### 依赖库 Hook 注意

- Glide 图片加载：Hook `Glide.with()` 可拦截图片请求
- AgentWeb WebView：Hook `AgentWeb` 相关类可拦截 WebView 行为
- SmartRefreshLayout：下拉刷新逻辑，Hook `RefreshLayout` 系列类

## 注意事项

1. 项目使用 AndroidX，不再维护 support 版本（最后一个 support 版本为 1.0.9）
2. 库本身体积很小（约 644k），但 Demo 集成了大量第三方库（约 18M）
3. 代码规范遵循阿里巴巴 Java 编码规范
4. 贡献代码请提交到 dev 分支，不要直接提交到 master
