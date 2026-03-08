# KYC Vault — OpenClaw Skill

自动完成网站 KYC 身份认证的 OpenClaw Skill。

你把证件照片放在本地文件夹，Skill 访问每个文件前会先请求你的授权，然后自动打开网站、找到身份认证页面并上传文件。

---

## 安装步骤

### 1. 确认已安装 OpenClaw

从 [openclaw.ai](https://openclaw.ai) 下载并完成初始设置。

### 2. 安装此 Skill

在终端运行：

```bash
# 创建 skill 目录
mkdir -p ~/.openclaw/workspace/skills/kyc-vault

# 下载 SKILL.md
curl -o ~/.openclaw/workspace/skills/kyc-vault/SKILL.md \
  https://raw.githubusercontent.com/seamao/kyc-vault-skill-/main/SKILL.md
```

### 3. 创建你的 Identity Vault

```bash
mkdir -p ~/identity-vault
```

复制模板文件：

```bash
curl -o ~/identity-vault/manifest.json \
  https://raw.githubusercontent.com/seamao/kyc-vault-skill-/main/manifest.template.json
```

### 4. 填写你的信息

用文本编辑器打开 `~/identity-vault/manifest.json`，替换所有占位符为你的真实信息。

### 5. 放入你的证件照片

把证件照片放到 `~/identity-vault/` 文件夹，文件名与 `manifest.json` 中的 `filename` 字段一致：

```
~/identity-vault/
  ├── manifest.json
  ├── palau_id_holding.jpg    ← 手持帕劳ID照片
  ├── passport_front.jpg      ← 护照照片页
  ├── selfie.jpg              ← 本人正面照
  └── address_proof.pdf       ← 地址证明
```

### 6. 在 OpenClaw 里激活 Skill

在 OpenClaw 聊天里发送：

```
/skills refresh
```

---

## 使用方法

| 命令 | 说明 |
|------|------|
| `kyc https://xxx.com` | 开始对该网站做 KYC |
| `kyc list` | 查看我的证件列表 |
| `kyc setup` | 引导创建 vault |
| `kyc status https://xxx.com` | 查看认证状态 |

---

## 安全说明

- 所有文件存储在你自己的电脑上，不经过任何服务器
- 每次使用文件前，Skill 都会弹出授权确认
- `trusted_sites` 字段可设置永久信任的网站

---

## 支持的证件类型

| 类型 | 说明 |
|------|------|
| `palau_id_with_selfie` | 手持帕劳数字居民ID |
| `palau_id` | 帕劳数字居民ID |
| `passport` | 国际护照 |
| `government_id_with_selfie` | 手持政府颁发ID |
| `government_id` | 政府颁发ID |
| `selfie` | 本人正面照 |
| `address_proof` | 地址证明 |
