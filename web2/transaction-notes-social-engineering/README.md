# Vulnerability Report: Social Engineering Abuse via Transaction Notes

**Status:** Reported / Disclosed after 90 days
**Category:** Social Engineering / User-Generated Content Abuse
**Severity:** Low to Medium

---

## 1. Vulnerability Overview
- **Title:** Potential Social Engineering Abuse Through Transaction Notes
- **Affected Component:** Cryptocurrency Transfers – Transaction Note / Remarks Field
- **Impact:** The platform allows a sender to include a free-text note (approx. 50 characters) when making a transfer. Although external URLs in the note are not clickable, the unrestricted field creates an opportunity for social engineering. A malicious sender can craft a deceptive message encouraging the recipient to manually copy/type an external URL into their browser (e.g., claiming a "reward" for visiting a website).

## 2. Technical Description
The application allows a sender to include a free-text note during a transfer. The key concern is the misuse of this field for unsolicited promotional claims, phishing instructions, and social engineering, which could undermine user trust in the platform's notification integrity.

## 3. Reproduction Scenario (PoC)
1. Log in to the application.
2. Navigate to the cryptocurrency transfer feature.
3. Enter a recipient's UID (e.g., [REDACTED]).
4. Enter a small transfer amount (e.g., dust amount).
5. Use the transaction note field to enter a short message containing misleading instructions (e.g., "Visit [MALICIOUS_URL] to claim your reward").
6. Complete the transfer.
7. Observe how the message appears to the recipient in the transaction details.

**Observed Behavior:** The transaction note is displayed as text, and the external URL is not directly clickable. However, a recipient may manually enter the supplied URL into a browser and potentially fall victim to phishing.

## 4. Potential Impact
- Conduct phishing attacks by encouraging recipients to visit fraudulent websites.
- Impersonate official promotions.
- Damage platform reputation by associating fraudulent communications with the ecosystem.

## 5. Recommended Remediation
- **A.** Replace free-text notes with predefined transaction categories.
- **B.** If free-text notes are required, introduce stricter controls, content filtering, and abuse detection.
- **C.** Introduce reporting and anti-abuse mechanisms for recipients.
- **D.** Clarify the source of transaction notes to inform recipients that notes are written by the sender and are not official announcements.

---


--- *Disclaimer: This report is published for educational purposes and to demonstrate security research methodologies. The affected platform's 
identity has been anonymized to comply with responsible disclosure practices.*
