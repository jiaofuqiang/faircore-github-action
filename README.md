# FairCore GitHub Action — 自动股权贡献记录

> 让团队每一次 PR merge 的代码贡献，自动进入 FairCore DSCE 股权计算。

## 效果

```
PR #42 merged by @alice
  ↓ 自动记录
FairCore 项目成员 alice: +3.5 工时贡献点
  ↓ 累计
DSCE 股权实时更新（Alice 贡献占比 ↑）
```

## 快速安装

### 1. 在仓库根目录创建

```
.github/workflows/faircore-contribution.yml
```

内容直接复制本仓库的 `faircore-contribution.yml`

### 2. 配置 GitHub Secrets（仓库 Settings → Secrets → Actions）

| Secret | 说明 | 示例 |
|---|---|---|
| `FAIRCORE_API_KEY` | 团队 API Key | `fc_live_xxxx` |
| `FAIRCORE_PROJECT_ID` | 项目 ID | `p_demo_2026` |
| `FAIRCORE_API_URL` | API 地址（可选）| `https://api.faircore.ai` |

### 3. 完成

每次 PR 合并到 main/master/develop，分支自动：
1. 计算变更行数（+增/-删）
2. 估算工时（30行/小时）
3. 上报到 FairCore 项目

## 验证

在仓库 Actions 页面查看 `FairCore Contribution Auto-Record` workflow 日志：

```
✅ PR #42 contributed 3.5 hours to FairCore project p_demo_2026
```

## 配置项

```yaml
# 修改触发分支（默认 main/master/develop）
on:
  pull_request:
    branches: [main, develop, release/*]
```

```yaml
# 修改工时估算（默认 30行/小时）
HOURS=$(python3 -c "print(max(0.5, round(($ADDED+$DELETED)/30, 1)))")
```

## 隐私说明

- 仅读取 PR 元信息（标题、URL、作者、变更行数）
- 不读取代码内容
- 数据直接 POST 到你配置的 FairCore API（不开源、不共享）

## 扩展场景

- **代码评审者**：自动为 Reviewer 增加权重（`review` 类型贡献）
- **Issue 关闭**：关联 Issue 类型贡献
- **多语言**：支持非代码文件（文档、设计稿）

<!-- DELIVERED: 2026-08-20 -->
