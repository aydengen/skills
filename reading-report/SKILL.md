---
name: reading-report
description: "生成英语阅读能力评估报告（Star Reading）。用于将测试数据转换为精美的 A4 排版 HTML 报告，支持浏览器打印为 PDF。重点解决分页时内容截断问题。"
---

# 阅读报告生成技能

将 Star Reading 测试数据转换为精美的 A4 排版 HTML 报告，用户可在浏览器中打印为 PDF。

## 核心原则

1. **原文完整保留**：用户提供的所有测试数据和文字内容必须 100% 保留
2. **防止分页截断**：通过 CSS 分页控制，确保内容块不会在打印时被切断
3. **清晰的视觉层次**：分数卡片、优势/待加强区块、表格等结构化呈现

## 分页控制规则（核心）

### 必须使用的 CSS 规则

在 `@media print` 中添加以下规则，防止内容被截断：

```css
@media print {
  /* 防止元素内部被分页截断 */
  .score-card,
  .score-row,
  .score-row-2,
  .strength-box,
  .summary-box,
  .suggestions li,
  p,
  tr {
    break-inside: avoid;
    page-break-inside: avoid;
  }

  /* 防止标题与后续内容分离 */
  h1, h2, h3, h4 {
    break-after: avoid;
    page-break-after: avoid;
  }

  /* 表格整体不分页（如果表格不大） */
  table {
    break-inside: avoid;
    page-break-inside: avoid;
  }
}
```

### 手动分页控制

当需要强制从新页开始时，使用：

```css
.new-page {
  page-break-before: always;
  break-before: page;
}
```

或直接在元素上添加 inline style：

```html
<h2 style="page-break-before: always;">四、横向对比</h2>
```

### 分页决策原则

1. **第一页**：总体概览 + 核心分数 + 优势/待加强
2. **第二页**：推荐阅读级别 + 学习建议
3. 如果内容溢出，优先在「章节标题」前分页，而不是在段落中间

## 报告结构

### 固定结构

1. **报告标题**：学生英语阅读能力评估报告（Star Reading测试结果）
2. **学生姓名**
3. **一、总体水平概览**：总结性评价 + 测试说明备注
4. **二、核心测试分数解读**：GE、IRL、Est. ORF、PR、Lexile 等分数卡片
5. **三、整体阅读优势与待加强**：优势区块 + 待加强区块
6. **四、横向对比：推荐阅读级别**：书籍推荐表格
7. **五、学习建议**：具体可执行的建议列表

### 分数卡片说明

| 指标 | 全称 | 含义 |
|------|------|------|
| GE | Grade Equivalent | 年级等效值，如 3.1 = 三年级第1个月 |
| IRL | Instructional Reading Level | 教学阅读等级，最有效的教学材料难度 |
| Est. ORF | Estimated Oral Reading Fluency | 预估口语流利度（词/分钟） |
| PR | Percentile Rank | 百分位排名 |
| Lexile | Lexile Measure | 蓝思值 |
| ZPD | Zone of Proximal Development | 最近发展区（可选） |

## 模板文件

使用 `assets/template.html` 作为基础模板。模板已包含：
- A4 页面尺寸设置
- 分数卡片样式
- 优势/待加强区块样式
- 表格样式
- **完整的分页控制 CSS**

## 使用流程

1. 用户提供测试数据（文字或截图）
2. 识别学生姓名、各项分数、优势、待加强、推荐书目、学习建议
3. 使用模板生成 HTML
4. 根据内容量决定分页点
5. 输出完整 HTML 文件

## 输出格式

输出完整的 HTML 文件，用户可以：
1. 保存为 `.html` 文件
2. 在浏览器中打开
3. 使用 `Ctrl+P` / `Cmd+P` 打印为 PDF
4. 打印设置选择「A4」、「无边距」
