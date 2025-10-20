---
name: web-calculator
status: backlog
created: 2025-10-20T13:38:56Z
updated: 2025-10-20T14:01:27Z
progress: 0%
prd: .claude/prds/web-calculator.md
github: https://github.com/womendoushihaoyin/web-calculator/issues/1
---

# Epic: web-calculator

## Overview

web-calculator 是一个纯前端静态计算器应用,采用原生 HTML + CSS + JavaScript 技术栈实现。核心设计理念是"极简主义",通过单文件 HTML 架构提供流畅的四则运算体验,无需任何外部依赖或构建工具。

**技术亮点**:
- 单文件部署 (index.html 包含所有代码)
- 零依赖,无需 npm/webpack/babel
- 响应式 CSS Grid 布局,移动端优先设计
- 事件驱动架构,支持键盘和鼠标双输入
- 计算精度控制,避免 JavaScript 浮点数陷阱

## Architecture Decisions

### 技术选型

**核心决策: 原生技术栈 (Vanilla JS)**
- **理由**: PRD 明确要求轻量化 (<100KB),引入框架反而增加复杂度
- **优势**: 零依赖、快速加载、易于维护、无构建步骤
- **权衡**: 放弃组件化抽象,但项目规模小不需要

**架构模式: MVC 轻量变体**
- **Model (计算器状态)**:
  - 当前显示值 (currentValue)
  - 上一个操作数 (previousValue)
  - 当前运算符 (operator)
  - 等待新输入标志 (waitingForNewValue)

- **View (DOM 操作)**:
  - 显示区更新函数
  - 按钮视觉反馈

- **Controller (事件处理)**:
  - 按钮点击事件
  - 键盘输入事件

**浮点数精度处理策略**:
- 使用 `toFixed(6)` 限制小数精度
- 大数自动转科学计数法
- 避免直接比较浮点数

**响应式设计方案**:
- CSS Grid 作为主布局系统
- 媒体查询断点: 480px (手机), 768px (平板)
- 移动端优先,逐步增强桌面体验

## Technical Approach

### 文件结构

```
web-calculator/
├── index.html          (单文件包含所有代码)
├── README.md           (项目说明文档)
└── .gitignore          (版本控制配置)
```

**单文件架构设计**:
```html
<!DOCTYPE html>
<html>
<head>
  <style>
    /* 嵌入式 CSS */
    /* 1. CSS Reset */
    /* 2. 计算器容器布局 */
    /* 3. 显示区样式 */
    /* 4. 按钮网格布局 */
    /* 5. 响应式媒体查询 */
  </style>
</head>
<body>
  <!-- 计算器 DOM 结构 -->
  <div class="calculator">
    <div class="display" id="display">0</div>
    <div class="buttons">
      <!-- 数字按钮 0-9 -->
      <!-- 运算符按钮 +、-、×、÷ -->
      <!-- 功能按钮 C、DEL、= -->
    </div>
  </div>

  <script>
    // 嵌入式 JavaScript
    // 1. 状态管理对象
    // 2. 计算逻辑函数
    // 3. DOM 操作函数
    // 4. 事件监听器
  </script>
</body>
</html>
```

### 核心功能实现

#### 1. 计算器状态管理

```javascript
const calculator = {
  currentValue: '0',        // 显示区当前值
  previousValue: null,      // 上一个操作数
  operator: null,           // 当前运算符 (+, -, ×, ÷)
  waitingForNewValue: false // 等待新输入标志
};
```

#### 2. 数字输入处理

**逻辑流程**:
1. 检查是否等待新输入 → 替换当前值
2. 处理小数点逻辑 → 一个数字只能有一个小数点
3. 限制最大位数 → 防止显示溢出

**关键代码结构**:
```javascript
function inputDigit(digit) {
  if (calculator.waitingForNewValue) {
    calculator.currentValue = String(digit);
    calculator.waitingForNewValue = false;
  } else {
    calculator.currentValue =
      calculator.currentValue === '0'
        ? String(digit)
        : calculator.currentValue + digit;
  }
  updateDisplay();
}

function inputDecimal() {
  if (!calculator.currentValue.includes('.')) {
    calculator.currentValue += '.';
  }
  updateDisplay();
}
```

#### 3. 四则运算逻辑

