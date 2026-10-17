# MANDID Threat Intelligence Notes

Source-led defensive case notes that turn current cyber incidents, criminal cases and public reporting into practical questions, controls and response actions.

These notes are written for system owners, administrators, defenders and security professionals working on systems they own or are explicitly authorized to protect. They separate reported facts and legal allegations from MANDID's defensive analysis.

## Available notes

- [Ransomware Recovery Vendor Risk: Lessons from the MonsterCloud Allegations](notes/ransomware-recovery-vendor-risk.md) — questions, evidence requirements and authorization controls for selecting a ransomware recovery provider under pressure.
- [Microscan and FishHub: Defensive Hunting After an Infrastructure Seizure](notes/microscan-fishhub-defensive-hunting.md) — a source-led triage and evidence workflow for scanning, Exchange authentication, VPN persistence and email-exfiltration signals described in AA26-281A.
- [PaperCut Exploit Chain: Post-Patch Triage for CVE-2026-81578 and CVE-2026-82078](notes/papercut-post-patch-triage.md) — an authorized defensive workflow for exposure review, evidence preservation, application and host hunting, containment and recovery validation.
- [SonicWall SMA1000: Post-Patch Hunt for CVE-2026-83548 and CVE-2026-83549](notes/sonicwall-sma1000-post-patch-hunt.md) — a gateway-focused workflow for exposure reconstruction, evidence preservation, identity and TOTP review, configuration integrity, containment and trusted recovery.
- [CI/CD Supply-Chain Compromise: Secrets, Provenance and Containment](notes/ci-cd-supply-chain-compromise-response.md) — a release-integrity and credential-response workflow covering evidence preservation, artifact scoping, provenance, clean rebuilds and downstream notification.

## How to use this repository

1. Start with the cited primary sources and check their publication dates.
2. Treat charges and claims as allegations unless a court has established otherwise.
3. Adapt the controls to the affected systems, jurisdiction, insurer requirements and incident-response plan.
4. Record who authorized each material action and communication.
5. Preserve evidence before changing compromised systems whenever it is safe to do so.

## Method

Each note aims to include:

- the source record and confidence boundaries;
- the defensive significance of the case;
- concrete questions and evidence to request;
- a short decision or response template;
- links to authoritative guidance and the related MANDID analysis.

## Need help with an active incident?

MANDID supports remote incident containment, investigation, recovery and hardening for websites, web applications, servers and online accounts.

[Request cybersecurity help from MANDID](https://mandidsecurity.com/cyber-help/?utm_source=github&utm_medium=repository&utm_campaign=mandid-threat-intelligence-notes&utm_content=readme)

## Authoritative defensive resources

- [U.S. Department of Justice — TeamPCP software supply-chain indictment](https://www.justice.gov/usao-ndca/pr/australian-man-indicted-teampcp-cyberattacks-software-supply-chain)
- [GitHub Docs — Secure use reference for GitHub Actions](https://docs.github.com/en/actions/reference/security/secure-use)
- [npm Docs — Trusted publishing for npm packages](https://docs.npmjs.com/trusted-publishers/)
- [SLSA v1.2 — Provenance](https://slsa.dev/spec/v1.2/provenance)
- [SonicWall product notice SNWLID-2026-0016](https://www.sonicwall.com/support/notices/kA1VN000002AXmQ0AW)
- [CISA Known Exploited Vulnerabilities catalog](https://www.cisa.gov/known-exploited-vulnerabilities-catalog)
- [Rapid7 SonicWall SMA1000 threat research](https://www.rapid7.com/blog/post/etr-critical-sonicwall-sma1000-vulnerabilities-cve-2026-83548-cve-2026-83549-exploited-in-the-wild/)
- [PaperCut urgent security advisory](https://www.papercut.com/kb/Main/security-bulletin-27-aug-2026-urgent-security-advisory/)
- [Joint Cybersecurity Advisory AA26-281A](https://www.ic3.gov/CSA/2026/261008.pdf)
- [CISA — #StopRansomware Guide](https://www.cisa.gov/stopransomware/ransomware-guide)
- [FBI Internet Crime Complaint Center — Ransomware](https://www.ic3.gov/CrimeInfo/Ransomware)
- [U.S. Treasury — ransomware reporting, resilience and sanctions considerations](https://home.treasury.gov/news/press-releases/jy0364)

## Responsible use

Authorized and defensive use only. Never access, test or modify systems without the owner's explicit permission. Do not place secrets, personal data, client evidence or active incident details in public issues or commits.

This repository provides general defensive information, not legal, regulatory, insurance or sanctions advice.

## License

Released under the [MIT License](LICENSE).
