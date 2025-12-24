---
description: 从现有项目自动采集技术栈/目录信息，生成或补充项目宪法
---

## User Input

```text
1. 加载以下提示词文档（按顺序，遵循 doc_loader.md 规则）：
   - ${constitution}.md  # 宪法生成 prompt

2. 提供当前仓库顶层文件/目录列表（执行 `ls`）和 Shell 环境（如 bash/zsh）

3. 目标：
   - 生成新的宪法或补充已有宪法
   - 输出路径：`./constitution.md`（可指定已有文件路径用于补充）
```

## 执行步骤（Codex 应用）

1) 生成采集脚本  
   - 基于提供的顶层文件/目录与 Shell，生成一段采集脚本，要求覆盖：
     - 目录树（建议 tree -L 3，可降级为 find）
     - 依赖/包管理文件：package.json/pnpm-lock.yaml/yarn.lock/bun.lockb/go.mod/pom.xml/build.gradle/settings.gradle/Cargo.toml/Gemfile/mix.exs/pyproject.toml/requirements*.txt/composer.json等
     - 构建/运行：Makefile/docker-compose*.yml/helm/chart.yaml/justfile 等
     - CI 配置：.github/workflows/*.yml、.gitlab-ci.yml、azure-pipelines.yml 等
     - 其他可识别语言标识文件（*.sln、*.csproj 等）
   - 脚本输出统一写入 `/tmp/constitution_context.md`（如权限受限可提示改路径）

2) 执行采集脚本  
   - 运行生成的脚本，确保命令安全、读操作为主；记录实际执行的命令与输出路径

3) 生成/补充宪法  
   - 使用 `constitution.md` 作为主 prompt，将 `/tmp/constitution_context.md` 作为长输入
   - 说明是“新建”还是“补充已有”（提供旧文件路径）
   - 如需项目级规则文件（codestyle/data_rule 衍生版等），允许生成并在宪法中记录路径与差异

## 输出

- 宪法文档：`./constitution.md`（或指定补充的现有路径）
- 如生成项目级规则文件：写入仓库内约定位置，并在宪法中记录路径与差异

> 注：`constitution.md` 已默认加载 `old_base`/`new_base`/`data_rule`，生成时会继承这些基线约束；采集脚本应依据实际文件类型动态生成，而非写死语言。
