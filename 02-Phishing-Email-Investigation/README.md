# Phishing Email Investigation

## Objective

Analyze a suspicious email to identify phishing indicators and extract indicators of compromise (IOCs).

## Scenario

A user received an email claiming to be from Microsoft Security requesting urgent password verification.

The SOC team analyzed the email headers, URLs, and attachment to determine whether the message was malicious.

## Tools Used

- Email header analysis
- VirusTotal
- URL analysis tools
- IOC extraction

## Findings

The email contained multiple phishing indicators:

- Failed SPF, DKIM, and DMARC authentication
- Suspicious Reply-To address
- Mismatched URL destination
- Executable file disguised as PDF

## Conclusion

The email was identified as a phishing attempt designed to steal user credentials.

## Recommendations

- Do not open attachments
- Block malicious domains and IP addresses
- Reset credentials if the user interacted with the email
- Add indicators to security monitoring tools