**计算核心函数**:
```javascript
function performCalculation() {
  const prev = parseFloat(calculator.previousValue);
  const current = parseFloat(calculator.currentValue);

  let result;
  switch(calculator.operator) {
    case '+': result = prev + current; break;
    case '-': result = prev - current; break;
    case '×': result = prev * current; break;
    case '÷':
      if (current === 0) {
        alert('除数不能为零');
        resetCalculator();
        return;
      }
      result = prev / current;
      break;
  }

  // 精度处理
  calculator.currentValue = parseFloat(result.toFixed(6)).toString();
  calculator.operator = null;
  calculator.previousValue = null;
}
```

#### 4. 键盘事件映射

**键盘支持方案**:
```javascript
const keyMap = {
  '0'-'9': inputDigit,
  '.': inputDecimal,
  '+': () => setOperator('+'),
  '-': () => setOperator('-'),
  '*': () => setOperator('×'),
  '/': () => setOperator('÷'),
  'Enter': calculate,
  '=': calculate,
  'Backspace': deleteLastDigit,
  'Escape': resetCalculator,
  'c': resetCalculator
};

document.addEventListener('keydown', (e) => {
  if (keyMap[e.key]) {
    e.preventDefault();
    keyMap[e.key]();
  }
});
```

### 用户界面设计

#### 布局策略 (CSS Grid)

```css
.calculator {
  display: grid;
  grid-template-rows: minmax(80px, auto) 1fr;
  max-width: 400px;
  margin: 50px auto;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
}

.buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1px;
  background: #ddd;
}

/* 移动端适配 */
@media (max-width: 480px) {
  .calculator {
    max-width: 100%;
    margin: 0;
    border-radius: 0;
  }

  button {
    font-size: 1.2rem;
    padding: 20px;
  }
}
```

#### 交互反馈

**按钮状态样式**:
```css
button {
  padding: 20px;
  border: none;
  background: #fff;
  cursor: pointer;
  transition: all 0.2s;
}

button:hover {
  background: #f0f0f0;
}

button:active {
  transform: scale(0.95);
  background: #e0e0e0;
}

/* 运算符按钮高亮 */
button.operator {
  background: #ff9500;
  color: white;
}

button.operator:hover {
  background: #ff7f00;
}
```

### 错误处理与边界情况

**处理场景**:
1. **除零错误**: 弹窗提示 + 重置状态
2. **大数显示**: 超过 15 位自动转科学计数法
3. **重复小数点**: 忽略后续输入
4. **空输入退格**: 保持当前状态
5. **连续运算符**: 替换为最新运算符

## Implementation Strategy

### 开发阶段

**Phase 1: 静态页面搭建 (2 天)**
- HTML 结构定义
- CSS 样式完成 (包括响应式)
- 按钮布局和视觉效果

**Phase 2: 核心逻辑开发 (3 天)**
- 数字输入功能
- 四则运算实现
- 清除和退格功能
- 错误处理逻辑

**Phase 3: 键盘支持 (1 天)**
- 键盘事件监听
- 快捷键映射
- 焦点状态管理

**Phase 4: 测试与优化 (2 天)**
- 跨浏览器测试
- 移动端测试
- 性能优化
- Bug 修复

### 测试策略

**手工测试用例**:
- 基础运算: `123 + 456 = 579`
- 小数运算: `12.5 × 4 = 50`
- 连续运算: `100 - 20 + 30 = 110`
- 除零测试: `10 ÷ 0` → 错误提示
- 负数测试: `5 - 10 = -5`
- 清除测试: `123` → `C` → `0`
- 退格测试: `1234` → `DEL` → `123`

**浏览器兼容性测试**:
- Chrome (最新版)
- Firefox (最新版)
- Safari (macOS + iOS)
- Edge (最新版)

**移动端测试**:
- iOS Safari (iPhone)
- Android Chrome (各品牌手机)
- 横竖屏切换
- 触摸响应

### 风险控制

**潜在风险**:
1. **JavaScript 浮点数精度问题**
   - 缓解: 使用 `toFixed(6)` 和 `parseFloat` 组合

2. **移动端按钮误触**
   - 缓解: 按钮最小尺寸 44×44px,间距 1px

3. **键盘事件冲突**
   - 缓解: `e.preventDefault()` 阻止默认行为

4. **大数显示溢出**
   - 缓解: CSS `overflow-x: auto`,超长自动滚动

