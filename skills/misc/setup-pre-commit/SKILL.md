---
name: setup-pre-commit
description: 在当前仓库中设置带有 lint-staged (Prettier)、类型检查和测试的 Husky pre-commit hooks。当用户想要添加 pre-commit hooks、设置 Husky、配置 lint-staged 或添加提交时格式化/类型检查/测试时使用。 (Set up Husky pre-commit hooks with lint-staged (Prettier), type checking, and tests in the current repo. Use when user wants to add pre-commit hooks, set up Husky, configure lint-staged, or add commit-time formatting/typechecking/testing.)
---

# 设置 Pre-Commit Hooks

## 设置内容 (What This Sets Up)

- **Husky** pre-commit hook
- **lint-staged** 对所有暂存文件运行 Prettier
  > **lint-staged** running Prettier on all staged files
- **Prettier** 配置（如果缺失）
  > **Prettier** config (if missing)
- pre-commit hook 中的 **typecheck** 和 **test** 脚本
  > **typecheck** and **test** scripts in the pre-commit hook

## 步骤 (Steps)

### 1. 检测包管理器

检查 `package-lock.json`（npm）、`pnpm-lock.yaml`（pnpm）、`yarn.lock`（yarn）、`bun.lockb`（bun）。使用存在的那个。如果不确定，默认使用 npm。

> Check for `package-lock.json` (npm), `pnpm-lock.yaml` (pnpm), `yarn.lock` (yarn), `bun.lockb` (bun). Use whichever is present. Default to npm if unclear.

### 2. 安装依赖

以 devDependencies 安装：
> Install as devDependencies:

```
husky lint-staged prettier
```

### 3. 初始化 Husky

```bash
npx husky init
```

这会创建 `.husky/` 目录并在 package.json 中添加 `prepare: "husky"`。

> This creates `.husky/` dir and adds `prepare: "husky"` to package.json.

### 4. 创建 `.husky/pre-commit`

写入此文件（Husky v9+ 不需要 shebang）：
> Write this file (no shebang needed for Husky v9+):

```
npx lint-staged
npm run typecheck
npm run test
```

**适配**：将 `npm` 替换为检测到的包管理器。如果仓库的 package.json 中没有 `typecheck` 或 `test` 脚本，省略相应行并告知用户。

> **Adapt**: Replace `npm` with detected package manager. If repo has no `typecheck` or `test` script in package.json, omit those lines and tell the user.

### 5. 创建 `.lintstagedrc`

```json
{
  "*": "prettier --ignore-unknown --write"
}
```

### 6. 创建 `.prettierrc`（如果缺失）

仅在没有 Prettier 配置时创建。使用以下默认值：
> Only create if no Prettier config exists. Use these defaults:

```json
{
  "useTabs": false,
  "tabWidth": 2,
  "printWidth": 80,
  "singleQuote": false,
  "trailingComma": "es5",
  "semi": true,
  "arrowParens": "always"
}
```

### 7. 验证

- [ ] `.husky/pre-commit` 存在且可执行
  > `.husky/pre-commit` exists and is executable
- [ ] `.lintstagedrc` 存在
  > `.lintstagedrc` exists
- [ ] package.json 中的 `prepare` 脚本是 `"husky"`
  > `prepare` script in package.json is `"husky"`
- [ ] `prettier` 配置存在
  > `prettier` config exists
- [ ] 运行 `npx lint-staged` 验证是否工作
  > Run `npx lint-staged` to verify it works

### 8. 提交

暂存所有更改/创建的文件并提交，消息为：`Add pre-commit hooks (husky + lint-staged + prettier)`

> Stage all changed/created files and commit with message: `Add pre-commit hooks (husky + lint-staged + prettier)`

这将通过新的 pre-commit hooks —— 一个验证一切正常的烟雾测试。

> This will run through the new pre-commit hooks — a good smoke test that everything works.

## 注意事项 (Notes)

- Husky v9+ 不需要在 hook 文件中添加 shebang
  > Husky v9+ doesn't need shebangs in hook files
- `prettier --ignore-unknown` 跳过 Prettier 无法解析的文件（如图片）
  > `prettier --ignore-unknown` skips files Prettier can't parse (images, etc.)
- pre-commit 先运行 lint-staged（快速，仅暂存文件），然后运行完整的类型检查和测试
  > The pre-commit runs lint-staged first (fast, staged-only), then full typecheck and tests
