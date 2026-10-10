# Microscan and FishHub: Defensive Hunting After an Infrastructure Seizure

Published by MANDID  
Case-note date: October 10, 2026  
Scope: authorized defensive triage, evidence preservation and remediation

## Status and confidence boundary

On October 8, 2026, the U.S. Department of Justice and FBI announced court-authorized seizures of domains associated with two tools: Microscan and FishHub.

DOJ alleges that malicious cyber actors working for China-based Integrity Technology Group operated and used the tools. DOJ describes Microscan as a vulnerability-scanning platform and FishHub as a spear-phishing and post-compromise malware-delivery capability. A multi-country joint cybersecurity advisory, AA26-281A, provides technical details, indicators of compromise, observed techniques and mitigations associated with Integrity Tech-enabled activity.

This note does not make an independent attribution. It separates the public source record from MANDID's defensive analysis and uses the advisory as a starting point for authorized investigation.

## What the seizure changes—and what it does not

A domain seizure can interrupt authentication, command routing, payload delivery and operator workflow. It can also create a short period in which affected operators must rebuild infrastructure.

It does not automatically:

- remove malware or persistence from victim systems;
- revoke stolen credentials, sessions or application tokens;
- restore altered web content or configurations;
- prove that email, files or credentials were not exfiltrated;
- patch exposed services or close unnecessary ports;
- replace evidence-led scoping and recovery.

Treat the disruption as a hunting window, not an all-clear signal.

## Source-led activity map

The following activity areas come from the DOJ release and AA26-281A. They are not proof of compromise by themselves.

### Reconnaissance and exposed services

The advisory describes automated scanning, large-scale botnets and hands-on exploitation. It lists common scanning focus on ports 21, 22, 53, 80, 443 and 1080 and describes the use of multiple open-source scanners alongside Microscan.

Defensive questions:

- Which internet-facing services were intentionally exposed during the relevant period?
- Did external scan volume or source diversity change unexpectedly?
- Were administrative, file-transfer, DNS, web or proxy services reachable without a documented need?
- Do vulnerability-management records show known weaknesses on those systems?
- Are firewall, reverse-proxy, web-server and cloud-edge logs retained long enough to investigate?

Do not classify ordinary internet scanning as a confirmed intrusion. Correlate it with authentication, application, endpoint and outbound-network evidence.

### Web exploitation and credential harvesting

AA26-281A describes observed cross-site scripting activity used to alter web content and present credential fields. It also describes follow-on malware behavior associated with credential and email access.

Defensive questions:

- Are there unexpected changes in web templates, JavaScript, HTML, plugins or application configuration?
- Do web logs show unusual requests immediately before a content change or authentication anomaly?
- Were credentials entered into an unexpected form or a page served from an unusual path?
- Did endpoint or email activity begin shortly after the suspected web event?

Preserve the page source, relevant files, hashes, web logs, database audit records and screenshots before remediation when it is safe to do so.

### Exchange password spraying and account access

The advisory describes password spraying and guessing against Microsoft Exchange interfaces, including ECP, EWS, OAB, OWA, RPC, API, MAPI, PowerShell, Autodiscover and ActiveSync.

Defensive questions:

- Are there repeated failed authentications spread across many accounts?
- Did one source, network or user agent test multiple Exchange interfaces?
- Were successful logins preceded by spraying activity?
- Did a successful login create inbox rules, forwarding, OAuth grants, application passwords or new MFA methods?
- Do cloud identity, Exchange, VPN and endpoint timelines agree?

Resetting a password alone may not revoke sessions, tokens, forwarding rules or persistence. Follow the organization's approved identity-recovery process.

### VPN software as persistence

AA26-281A describes the use of SoftEther clients for persistence and command-and-control obfuscation. It notes that installers may be named like common Windows processes, including `conhost.exe` or `dllhost.exe`, and configured to reconnect at startup.

Defensive questions:

- Is SoftEther or another unexpected VPN client installed on affected systems?
- Is the binary in an unusual path, unsigned or inconsistent with the approved software inventory?
- What parent process, account and installation time are recorded?
- Are there new services, startup entries, scheduled tasks or configuration files?
- Which destinations did the client contact, and was the traffic expected?

SoftEther is legitimate software. Product presence or a familiar filename is not sufficient for a finding. Validate provenance, path, signature, configuration, timing and network behavior.

### Email collection, staging and exfiltration

The advisory describes scripts and utilities used to access mailboxes, compress data and transfer emails or credentials. It also notes discreet staging filenames intended to blend into normal content.

Defensive questions:

- Were EWS or other mailbox APIs used at unusual volume or by unexpected accounts?
- Did a web or application server create archives, scripts or image-like files that do not match their extensions?
- Are there large or repeated outbound transfers after authentication anomalies?
- Did processes access mail, database or credential stores outside normal operating patterns?
- Is outbound traffic visible at the proxy, firewall, endpoint and cloud-service layers?

Do not rely on filenames alone. Record hashes, file type, owner, timestamps, process lineage and destinations.

