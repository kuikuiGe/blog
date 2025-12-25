---
description: 生成中文 commit 信息
---

# 生成中文 Commit 信息 Workflow

这个 workflow 帮助你基于当前的 git 改动生成规范的中文 commit 信息。

## 步骤

### 1. 检查 git 状态

首先查看当前的 git 状态，确认有哪些文件被修改：

```bash
git status
```

### 2. 查看改动内容

查看已暂存的改动（如果有）：

```bash
git diff --cached
```

如果暂存区为空，查看工作区的改动：

```bash
git diff
```

### 3. 分析改动内容

根据 diff 输出，分析以下内容：

- 修改了哪些文件
- 主要改动类型（重构、修复、新功能、优化等）
- 改动的核心目的
- 是否有破坏性变更
- 是否修复了 bug 或关联了 issue

### 4. 生成 commit 信息

根据改动内容，生成符合以下规范的中文 commit 信息：

**格式：**

```
<type>(<scope>): <subject>

<body>
```

**Type 类型：**

- `feat`: 新功能
- `fix`: 修复 bug
- `refactor`: 重构（既不是新功能也不是修复）
- `perf`: 性能优化
- `style`: 代码格式调整（不影响代码运行）
- `docs`: 文档更新
- `test`: 测试相关
- `chore`: 构建过程或辅助工具的变动
- `revert`: 回滚之前的 commit
- `build`: 构建系统或外部依赖的变动
- `ci`: CI 配置文件和脚本的变动

**Scope 范围（可选）：**

- 组件名、模块名或文件名
- 示例：`user-card`、`auth`、`workflow`、`config`
- 如果改动影响多个模块，可以省略或使用 `*`

**Subject 主题：**

- 简短描述，不超过 50 字
- 使用中文
- 动词开头，如：添加、修复、优化、重构
- 不要句号结尾

**Body 正文（可选）：**

- 详细说明改动内容
- 说明改动原因
- 列出主要改动点
- 每行不超过 72 字

### 5. 执行 commit

使用生成的 commit 信息提交代码：

```bash
git commit -m "生成的 commit 信息"
```

如果 commit 信息较长，使用多行格式：

```bash
git commit -m "type(scope): subject" -m "body 内容"
```

## 示例

### 示例 1：添加新功能

```bash
git commit -m "feat(login): 添加记住密码功能"
```

### 示例 2：修复 Bug

```bash
git commit -m "fix(button): 修复按钮点击无响应问题

- 修复事件监听器未正确绑定的问题
- 添加防抖处理避免重复点击"
```

### 示例 3：代码重构

```bash
git commit -m "refactor(utils): 重构日期格式化函数

- 使用 dayjs 替代原生 Date 对象
- 统一日期格式化逻辑
- 提升代码可读性"
```

### 示例 4：性能优化

```bash
git commit -m "perf(list): 优化列表加载速度

- 添加分页加载
- 实现图片懒加载
- 减少首屏渲染时间"
```

### 示例 5：更新文档

```bash
git commit -m "docs(readme): 更新项目安装说明"
```

### 示例 6：修改配置

```bash
git commit -m "chore(eslint): 更新代码规范配置

- 添加 Vue 相关规则
- 禁用 console 警告"
```

### 示例 7：调整样式

```bash
git commit -m "style(header): 调整导航栏样式

- 修改导航栏背景色
- 统一按钮间距"
```

### 示例 8：添加测试

```bash
git commit -m "test(login): 添加登录功能单元测试"
```

## 注意事项

1. **保持简洁**：subject 应该简短明了，详细内容放在 body 中
2. **使用中文**：所有描述使用中文，更易于团队理解
3. **遵循规范**：统一的 commit 格式有助于生成 changelog 和代码审查
4. **原子性提交**：每次 commit 应该只做一件事，便于回滚和追踪
5. **动词开头**：subject 使用动词开头，如：添加、修复、优化、重构、更新
6. **现在时态**：使用现在时态描述改动，而不是过去时

## 快捷使用

### 使用 AI 助手

最推荐的方式是直接在 AI 助手对话中输入 `/commit`，AI 会自动：

- 检查 git 状态和改动内容
- 分析改动类型和范围
- 生成符合规范的中文 commit 信息
- 提供可直接执行的 git commit 命令

### 常用场景快速参考

| 场景     | Type     | 示例                               |
| -------- | -------- | ---------------------------------- |
| 新增功能 | feat     | `feat(auth): 添加用户登录功能`     |
| 修复问题 | fix      | `fix(api): 修复接口超时问题`       |
| 代码重构 | refactor | `refactor(utils): 重构工具函数`    |
| 性能优化 | perf     | `perf(image): 优化图片加载速度`    |
| 样式调整 | style    | `style: 统一代码格式`              |
| 文档更新 | docs     | `docs(api): 更新 API 文档`         |
| 测试相关 | test     | `test(user): 添加用户模块单元测试` |
| 构建配置 | chore    | `chore(deps): 升级依赖版本`        |
| 依赖更新 | build    | `build(npm): 更新 package.json`    |
| CI 配置  | ci       | `ci(github): 添加 GitHub Actions`  |
