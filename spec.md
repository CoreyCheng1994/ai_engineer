
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

2. 按照上述文档所体现的工程价值观，
   将以下输入工程化输出为结构化需求文档（只写文档，不写代码）：

   $ARGUMENTS

3. 输出语言：中文为主，术语可使用英文
````

---

## 二、Git 分支管理（强制）

```
- 在开始处理本任务前，必须创建或切换到新的 Git 分支
- 分支命名规则为：

  feature/{taskname}

- 所有本次任务产生的文档变更
  必须提交在该 feature 分支上
```

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

## 五、输出要求（真实落盘）

```text
./{workspace}/mydoc/{date}-{taskname}/spec.md
```

---

## 六、spec.md 模板（增强版）

```markdown
# 结构化需求文档（AI Friendly）

## 0. 元信息
- Feature Name：
- Git Branch：feature/{taskname}
- 创建日期：
- 需求状态：Draft / Clarifying / Approved

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

