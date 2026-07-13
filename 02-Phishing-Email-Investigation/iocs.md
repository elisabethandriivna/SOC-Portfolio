# Indicators Found During the Investigation

## Important Note

This project uses safe training data.

The domains ending in `.test` and the IP addresses in this sample are reserved for examples and documentation. They are not real malicious systems.

For this reason, the values below are training indicators, not active indicators of compromise.

## Email Indicators

| Type | Value | Why It Is Suspicious |
|---|---|---|
| Claimed sender | `security@microsoft.com` | The email pretends to come from Microsoft |
| Reply-To | `microsoft-helpdesk-verify@gmail.com` | It does not match the claimed Microsoft sender |
| Return-Path | `bounce@evil-example.test` | It uses a different domain |
| Message-ID domain | `evil-example.test` | It does not match the visible sender |
| Subject | `Urgent password verification required` | It uses urgency to pressure the user |

## Network Indicators

| Type | Value | Why It Is Suspicious |
|---|---|---|
| Sending host | `unknown-vps-host.test` | It is not connected to the claimed Microsoft sender |
| Sending IP | `198.51.100.23` | Training IP used by the sending host |
| Link | `http://198.51.100.77/login` | It is hidden behind Microsoft link text |
| Link IP | `198.51.100.77` | The login page uses a raw IP address |
| Mail server IP | `203.0.113.10` | Reserved IP used in the training email |

## Attachment Indicator

| Type | Value | Why It Is Suspicious |
|---|---|---|
| Filename | `invoice.pdf.exe` | An executable file is disguised as a PDF |
| File type | `application/octet-stream` | The attachment is handled as binary data |
| Safe content | `FAKE-DEMO-CONTENT-NOT-MALWARE` | Confirms that this is a safe training sample |

## Authentication Results

| Security Check | Result | Meaning |
|---|---|---|
| SPF | Fail | The sending server did not pass the SPF check |
| DKIM | Fail | The email did not pass the DKIM signature check |
| DMARC | Fail | The sender identity did not pass DMARC checks |

## Indicator Summary

The strongest indicators are:

- the Microsoft sender and Gmail Reply-To mismatch;
- failed SPF, DKIM, and DMARC checks;
- the link mismatch;
- the raw IP address used as a login page;
- the `.pdf.exe` attachment.
