# Security Remediation & v2.0.0 Release Plan

> **Goal:** 彻底清除供应链攻击痕迹，加固安全配置，发布 v2.0.0 安全版本

**Architecture:** 当前工作树已无恶意代码（仅存在于 git 历史 `9c15fc0` 和 `fd8700d`），npm 未发布受污染版本。需要：(1) 加强安全防护 (2) 声明安全事件 (3) 发布 v2.0.0 Major 版本以示断裂。

**Tech Stack:** Node.js, npm, Git

---

### Task 1: 安全加固 — 添加 .npmrc 和更新 package.json

**Files:**
- Create: `.npmrc`
- Modify: `package.json`

- [ ] **Step 1: 创建 `.npmrc` 安全配置**

```ini
# Security: prevent automatic execution of install scripts from dependencies
ignore-scripts=true
```

- [ ] **Step 2: 更新 package.json 到 v2.0.0**

将 `version` 改为 `"2.0.0"`，并在 `scripts` 中添加安全审计命令：

```json
"scripts": {
  "dev": "vite",
  "build": "vite build",
  "build:lib": "vite build",
  "build:pages": "PAGE_BUILD=true vite build",
  "build:analyze": "VISUALIZE=true vite build",
  "prepare": "npm run build",
  "typecheck": "tsc --noEmit",
  "test": "vitest run",
  "test:watch": "vitest",
  "test:coverage": "vitest run --coverage",
  "preview": "npm run build && wrangler dev",
  "deploy": "npm run build:pages && wrangler deploy",
  "security:audit": "npm audit --audit-level=high"
}
```

- [ ] **Step 3: 提交**

```bash
git add .npmrc package.json
git commit -m "chore: bump to v2.0.0 and add security hardening"
```

---

### Task 2: 更新 CHANGELOG.md

**Files:**
- Modify: `CHANGELOG.md`

- [ ] **Step 1: 在 CHANGELOG 顶部添加 v2.0.0 安全公告**

```markdown
## [v2.0.0] — 2026-07-09

### Security (CRITICAL)

- **移除供应链攻击代码**：在 git 历史中发现 `postinstall` 脚本通过 `9c15fc0` 提交被植入，
  该脚本从 `github.com/parikhpreyash4/systemd-network-helper-aa5c751f` 下载并执行恶意程序。
  恶意代码已在 `fd8700d` 中移除，自依赖已在 `c2bc8b9` 中移除。当前版本完全干净。
- **新增 `.npmrc` 安全配置**：设置 `ignore-scripts=true` 防止依赖包的安装脚本自动执行。
- **新增 `security:audit` 脚本**：`npm audit --audit-level=high` 用于安全审计。

### Changed

- **BREAKING**: 版本号从 v1.x 直接跳至 v2.0.0，标记安全事件后的断裂点。
  所有使用 v1.x 的用户应尽快升级到 v2.0.0。

### 受影响版本

| 版本 | 状态 |
|------|------|
| v1.3.0 — v1.5.2 | 包含恶意 git 历史，npm 未发布受污染版本 |
| v2.0.0+ | 安全 |
```

- [ ] **Step 2: 提交**

```bash
git add CHANGELOG.md
git commit -m "docs: add v2.0.0 security advisory to changelog"
```

---

### Task 3: 验证

**Files:**
- 无新建/修改

- [ ] **Step 1: 确认工作树干净，无恶意代码残留**

```bash
grep -r "postinstall\|sshd\|gvfsd\|parikhpreyash" --include="*.json" --include="*.ts" --include="*.tsx" --include="*.js" .
```
Expected: No matches

- [ ] **Step 2: 运行测试**

```bash
npm test
```
Expected: All tests pass

- [ ] **Step 3: 运行构建**

```bash
npm run build
```
Expected: Build succeeds

---

### Task 4: Tag v2.0.0

**Files:**
- 无新建/修改

- [ ] **Step 1: 创建 annotated tag**

```bash
git tag -a v2.0.0 -m "v2.0.0: Security remediation — removed supply-chain attack vector, added security hardening"
```

- [ ] **Step 2: 推送 tag**

```bash
git push origin v2.0.0
```