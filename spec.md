
---
description: 用于将多源需求输入（文档 / 图片 / 链接 / Figma 等）
---

---
details: 工程化转化为 AI 友好的结构化需求文档（spec.md）。 强制创建 feature 分支，按模块 × 功能组织需求， 明确事实 / 推断 / 假设，并给出可验证的行为语义与验收标准。 仅输出文档，不进入实现与代码阶段。
---

# Spec Generation Prompt（Final · With Clarification）

## 一、User Input

```text
1. 加载以下提示词文档（按顺序，遵循 doc_loader.md 规则）：
   - ${old_base}
   - ${git_rules}
   - ${constitution}（如存在，项目级基础规则）

2. 按照上述文档所体现的工程价值观，
   将以下输入工程化输出为结构化需求文档（只写文档，不写代码）：

   $ARGUMENTS

3. 输出语言：中文为主，术语可使用英文
````

### 前置环境检查（强制）

- 遵循 `git_rules` 中的 Git 规则
- 确认输出目录 `./mydoc/{date}-{taskname}/` 可写，不存在则创建
- 按 doc_loader 规则检查所需 prompt 是否可读取，缺失时必须暂停并告知

---

## 二、Git 规则引用（强制）

- 遵循 `git_rules`

---

## 三、核心原则（强制）

```
- spec 的目标不是“看起来完整”，而是“真实、可核查”
- 任何无法从输入直接确认的内容：
  - 必须标注为 Inference 或 Assumptions
  - 并进入澄清问题列表
- 严禁 spec 对不确定问题自行做决定
- spec 允许不完整，但不允许隐瞒不确定性
```

---

## 四、执行规则（强制）

* 不允许总结原文，必须进行结构化重组
* 必须区分并标注：

  * Facts
  * Inference（注明依据）
  * Assumptions
* 不允许进入技术方案、实现、架构讨论
* 图片 / Figma：

  * 只提取可观察事实
  * 所有交互必须标注为推断
* 每个功能：

  * 必须有行为语义（Given / When / Then）
  * **若行为存在不确定性，必须转化为澄清问题**

---

## 四.五、OpenAPI 规范约束（强制）

**OpenAPI 规范是 API 接口定义的唯一真源（Single Source of Truth）**

* 所有 API 接口定义必须基于 OpenAPI 规范文件
* 若需求涉及 API 接口：
  * 必须首先查找并引用对应的 OpenAPI 规范文件
  * 不得基于代码、文档或其他来源自行推断接口定义
  * 若 OpenAPI 规范不存在或过时，必须：
    - 在 spec.md 中明确标注为"缺失 OpenAPI 规范"
    - 将其作为澄清问题提出
    - 建议创建或更新 OpenAPI 规范
* 在 spec.md 中引用 API 时：
  * 必须标注对应的 OpenAPI 规范路径和操作 ID
  * 格式：`[OpenAPI: {file_path}#/{operationId}]`
* 若发现代码实现与 OpenAPI 规范不一致：
  * 以 OpenAPI 规范为准
  * 在 spec.md 中标注不一致点，并建议修正代码实现

---

## 五、输出要求（真实落盘）

```text
./mydoc/{date}-{taskname}/spec.md
```

> spec 生成后，可按需运行 `special.md` 进行澄清质询；非强制，但如存在不确定性或输入模糊，建议执行后再进入后续流程

---

## 六、spec.md 模板（增强版）

```markdown
# 结构化需求文档（AI Friendly）

## 0. 元信息
- Feature Name：
- Git Branch：feature/{taskname}
- 创建日期：
- 需求状态：Draft / Clarifying / Approved
- OpenAPI 规范路径：（如有）

---

## 1. 需求来源与输入摘要
...

---

## 2. 需求理解
### Facts
### Inference
### Assumptions

---

## 3. 模块与功能清单
...

---

## 4. 结构化需求（按模块）

### 模块 A

#### 功能 A1

- 目标：
- 非目标：
- 主流程：
- 异常与边界：
- 不变量：

##### API 接口（如有）
- 接口定义：[OpenAPI: {file_path}#/{operationId}]
- 请求参数：基于 OpenAPI 规范
- 响应结构：基于 OpenAPI 规范
- 若 OpenAPI 规范缺失：标注为澄清问题

##### 验证用例（Given / When / Then）
- Given：
- When：
- Then：

##### 未决点（Open Questions · 必须填写）
- OQ-1：
- OQ-2：
（若无，明确写 “None”）

---

（按功能重复）

---

## 5. 全局规则与不确定性
- 未明确的全局约束：
- 潜在冲突点：

---

## 6. 验收标准
...

---

## 7. 澄清问题总表（Spec Self-Disclosure）

> 本节为 spec 主动暴露的不确定性汇总  
> 禁止在此给出答案

- Q1（模块 A / 功能 A1）：…
- Q2（模块 B / 功能 B3）：…
- ...

---

## 8. 变更记录
- v1：初版 spec（包含未决问题）
```
