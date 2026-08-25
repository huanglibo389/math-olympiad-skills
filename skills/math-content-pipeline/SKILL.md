---
name: math-content-pipeline
description: 统筹数学内容提取、OCR、拆题、答案配对、题库、Obsidian、讲义、TeX 与 PDF 交付。用于多题或产物型任务；非平凡证明交给 math-olympiad，原题与本地证据检索交给 math-olympiad-search。转换或编译成功不等于数学认证。
---

# 数学内容流水线

## 角色边界

- `math-content-pipeline`：拆解任务、维护逐题状态、组织格式、生成并检查交付物。
- `math-olympiad`：求解或严格审查非平凡数学证明。
- `math-olympiad-search`：在允许的证据策略下查找本地来源和参考解法。

不要让一个阶段的成功替代另一个阶段：OCR 可读、文件已保存、TeX 已编译、PDF 已打开，都不等于题干无误或证明正确。

## 状态机

对每道题分别记录：

1. `captured`：题干和现有解答已准确转写。
2. `source-matched`：来源已确认，或明确标记未确认。
3. `proof-verified`：证明已独立审查并闭合。
4. `formatted`：题干、解答、编号、callout 和分页符合要求。
5. `rendered`：最终阅读视图或 PDF 已实际检查。

只有用户要求的全部状态都完成，才能宣布交付完成。

## 路由

### 单题或少量题

1. 固定题干。
2. 将证明核心交给 `math-olympiad`。
3. 需要来源时，按 `offline`、`local-only` 或 `web-allowed` 调用 `math-olympiad-search`。
4. 整理为用户要求的 Markdown、TeX 或 PDF。
5. 重新打开最终产物核对。

### 批量材料

1. 建立逐题清单和稳定编号。
2. 分离题干、学生作答、参考答案、图片和元数据。
3. 逐题配对，不把“答案页顺序相近”当作已经对应。
4. 对非平凡证明逐题调用 `math-olympiad`；不得只抽查后声称全部正确。
5. 汇总状态，再统一排版和渲染。

## 本地资源配置

优先读取仓库附近的 `config/local-paths.yaml`，其次读取环境变量：

| 用途 | 环境变量 |
| --- | --- |
| Obsidian 题库 | `MATH_CONTENT_OBSIDIAN_VAULT` |
| 综合竞赛知识库 | `MATH_CONTENT_MATH_COMPETITION_WIKI` |
| 数论知识库 | `MATH_CONTENT_NUMBER_THEORY_WIKI` |
| MathNet 本地资料 | `MATH_CONTENT_MATHNET_VAULT` |
| 训练数据 | `MATH_CONTENT_TRAINING_DATA` |
| 视频转文稿工具 | `MATH_CONTENT_VIDEO2MD` |

未配置时只说明相关本地能力不可用，不得猜测路径。

## 内容标准

### 题干

- 保留量词、范围、编号、图形关系和符号定义。
- OCR 不确定处必须标记并回看原图。
- 来源标签与题干准确性分别核验。

### 解答

- 给出明确结论，不把“思路”冒充完整证明。
- 写明定义域、非零条件、整除桥梁、边界、等号和退化情形。
- 审查学生证明时区分答案正确、主线正确和原稿完整正确。

### Obsidian

可折叠解答使用：

```markdown
> [!solution]- 解答
> 这里写完整证明。
```

每道解答必须紧跟对应题干。思维导图优先使用简洁的 `flowchart TB`，并在实际阅读视图中检查字号与层次。

### TeX 与 PDF

- 中文材料优先使用 XeLaTeX。
- 编译后检查题号、公式、分页、图片、页眉页脚和字体。
- 生成 PDF 后实际渲染或打开核对；只看到输出文件不算视觉验收。

## 最终报告

```markdown
## 交付物
- 文件：...
- 题目数：...

## 状态
- 捕获：通过 / 有待确认
- 来源：通过 / 未确认 / 不适用
- 数学：逐题通过 / 存在问题
- 格式：通过 / 存在问题
- 渲染：通过 / 未检查

## 剩余风险
- ...
```

不得隐藏未验证项，也不得用“已生成”代替“已检查”。
