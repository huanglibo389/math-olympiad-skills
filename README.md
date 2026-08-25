# Math Olympiad Skills

一套可移植的数学竞赛工作流，面向 Codex、Claude Code、OpenCode 及其他兼容 `SKILL.md` 的本地智能体。项目把单题证明、证据检索和内容交付分成三个边界清晰、可以协同调用的技能。

## 三个技能

| 技能 | 适用任务 | 主要产物 |
| --- | --- | --- |
| `math-olympiad` | 独立求解、学生证明审查、补全证明缺口 | 严谨证明、审查结论、最小修补 |
| `math-olympiad-search` | 在已配置的本地题库、Obsidian 与知识库中查找原题、官方解答和同类方法 | 带出处的证据包，不替代证明验证 |
| `math-content-pipeline` | OCR、批量题库、答案配对、Obsidian、讲义、TeX/PDF 生产 | 可审计的内容与交付物 |

默认路由很简单：

1. 单道题的数学核心交给 `math-olympiad`。
2. 需要出处或本地同类资料时，再调用 `math-olympiad-search`。
3. 涉及多题、OCR、Markdown、讲义或 PDF 时，由 `math-content-pipeline` 统筹，并把非平凡证明交回 `math-olympiad`。

## 安装

最直接的方式是把三个技能目录链接到智能体的技能目录：

```bash
git clone https://github.com/huanglibo389/math-olympiad-skills.git
cd math-olympiad-skills

SKILLS_HOME="${CODEX_HOME:-$HOME/.codex}/skills"
mkdir -p "$SKILLS_HOME"

for skill in math-olympiad math-olympiad-search math-content-pipeline; do
  ln -sfn "$PWD/skills/$skill" "$SKILLS_HOME/$skill"
done
```

也可以把对应目录复制到智能体支持的技能目录。不同客户端的安装位置可能不同，详见 [安装说明](docs/installation.md)。

## 调用

显式调用最稳定：

```text
$math-olympiad 严格审查这份学生证明，指出第一处无效推理并给出最小修补。
```

```text
$math-olympiad-search 只查本地资料，定位这道题的原题和官方解答，不做网络搜索。
```

```text
$math-content-pipeline 把这份试卷整理成 Obsidian Markdown；每题题干后紧跟可折叠解答，并逐题核验。
```

更多示例见 [调用说明](docs/invocation.md) 和 [`examples/`](examples/)。

## 证据策略

每次检索任务都应明确采用以下一种策略：

- `offline`：只独立推理，不读取任何外部题库或网络资料。
- `local-only`：只读本机已配置的知识库和题库。
- `web-allowed`：本地证据不足时允许查阅网络，并明确标注来源。

检索结果只是证据，不是证明认证。最终数学闭合仍由 `math-olympiad` 完成。

## 本地配置

仓库不包含个人题库、学生材料、扫描件或私有绝对路径。需要本地检索时：

1. 复制 `config/local-paths.example.yaml` 为 `config/local-paths.yaml`。
2. 填入自己的本地目录。
3. 保留该文件在本地；它已经被 `.gitignore` 忽略。

也可以使用文档中列出的环境变量。未配置本地语料时，技能会明确说明“本地检索不可用”，而不会猜测目录或伪造来源。

## 设计原则

- 题干、证明、来源、格式与渲染分别核验。
- 区分“答案正确”“主线正确”和“原稿完整正确”。
- OCR 成功、TeX 编译成功或 PDF 生成成功都不等于数学证明正确。
- 不把私有题库、学生信息、访问令牌或未经授权的扫描资料提交到仓库。
- 对等号、边界、退化情形、除零条件和逆命题进行显式检查。

架构与角色边界见 [架构说明](docs/architecture.md)。

## 许可

代码与技能说明以 [MIT License](LICENSE) 发布。使用者仍需自行确认题目、解答、图片和扫描材料的版权与隐私权限。
