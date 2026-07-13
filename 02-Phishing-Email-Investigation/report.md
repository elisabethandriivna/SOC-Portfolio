# Phishing Email Investigation Report

## Case Information

| Field | Information |
|---|---|
| Case ID | PHISH-2026-001 |
| Investigation Type | Phishing email analysis |
| Severity | High |
| Status | Closed |
| Analyst | Yelisaveta Kaprelova |
| Evidence | Safe training `.eml` file |

## Summary

A user received an email that claimed to come from Microsoft Security.

The email asked the user to verify a password immediately. It warned that the user's account could be suspended.

During the investigation, I found several signs of phishing:

- the email failed SPF, DKIM, and DMARC checks;
- the Reply-To address did not match the sender;
- the visible link and the real link were different;
- the email contained an executable attachment disguised as a PDF;
- the message used urgent language to pressure the user.

I classified the email as a high-risk phishing simulation.

## Investigation Scope

I checked the following parts of the email:

- sender address;
- Reply-To address;
- Return-Path;
- Message-ID;
- Received headers;
- SPF, DKIM, and DMARC results;
- email subject and body;
- embedded link;
- attachment name.

I did not open the link or run the attachment.

## Email Details

| Field | Value |
|---|---|
| Subject | `Urgent password verification required` |
| Claimed Sender | `Microsoft Security <security@microsoft.com>` |
| Recipient | `Philip Ritter <philip@example.test>` |
| Reply-To | `microsoft-helpdesk-verify@gmail.com` |
| Return-Path | `bounce@evil-example.test` |
| Real Link | `http://198.51.100.77/login` |
| Attachment | `invoice.pdf.exe` |

## Detailed Analysis

### 1. Urgent Language

The email says:

`Please verify your password immediately to avoid account suspension.`

The sender tries to make the user feel worried and act quickly.

This is a common social engineering technique. Attackers often use fear and urgency because they do not want the user to stop and check the message.

### 2. Sender Information

The visible sender is:

`Microsoft Security <security@microsoft.com>`

This looks legitimate at first.

However, the Return-Path and Message-ID use:

`evil-example.test`

The email was also sent through:

`unknown-vps-host.test`

These values do not match the claimed Microsoft sender.

This suggests that the From address was spoofed.

### 3. Reply-To Address

The Reply-To address is:

`microsoft-helpdesk-verify@gmail.com`

This does not match the Microsoft sender address.

If the user replied to the email, the response would go to the Gmail address instead of Microsoft.

This is a strong phishing sign.

### 4. Email Authentication

The Authentication-Results header shows:

- `spf=fail`
- `dkim=fail`
- `dmarc=fail`

#### SPF

SPF checks whether the sending server is allowed to send email for the envelope sender's domain.

The SPF result was `fail`.

#### DKIM

DKIM checks the digital signature of an email.

The DKIM result was `fail`.

#### DMARC

DMARC checks whether the visible sender domain matches the authenticated domain.

The DMARC result was `fail`.

Because all three checks failed, the email could not prove that it was a legitimate Microsoft message.

### 5. Fake Link

The email displays:

`https://microsoft.com/security`

However, the HTML source shows this real destination:

`http://198.51.100.77/login`

The user sees a trusted Microsoft link, but clicking it would open a different address.

Other suspicious details are:

- the destination uses an IP address instead of a normal domain;
- the link uses HTTP instead of HTTPS;
- the URL contains `/login`, which may be used to collect passwords.

This is a clear link mismatch.

### 6. Suspicious Attachment

The attachment is named:

`invoice.pdf.exe`

This file uses two extensions:

- `.pdf`
- `.exe`

The `.pdf` part may make the user think that the file is a document.

However, the real extension is `.exe`. This means it is an executable file.

In a real case, the attachment could run malicious code.

In this training sample, the attachment contains only:

`FAKE-DEMO-CONTENT-NOT-MALWARE`

The file is safe and was not executed.

## Findings

| ID | Finding | Severity |
|---|---|---|
| F-01 | The email uses urgent account suspension language | Medium |
| F-02 | The visible sender does not match other header fields | High |
| F-03 | The Reply-To address uses an unrelated Gmail account | High |
| F-04 | SPF, DKIM, and DMARC checks failed | High |
| F-05 | The visible link hides a different destination | High |
| F-06 | The link uses a raw IP address and HTTP | High |
| F-07 | An executable file is disguised as a PDF | Critical |

## Final Assessment

The email should be treated as phishing.

The likely goals are:

1. to steal the user's password through a fake login page;
2. to make the user open an executable attachment;
3. to receive information through the fake Gmail Reply-To address.

The email presents a high risk because it uses several attack methods in one message.

## Recommended Response

### If the user did not interact with the email

- Quarantine and delete the email.
- Search for similar messages in other mailboxes.
- Block the suspicious Reply-To address and URL.
- Record the indicators in the security system.
- Warn other users about the phishing campaign.

### If the user clicked the link

- Ask whether the user entered a password.
- Reset the user's password if needed.
- Revoke active login sessions.
- Check recent login activity.
- Review MFA notifications and changes.

### If the user opened the attachment

- Isolate the computer from the network.
- Run an endpoint security scan.
- Review running processes and network connections.
- Check endpoint logs for suspicious activity.
- Escalate the case to the incident response team.

## Conclusion

The investigation found strong signs of phishing.

The sender information was inconsistent, all email authentication checks failed, the link was deceptive, and the attachment was an executable file disguised as a PDF.

This project used safe training data. No real malware or active malicious infrastructure was used.
