# Business Logic Flaw: KYC Bypass via Mismatched ID Document Images

> **⚠️ Disclaimer:** This is a **sanitized educational writeup** of a business logic vulnerability found during authorized bug bounty testing. All identifying information (vendor name, platform, account IDs, program details, bounty amounts) has been **removed or replaced** to comply with responsible disclosure policies. The vulnerability has been **fully remediated** by the affected party.

---

## 📌 Executive Summary

| Field | Value |
|-------|-------|
| **Category** | Business Logic Flaw |
| **Subcategory** | Identity Verification (KYC) Bypass |
| **Affected Platform** | Android Mobile Application + Web API |
| **Severity (Original)** | Critical (per vendor's scoring) |
| **Severity (Adjusted)** | Low - Medium (per final vendor assessment) |
| **Status** | ✅ Remediated |

---

## 🔍 Vulnerability Overview

During authorized security testing of a **financial services mobile application**, a **KYC (Know Your Customer) verification bypass** was identified.

### The Flaw

The mobile application's Primary KYC verification workflow **accepted mismatched front and back images** of government-issued identity documents:

- **Front image**: Belonging to Identity A
- **Back image**: Belonging to Identity B (different person)

The server-side verification logic **failed to cross-validate** that both sides of the document belonged to the same individual. The system:

1. ✅ Marked the account as **"Verified"**
2. ✅ Granted full platform access
3. ✅ Allowed creation of **API keys with elevated permissions** (Trade + Withdraw)

---

## 🎯 Affected Asset

- **Platform**: Financial Services Mobile Application (Android)
- **Component**: KYC Verification Module
- **Also affects**: Web API key generation endpoint (secondary impact)

> ⚠️ **Vendor, application name, and platform details are intentionally omitted.**

---

## 🧪 Steps to Reproduce (Sanitized)

> **Setup**: Register a test account on the target platform.

1. **Login** to the Android application with a new test account.

2. **Navigate** to the KYC verification section.
   - Select **Primary KYC** (Region: Egypt).

3. **Submit** a live photo of the **front side** of a national ID document belonging to **Identity A**.

4. **Submit** a live photo of the **back side** of a national ID document belonging to **Identity B** (a completely different person).

5. **Submit** the documents for verification.

6. **Observe**: The system approves the submission and updates the account status to **"Verified"**.

7. **Login** to the **Web platform** using the same verified account.

8. **Navigate** to **API Management**.

9. **Create** a new API key with the following permissions enabled:
   - ✅ Trade
   - ✅ Withdraw

10. **Observe**: API key creation succeeds with elevated permissions.

---

## 📊 Actual vs. Expected Behavior

### ❌ Actual Behavior

The system:
- Accepts **mismatched** front and back ID document images
- Grants **"Verified"** account status
- Permits API key creation with **Trade + Withdraw** permissions

### ✅ Expected Behavior

The system should:
- Perform **strict cross-validation** on submitted front and back ID images
- **Automatically reject** mismatched documents
- Route suspicious submissions to **manual compliance review**
- Enforce **additional verification** before granting elevated API permissions

---

## 💥 Security Impact

### 1. **Identity Fraud Enablement**

Attackers can complete KYC verification using **inconsistent identity documents**, bypassing the platform's compliance framework.

### 2. **Synthetic Identity & Deceased Identity Fraud**

- Attackers can obtain **valid back-side document templates** from illicit markets
- Pair them with **arbitrary front-side images**
- Obtain **"Verified"** status
- Render identity attribution and legal compliance **ineffective**

### 3. **Money Laundering Risk**

- Pass KYC without proper document cross-validation
- Establish **clean, verified mule accounts**
- With API key generation (Trade + Withdraw), these accounts can:
  - Automate fund cycling
  - Mix fraudulent funds across borders
  - Bypass identity-bound transaction flags

### 4. **Regulatory Non-Compliance**

- Direct violation of **AML (Anti-Money Laundering)** regulations
- Direct violation of **CTF (Counter-Terrorist Financing)** regulations
- Exposure to:
  - Regulatory fines
  - Compliance audits
  - License suspension
  - Operational sanctions

---

## 🛠️ Recommended Fixes

### 1. **Server-Side Document Cross-Validation**

Implement robust server-side validation:
- **OCR** on both document sides
- **Data-matching algorithms** to verify that:
  - Names match
  - ID numbers match
  - Dates of birth match
  - Photos match

### 2. **Strict Server-Side Verification Logic**

- Ensure final KYC decisions originate from a **secure, trusted backend service**
- **Do NOT** rely on frontend completion signals or UI state

### 3. **Discrepancy & Anomaly Handling**

- **Automatically reject** submissions with conflicting identity data
- Route flagged submissions to a **manual compliance queue**
- **Do NOT** change account status to "Verified" until review completes

### 4. **API Authorization Controls**

- **Re-evaluate server-side authorization checks** for API key generation
- Enforce **additional validation** before granting:
  - Trade permission
  - Withdraw permission

### 5. **Audit Log Review**

- Inspect KYC verification logs
- Inspect API creation events
- Pinpoint the exact failure point in the processing pipeline

### 6. **Automated Regression Testing**

Introduce test suites containing:
- ✅ Matched ID image pairs
- ❌ Mismatched ID image pairs

To prevent regression of document consistency checks.

### 7. **Scope Assessment**

Assess whether this validation flaw affects:
- Other document types
- Other regional KYC workflows
- Related verification modules

---

## 📚 Lessons Learned

### For Security Researchers

1. **Business logic flaws** are often missed by automated scanners
2. **Cross-validation checks** are frequently absent in KYC workflows
3. **Mobile application testing** often reveals server-side issues
4. **Always test edge cases** (mismatched inputs, boundary conditions)

### For Developers

1. **Never trust the client** — validate everything server-side
2. **Cross-validate related data points** (front/back of documents, related fields)
3. **Implement defense in depth** for identity verification
4. **Add regression tests** for security-critical logic

---

## 🎓 Key Takeaways

| Aspect | Lesson |
|--------|--------|
| **Discovery** | Manual testing > Automated scanning for business logic |
| **Impact** | Compliance bypass = regulatory + financial risk |
| **Exploitability** | Requires no special tools — just mismatched inputs |
| **Fix Complexity** | Medium — requires server-side validation logic |
| **Prevention** | Defense in depth + automated testing |

---

## ⚖️ Responsible Disclosure

This vulnerability was:

- ✅ Discovered during **authorized bug bounty testing**
- ✅ Reported through the platform's **official bug bounty program**
- ✅ **Fully remediated** by the vendor
- ✅ Published as an **educational case study** after remediation

**All identifying information has been removed** to comply with:
- The bug bounty program's confidentiality agreement
- Responsible disclosure best practices
- Ethical security research guidelines

---

## 📄 License

This writeup is released under **MIT License** for educational purposes.

---

**Last Updated**: October 2026
