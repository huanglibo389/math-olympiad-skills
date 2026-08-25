# 安装说明

## Codex

把仓库中的三个技能目录链接或复制到 `${CODEX_HOME:-$HOME/.codex}/skills/`：

```bash
SKILLS_HOME="${CODEX_HOME:-$HOME/.codex}/skills"
mkdir -p "$SKILLS_HOME"

for skill in math-olympiad math-olympiad-search math-content-pipeline; do
  ln -sfn "$PWD/skills/$skill" "$SKILLS_HOME/$skill"
done
```

重新打开会话后，可通过 `$math-olympiad` 等名称显式调用。

## 其他智能体

对于支持 `SKILL.md` 的客户端，把 `skills/<name>/` 整个目录放入其技能搜索目录。若客户端不自动发现技能，可在项目级说明文件中写明三个技能的路径与路由规则。

## 本地语料配置

```bash
cp config/local-paths.example.yaml config/local-paths.yaml
```

编辑本地文件后不要提交。也可以设置以下环境变量：

```text
MATH_OLYMPIAD_GENERAL_WIKI
MATH_OLYMPIAD_REFERENCES
MATH_OLYMPIAD_COMBINATORICS_WIKI
MATH_CONTENT_OBSIDIAN_VAULT
MATH_CONTENT_MATH_COMPETITION_WIKI
MATH_CONTENT_NUMBER_THEORY_WIKI
MATH_CONTENT_MATHNET_VAULT
MATH_CONTENT_TRAINING_DATA
MATH_CONTENT_VIDEO2MD
```

技能应优先读取仓库附近的 `config/local-paths.yaml`，其次读取环境变量。两者都不存在时，不得猜测路径。
