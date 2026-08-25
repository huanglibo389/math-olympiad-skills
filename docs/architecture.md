# 架构说明

## 分层

```text
输入材料
  -> math-content-pipeline：提取、拆题、配对、状态管理
      -> math-olympiad-search：可选的本地证据检索
      -> math-olympiad：证明求解与严格审查
  -> math-content-pipeline：Obsidian / TeX / PDF 交付与渲染检查
```

## 证据层

一次“完成”至少可能包含五种彼此独立的状态：

1. **捕获状态**：题干与手写内容是否准确识别。
2. **来源状态**：原题、官方解答或参考资料是否匹配。
3. **数学状态**：证明是否逐步闭合。
4. **格式状态**：题干、解答、编号和 callout 是否正确组织。
5. **渲染状态**：Markdown、TeX 或 PDF 是否实际显示正确。

其中任何一层通过，都不能自动推出其余层通过。

## 协作边界

- `math-olympiad-search` 返回证据包，不宣布证明正确。
- `math-olympiad` 可以独立证明，也可以审查检索到的证明，但不负责批量 OCR 与出版流程。
- `math-content-pipeline` 维护逐题状态并组织交付，不用“编译成功”替代数学验证。
