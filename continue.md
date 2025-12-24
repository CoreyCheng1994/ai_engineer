---
description: 继续当前分支未完成任务（special -> task）
---
---
details: 识别当前分支上的未完成任务，必要时先运行 special 澄清，再继续 task 执行与推进。
---

# Continue Prompt（special -> task）

## 一、User Input

```text
1. 加载以下提示词文档（按顺序，遵循 doc_loader.md 规则）：
   - ${old_base}
   - ${new_base}
   - ${git_rules}
   - ${codestyle}
   - ${data_rule}
   - ${constitution}（如存在，项目级基础规则）

2. 我将要求你继续当前分支未完成任务，请按本文档流程执行：

   $ARGUMENTS
```

---

## 二、目标

- 自动识别当前分支是否存在未完成任务
- 若存在未澄清 spec，先执行 special 输出澄清问题
- 若存在 task.md，按优先级继续推进任务执行
- 保持文档与代码同步，确保过程可追溯

---

## 三、执行流程（强制）

### 3.1 定位当前任务上下文

- 必须通过 Git 识别当前分支
- 分支名必须符合 `feature/{taskname}`，否则暂停并请求确认
- 在 `./mydoc/` 下搜索匹配 `{date}-{taskname}` 的目录
  - 若有多个匹配目录，选择日期最新的一个
  - 若未找到，暂停并请求用户提供路径或重新初始化流程

---

### 3.2 spec 澄清优先（special）

- 若存在 `spec.md` 且尚未进行澄清：
  - 运行 special 流程，输出澄清问题
  - 将结果写入 `./mydoc/{date}-{taskname}/special.md`
  - 暂停后等待用户答复（收到答复后再继续）

> 判定“尚未澄清”的最小条件：`special.md` 不存在。

---

### 3.3 继续任务（task）

- 若存在 `task.md`：
  - 优先继续状态为 **DOING** 的任务
  - 若无 DOING，按 P0 → P1 → P2 顺序选择第一个 **TODO** 任务
  - 若仅剩 **BLOCKED**，说明阻塞原因并请求用户解除
  - 每完成一个任务：
    - 更新 `task.md` 中状态为 DONE
    - 在 `framework.md` 中记录实现要点与测试结果

- 若不存在 `task.md` 但存在 `framework.md`：
  - 根据 `framework.md` 生成 `task.md`（遵循 old_base 的任务拆解要求）
  - 然后进入任务推进逻辑

- 若 `spec.md` / `framework.md` / `task.md` 均不存在：
  - 暂停并提示需要先运行 spec 或对应的任务初始化流程

---

## 四、输出要求

- 文档输出路径必须落盘到：
  - `./mydoc/{date}-{taskname}/special.md`（如执行 special）
  - `./mydoc/{date}-{taskname}/framework.md`
  - `./mydoc/{date}-{taskname}/task.md`
- 所有状态更新必须真实写入文件
- 若流程被阻塞，必须给出明确阻塞原因与待确认点
