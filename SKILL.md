---
name: kyc_vault
description: Automates KYC identity verification by securely managing and submitting identity documents. Always asks user permission before accessing or uploading any file.
---

# KYC Vault Skill

This skill automates KYC (Know Your Customer) identity verification on websites using locally stored identity documents.

**Security Rule: ALWAYS ask the user for permission before accessing or uploading any file. Never skip this step.**

---

## Identity Vault Location

All identity documents are stored in `~/identity-vault/`.
Before doing anything, read `~/identity-vault/manifest.json` to understand what files and personal information are available.

---

## Permission Protocol

Before using ANY file, show this message and wait for user confirmation:

```
⚠️ 授权请求
文件：[filename]
类型：[document type]
用途：上传到 [website domain]

是否授权？
• 是（仅此次）
• 是（本次流程全部允许）
• 否
```

Only proceed after the user explicitly confirms. If user says no, skip that file and inform them which step was skipped.

---

## KYC Workflow

When user says "KYC [website URL]" or "帮我完成 [website] 的 KYC":

### Step 1: Read Vault
- Read `~/identity-vault/manifest.json`
- List available documents to the user
- Confirm which documents they want to make available for this session

### Step 2: Navigate to Website
- Open the website URL
- Find the KYC / Identity Verification / Account Verification section
- Look for links or buttons with text like: "Verify Identity", "Complete KYC", "Upload ID", "身份认证", "实名认证"

### Step 3: Identify Required Documents
- Analyze the KYC form to determine what documents are needed
- Map requirements to available files using this priority:
  - "Government ID" / "Photo ID" → `government_id_with_selfie` (preferred) or `government_id`
  - "Passport" → `passport`
  - "Selfie" / "Face photo" / "Liveness" → `selfie`
  - "Proof of address" / "Address verification" → `address_proof`
  - "Residency certificate" → `palau_id` or `government_id`

### Step 4: Request Permission and Upload
- For each required document:
  1. Show the permission request (see Permission Protocol above)
  2. Wait for user confirmation
  3. Upload the file to the correct field on the form
  4. Confirm the upload succeeded

### Step 5: Fill Text Fields
- Use `personal_info` from manifest.json to fill text fields:
  - Full name, date of birth, nationality, address
- Show the user what information you are about to fill in before submitting

### Step 6: Final Confirmation
Before clicking any final submit button, show:
```
📋 提交确认
网站：[website]
将要提交：
• [list of files used]
• 个人信息：姓名、生日等

确认提交吗？（是 / 否）
```

---

## Available Commands

| Command | Action |
|---------|--------|
| `kyc [URL]` | Start KYC process for a website |
| `kyc setup` | Guide user to set up their identity vault |
| `kyc list` | Show available documents (no file contents exposed) |
| `kyc status [URL]` | Check current KYC verification status on a website |

---

## Document Type Reference

| Type Key | Description |
|----------|-------------|
| `government_id` | Government-issued ID card (front) |
| `government_id_back` | Government-issued ID card (back) |
| `government_id_with_selfie` | Photo of person holding ID card |
| `passport` | International passport photo page |
| `selfie` | Face photo (no ID) |
| `address_proof` | Utility bill or bank statement |
| `palau_id` | Palau Digital Residency ID |
| `palau_id_with_selfie` | Holding Palau Digital Residency ID |

---

## Setup Guide (when user says "kyc setup")

Guide the user step by step:

1. Confirm `~/identity-vault/` folder exists
2. Ask them to place their identity documents in that folder
3. Help them fill out `manifest.json` with their document filenames and personal info
4. Verify the manifest is correct before finishing setup
