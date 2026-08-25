# 调用说明

## 严格求解

```text
$math-olympiad 断网独立求解这道数论题。给出完整题干、明确答案、竞赛标准证明，并检查所有边界情形。
```

## 学生证明审查

```text
$math-olympiad 检查学生证明。分别判断答案、主线和原稿是否正确；指出第一处失效推理，并给出不改变主线的最小修补。
```

## 本地出处检索

```text
$math-olympiad-search 采用 local-only，先查方法与知识点，再查同类题和原题。输出精确文件路径、命中片段和可验证的出处信息。
```

## 内容流水线

```text
$math-content-pipeline 把这批图片整理为 Obsidian Markdown。每题保留完整题干，解答使用可折叠 callout；逐题标注 OCR、来源、证明和格式状态。
```

## 联合调用

```text
$math-content-pipeline 负责 OCR 和排版；所有非平凡证明交给 $math-olympiad 审查；只有我允许时才由 $math-olympiad-search 查本地资料。
```

自然语言也可能触发技能，但显式写出技能名、证据策略和期望产物最稳定。