## Task Breakdown Preview

基于上述技术方案,将创建以下高层任务类别:

- [ ] **T1: 项目初始化** - 创建基础文件结构和 Git 仓库
- [ ] **T2: HTML 结构开发** - 定义计算器 DOM 结构
- [ ] **T3: CSS 样式实现** - 完成布局、主题和响应式设计
- [ ] **T4: 核心 JavaScript 逻辑** - 实现计算器状态管理和运算逻辑
- [ ] **T5: 键盘支持** - 添加键盘事件监听和快捷键
- [ ] **T6: 错误处理** - 实现除零检测和边界条件处理
- [ ] **T7: 跨浏览器测试** - 在主流浏览器中验证功能
- [ ] **T8: 移动端优化** - 适配移动设备并测试触摸交互
- [ ] **T9: 文档编写** - 创建 README 和代码注释
- [ ] **T10: 部署发布** - 部署到静态托管平台

## Dependencies

### 技术依赖
- **无外部库依赖**: 完全使用浏览器原生 API
- **浏览器要求**: 支持 ES6+ (现代浏览器)
- **开发工具**: 任何代码编辑器 + 浏览器开发者工具

### 部署依赖
- **静态托管平台**: GitHub Pages / Vercel / Netlify (任选其一)
- **域名**: 可选,可使用平台提供的免费子域名

### 团队依赖
- **无**: 单人即可完成开发 (1-2 名前端工程师)

## Success Criteria (Technical)

### 功能完整性
- [ ] 支持加减乘除四则运算
- [ ] 支持小数运算 (精度 6 位)
- [ ] 支持负数运算
- [ ] 除零错误正确处理
- [ ] 清除和退格功能正常
- [ ] 键盘快捷键全部工作

### 性能基准
- [ ] 首次页面加载时间 < 1 秒
- [ ] 文件总大小 < 100KB (实际预计 < 20KB)
- [ ] 按钮响应时间 < 50ms
- [ ] 移动端运行流畅 (无明显卡顿)

### 质量标准
- [ ] 通过所有手工测试用例
- [ ] 主流浏览器 (Chrome/Firefox/Safari/Edge) 100% 兼容
- [ ] 移动端触摸交互友好
- [ ] 代码通过 ESLint 检查 (可选)
- [ ] 无控制台错误或警告

### 用户体验
- [ ] 界面简洁清晰,符合通用计算器习惯
- [ ] 按钮有明确的悬停和点击反馈
- [ ] 错误提示友好易懂
- [ ] 响应式布局适配所有屏幕尺寸

## Estimated Effort

### 总体时间线
- **开发周期**: 1-2 周 (按 1 人全职计算)
- **关键里程碑**:
  - Day 1-2: 静态页面完成
  - Day 3-5: JavaScript 逻辑完成
  - Day 6: 键盘支持完成
  - Day 7-8: 测试和优化
  - Day 9-10: 文档和部署

### 资源需求
- **开发人员**: 1 名前端工程师
- **设计师**: 可选 (使用简约默认样式)
- **测试人员**: 可选 (开发者自测)

### 关键路径
1. HTML/CSS 结构 → 2. 核心 JS 逻辑 → 3. 测试 → 4. 部署

**备注**: 由于项目复杂度低,无技术难点,预计可在 1 周内完成 MVP。


## Tasks Created

- [ ] #2 - 项目初始化 (parallel: true)
- [ ] #3 - HTML 结构开发 (parallel: true)
- [ ] #4 - CSS 样式实现 (parallel: true)
- [ ] #5 - 核心 JavaScript 逻辑 (parallel: false)
- [ ] #6 - 键盘支持 (parallel: true)
- [ ] #7 - 错误处理与边界条件 (parallel: true)
- [ ] #8 - 跨浏览器兼容性测试 (parallel: true)
- [ ] #9 - 移动端优化与测试 (parallel: true)
- [ ] #10 - 文档编写 (parallel: true)
- [ ] #11 - 部署发布 (parallel: false)

**任务统计**:
- **总任务数**: 10 个
- **并行任务**: 7 个 (002, 003, 004, 006, 007, 008, 009, 010)
- **串行任务**: 3 个 (005, 011)
- **总预估工作量**: 19.5-28.5 小时 (约 2.5-3.5 个工作日)
