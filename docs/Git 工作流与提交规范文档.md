# CoBoard Git 工作流与提交规范文档

---

## 一、 Git 分支管理策略 (GitLab Flow 简化版)

### 1. 核心分支说明

- **`main` 分支**：线上/生产分支。仅存放经过测试、绝对稳定、可直接上线的代码。不可在此分支直接提交代码。
- **`test` 分支**：测试/集成分支。用于汇总各开发分支的代码进行集中测试，作为“测试沙盒”，不直接用于上线。

### 2. 工作流步骤

1.  **拉取开发分支**：所有新功能或 Bug 修复，统一从最新稳定的 **`main`** 分支拉取。
    ```bash
    git checkout main
    git pull origin main
    git checkout -b feature/your-feature-name
    ```
2.  **合并测试**：开发完成后，将开发分支合并到 **`test`** 分支进行集成测试。
    ```bash
    git checkout test
    git pull origin test
    git merge feature/your-feature-name
    # 在 test 环境下进行测试
    ```
3.  **上线合并**：测试通过后，将**开发分支**（而非 `test` 分支）合并入 **`main`** 分支发布。
    ```bash
    git checkout main
    git merge feature/your-feature-name
    git push origin main
    ```
4.  **清理分支**：合并完成后，删除本地和远程的临时开发分支。
    ```bash
    git branch -d feature/your-feature-name
    ```

---

## 二、 分支命名规范

分支命名统一采用 `类型/功能简述`（小写字母，单词间用连字符 `-` 连接）的格式：

| 分支名前缀      | 适用场景                   | 命名示例                                      |
| :-------------- | :------------------------- | :-------------------------------------------- |
| **`feature/`**  | 开发新功能                 | `feature/jwt-auth`<br>`feature/drag-and-drop` |
| **`bugfix/`**   | 修复普通 Bug               | `bugfix/cors-error`<br>`bugfix/token-expired` |
| **`hotfix/`**   | 紧急修复线上生产环境 Bug   | `hotfix/database-crash`                       |
| **`refactor/`** | 代码重构（无功能/Bug变化） | `refactor/prisma-client`                      |
| **`docs/`**     | 仅修改文档                 | `docs/api-spec`                               |

---

## 三、 Commit 提交信息规范 (Conventional Commits)

### 1. 提交格式

```text
<type>(<scope>): <subject>
```

- `type`（必填）：提交类型。
- `scope`（选填）：影响的范围/模块（如：`server`, `client`, `auth`, `board`）。
- `subject`（必填）：对本次提交的简要描述（动词开头，英文建议用一般现在时）。

### 2. 提交类型（Type）说明

| 类型 (Type)    | 说明                                 | 示例                                           |
| :------------- | :----------------------------------- | :--------------------------------------------- |
| **`feat`**     | 引入新功能                           | `feat(auth): add registration endpoint`        |
| **`fix`**      | 修复 Bug                             | `fix(client): resolve card drag state error`   |
| **`docs`**     | 仅修改文档                           | `docs: update deployment steps in README`      |
| **`style`**    | 格式化、缺少分号等（不影响代码逻辑） | `style(client): fix formatting in App.vue`     |
| **`refactor`** | 代码重构（非 feat 也非 fix）         | `refactor(server): extract jwt middleware`     |
| **`perf`**     | 提升性能、优化体验                   | `perf(db): add index to boardId in list table` |
| **`test`**     | 增加或修改测试用例                   | `test(server): add test for login service`     |
| **`chore`**    | 构建过程、辅助工具、依赖管理改动     | `chore: configure pnpm workspace`              |
| **`build`**    | 影响构建系统或外部依赖的改动         | `build: upgrade vite.config`                   |
| **`ci`**       | 持续集成、部署脚本改动               | `ci: add github actions test workflow`         |

### 3. 命令行操作示例

````bash
git commit -m "feat(auth): implement user registration with password hashing"
git commit -m "refactor(server): extract prisma client to shared instance"
git commit -m "chore: add .env template for development"
```v
````