## First 24-hour defensive workflow

### 1. Open and govern the incident

- Assign an incident owner, scribe and technical leads.
- Record the detection source, affected systems and current business impact.
- Separate confirmed facts, hypotheses and external reporting.
- Establish a restricted evidence location and approved communications channel.

### 2. Preserve high-value evidence

- Export identity and Exchange audit logs before retention windows expire.
- Preserve firewall, VPN, proxy, DNS, web-server and cloud-edge logs.
- Capture relevant endpoint triage data, running processes, services, network connections and persistence entries.
- Hash suspicious files and record paths, owners, timestamps and acquisition method.
- Preserve suspected web content and configuration before replacement.

### 3. Scope by workflow, not one indicator

- Correlate scanning with authentication, exploitation, persistence and outbound activity.
- Search for password spraying across all relevant Exchange interfaces.
- Review unexpected VPN software, services and startup persistence.
- Check mailbox access, forwarding, rules, OAuth grants and unusual export activity.
- Identify systems sharing credentials, administrative paths or trust relationships with confirmed assets.

### 4. Contain deliberately

- Restrict affected accounts, sessions and tokens according to the identity-response plan.
- Isolate confirmed or strongly suspected systems while preserving necessary evidence.
- Disable unused services and ports.
- Restrict administrative access paths and enforce MFA where supported.
- Block confirmed malicious infrastructure using the current official advisory data, not an old copied list.

### 5. Eradicate and recover

- Patch affected internet-facing and collaboration systems.
- Remove unauthorized tools, persistence and configuration changes.
- Rebuild systems when integrity cannot be established.
- Rotate credentials from a known-clean administrative environment.
- Validate logging, controls and backup integrity before reconnection.
- Monitor recovered assets for repeated scanning, authentication and outbound patterns.

## Evidence worksheet

```text
Incident ID:
Date and time opened (UTC):
Incident owner:
Detection source:

Potentially affected systems:
Internet-facing services:
Exchange or identity tenants:
Observed scanning window:
Observed authentication anomalies:
Observed web changes:
Unexpected VPN or remote-access software:
Mailbox or data-access anomalies:
Outbound destinations and transfer volumes:

Evidence preserved:
Log retention gaps:
Confirmed indicators and source date:
Related but unconfirmed observations:

Containment decisions:
Credentials, sessions or tokens revoked:
Systems isolated:
Services or ports restricted:
Patches or configuration changes applied:

Recovery validation owner:
Monitoring period:
Open risks and follow-up actions:
```

## Indicator-handling rules

- Retrieve IOCs from the current official advisory or its STIX files.
- Record the advisory version and retrieval time.
- Treat an IOC match as a triage lead, not automatic attribution.
- Correlate domain, IP, file and account evidence with time and system context.
- Do not publish client matches, internal addresses, credentials or active incident evidence in a public issue.
- Recheck infrastructure before blocking because ownership and resolution can change.

## Escalation triggers

Escalate to the incident lead when evidence shows or strongly suggests:

- successful authentication after a spraying pattern;
- unauthorized inbox rules, forwarding, OAuth grants or MFA changes;
- unexpected VPN persistence or remote-access tooling;
- altered web content or credential-harvesting behavior;
- suspicious mailbox, database or credential-store access;
- staged archives or unexplained outbound transfers;
- activity on critical infrastructure, regulated systems or high-value identities;
- loss of log coverage needed to establish scope.

Legal, regulatory, insurer and law-enforcement notifications depend on jurisdiction and facts. Route those decisions through the responsible organizational stakeholders.

## Sources

### Primary sources

- [U.S. Department of Justice — Justice Department and FBI Seize Vulnerability Scanning and Spear Phishing Tools Operated and Used by China-State Sponsored Hackers](https://www.justice.gov/opa/pr/justice-department-and-fbi-seize-vulnerability-scanning-and-spear-phishing-tools-operated)
- [Joint Cybersecurity Advisory AA26-281A — Chinese Government-linked Cyber Threat Actors Combine Automated and Hands-on Hacking Tools to Steal Sensitive Data](https://www.ic3.gov/CSA/2026/261008.pdf)

### Additional reporting

- [The Record — International coalition seizes tools used by cyber firm behind Flax Typhoon](https://therecord.media/flax-typhoon-china-tools-integrity-tech-international-takedown)

## Related MANDID analysis

[Microscan and FishHub seizures expose China’s contractor-hacker pipeline](https://mandidsecurity.com/integrity-technology-group-microscan-fishhub-seizure/?utm_source=github&utm_medium=repository&utm_campaign=mandid-threat-intelligence-notes&utm_content=microscan-fishhub-defensive-hunting)

## Need incident-response help?

MANDID supports incident containment, investigation, recovery and hardening for websites, web applications, servers and online accounts.

[Request cybersecurity help from MANDID](https://mandidsecurity.com/cyber-help/?utm_source=github&utm_medium=repository&utm_campaign=mandid-threat-intelligence-notes&utm_content=microscan-fishhub-defensive-hunting)
