# Web Calculator - 网页计算器

![Calculator Screenshot](screenshot.png) <!-- 可选:添加截图 -->

## 项目简介

Web Calculator 是一个简洁、快速、易用的网页四则运算计算器。采用纯前端技术实现,无需安装,打开浏览器即可使用。

**核心特性**:
- ✅ 简洁的用户界面,符合通用计算器习惯
- ✅ 支持加减乘除四则运算
- ✅ 支持小数运算,精度 6 位
- ✅ 支持键盘快捷键操作
- ✅ 响应式设计,完美适配桌面和移动设备
- ✅ 零依赖,单文件部署,加载速度快 (< 1 秒)

## 在线演示

🔗 [点击这里访问在线演示](https://womendoushihaoyin.github.io/web-calculator/)

## 功能特性

### 基础运算
- 加法 (+)
- 减法 (-)
- 乘法 (×)
- 除法 (÷)

### 辅助功能
- 清除 (C): 清空所有输入,重置计算器
- 退格 (DEL): 删除最后一位输入
- 小数点 (.): 输入小数
- 等号 (=): 计算结果

### 键盘快捷键
| 按键 | 功能 |
|------|------|
| 0-9 | 输入数字 |
| + - * / | 运算符 |
| Enter 或 = | 计算结果 |
| Backspace | 退格 |
| Escape 或 C | 清除 |
| . | 小数点 |

## 技术栈

- **HTML5**: 语义化标签,单文件架构
- **CSS3**: CSS Grid 布局,响应式设计
- **JavaScript (ES6+)**: 原生 JavaScript,无框架依赖

**浏览器兼容性**:
- Chrome (最新 2 个版本)
- Firefox (最新 2 个版本)
- Safari (最新 2 个版本)
- Edge (最新 2 个版本)

## 快速开始

### 本地运行

1. 克隆仓库:
```bash
git clone https://github.com/womendoushihaoyin/web-calculator.git
cd web-calculator
```

2. 打开 `index.html`:
```bash
# macOS
open index.html

# Windows
start index.html

# Linux
xdg-open index.html
```

或直接在浏览器中打开 `index.html` 文件即可。

### 部署到 GitHub Pages

1. Fork 本仓库
2. 进入仓库设置 (Settings)
3. 找到 Pages 选项
4. 选择 `main` 分支作为源
5. 保存后访问 `https://your-username.github.io/web-calculator/`

## 项目结构

```
web-calculator/
├── index.html          # 主应用文件 (包含 HTML/CSS/JavaScript)
├── README.md           # 项目说明文档
├── LICENSE             # 许可证文件 (MIT)
└── .gitignore          # Git 忽略配置
```

## 开发指南

### 代码结构

**HTML 结构**:
- `.calculator`: 计算器容器
- `.display`: 显示区
- `.buttons`: 按钮网格容器
- `button`: 按钮元素 (使用 data-* 属性)

**CSS 样式**:
- 基础样式重置
- CSS Grid 布局
- 响应式媒体查询 (480px, 768px)
- 按钮交互效果 (hover, active)

**JavaScript 逻辑**:
- 状态管理对象 `calculator`
- 核心函数: `inputDigit`, `setOperator`, `performCalculation`
- 事件处理: 按钮点击委托,键盘事件监听

### 开发规范

- 代码注释使用中文
- 函数命名使用驼峰式 (camelCase)
- CSS 类名使用连字符 (kebab-case)
- 遵循 DRY (Don't Repeat Yourself) 原则

## 测试

### 手工测试用例

- `123 + 456 = 579` (基础加法)
- `12.5 × 4 = 50` (小数乘法)
- `100 - 20 + 30 = 110` (连续运算)
- `5 - 10 = -5` (负数)
- `10 ÷ 0` → 错误提示 (除零)

### 浏览器测试

在以下浏览器中测试功能和显示效果:
- Chrome / Edge (Chromium)
- Firefox
- Safari (macOS / iOS)

## 许可证

本项目采用 [MIT License](LICENSE) 开源许可证。

## 贡献

欢迎提交 Issue 和 Pull Request!

## 联系方式

- 作者: womendoushihaoyin
- GitHub: [@womendoushihaoyin](https://github.com/womendoushihaoyin)

---

**开发时间**: 2025-10-20
**版本**: v1.0.0
