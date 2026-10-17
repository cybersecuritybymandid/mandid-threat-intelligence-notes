# CI/CD Supply-Chain Compromise: Secrets, Provenance and Containment

## Scope and safety

This note turns public reporting about the alleged TeamPCP software supply-chain campaign into a defensive response workflow for repository owners, package maintainers, build-platform administrators and authorized incident responders.

It is not an intrusion or malware-development guide. Do not access, test, scan or modify any repository, runner, registry, package or customer environment unless its owner has explicitly authorized the work. Keep credentials, private artifacts, personal data and active incident evidence out of public issues and commits.

## Source record and confidence boundaries

- On August 27, 2026, the U.S. Attorney's Office for the Northern District of California announced an indictment alleging that Ruben Ian Thomson and co-conspirators exploited trusted software supply-chain security tools and injected malicious code.
- DOJ says the alleged code scanned downstream customer environments for sensitive data, attempted to exfiltrate data, enabled persistent access and supported extortion.
- Ars Technica reported that Australian authorities attributed more than 1,000 affected organizations to the wider TeamPCP campaign and described self-propagation through software packages and CI/CD pipelines.
- KrebsOnSecurity reported additional campaign context and arrests in Australia.

An indictment contains allegations, not findings of guilt. The defendant is presumed innocent unless proven guilty. Public reporting does not identify every affected organization, package, credential or artifact. Use vendor notices, registry data and your own preserved telemetry to determine exposure.

## Why this incident class matters

A compromised build or security tool can cross multiple trust boundaries at once:

1. It executes where source code and dependencies are assembled.
2. It may see registry, signing, cloud and deployment credentials.
3. It can influence artifacts that downstream systems install automatically.
4. Stolen automation credentials can make malicious activity resemble a normal release.
5. The systems producing logs or attestations may themselves be untrusted.

Treat a suspected pipeline compromise as both a release-integrity incident and a credential-exposure incident.

## Trigger conditions

Escalate to the software-supply-chain response path when one or more of these conditions are credible:

- a dependency, action, plugin, container image or build tool is confirmed malicious or unexpectedly modified;
- a maintainer, bot or publisher credential is used from an unrecognized workflow, runner, location or time;
- a package version, tag, release or artifact appears without the expected review and approval trail;
- a build produces an unexplained network connection, secret access, file collection or persistence event;
- artifact hashes, signatures, attestations or dependency manifests do not match the release record;
- downstream consumers report behavior that cannot be reproduced from the reviewed source and known-good build process.

## First-response priorities

### 1. Establish authority and incident ownership

Record:

- the incident commander and technical owners;
- the repositories, registries, build systems and cloud accounts in scope;
- who authorized the release freeze, credential revocation, evidence collection and customer communications;
- legal, privacy, insurer and regulatory contacts that may need to be involved.

### 2. Freeze propagation without destroying evidence

- Pause affected publishing and deployment workflows.
- Block promotion of suspect packages, images and releases.
- Preserve the current state of relevant runners before rebuilding them when operationally safe.
- Avoid deleting releases, workflow runs or package versions until their metadata and contents are preserved.
- Notify downstream owners through the approved incident channel when continuing automation could spread risk.

### 3. Define the exposure window

Anchor the timeline to the earliest suspicious change or credential use, not only the first alert.

Include:

- source commits and branch-protection events;
- workflow, action, plugin and runner-image changes;
- dependency-lock and package-manager metadata;
- package publication, image push, signing and deployment events;
- secret reads, token exchanges and cloud audit events;
- downstream downloads and deployments.

## Evidence map

| Evidence source | Questions to answer | Preserve |
|---|---|---|
| Source-control audit log | Who changed code, workflows, protections, environments or deploy keys? | Native export, event IDs, timestamps and actor identities |
| CI/CD run history | Which workflow, revision, runner and permissions produced each artifact? | Run metadata, logs, job definitions and runner identifiers |
| Runner telemetry | Did the build access unexpected files, processes, networks or secrets? | Process, network, filesystem and EDR telemetry |
| Package/container registry | Which identity published, retagged, deprecated or removed content? | Publication logs, package metadata, manifests and digests |
| Secret manager and identity provider | Which secrets or federated roles were requested and used? | Audit events, token claims, session data and policy changes |
| Signing service | Were signing keys or identities used outside the approved release path? | Signing events, certificate data, signatures and policy decisions |
| Artifact repository | Do stored outputs match release hashes and attestations? | Exact artifacts, hashes, signatures, provenance and retention metadata |
| Deployment platform | Where did suspect artifacts run and what did they reach? | Deployment history, runtime inventory, workload identity and network logs |

Preserve original time zones and clock-skew information. Hash exported evidence and maintain a collection log.

## Credential blast-radius review

Do not rotate only the token named in the first alert. Build environments commonly hold several credential classes.

| Credential class | Questions | Defensive response |
|---|---|---|
| Package publisher | Could it publish, stage, unpublish or change distribution tags? | Revoke; review every publication and tag change; restore least privilege |
| Source-control token | Could it read secrets, modify workflows, create releases or bypass protections? | Revoke sessions and tokens; review grants, apps, deploy keys and branch events |
| Cloud/workload identity | Which accounts, projects, roles and regions were reachable? | Disable or constrain trust; review token exchanges and resource changes |
| Signing identity | Could it sign packages, binaries, containers or provenance? | Suspend the identity; verify every signature in the window; reissue if necessary |
| Deployment credential | Which environments and clusters could it change? | Revoke; compare deployed state with approved manifests; examine lateral access |
| Read-only dependency token | Which private source or package data was visible? | Rotate; review downloads and subsequent misuse; scope possible disclosure |

Perform rotations from a clean, separately trusted administrative system. Record old credential identifiers, revocation time, replacement scope and validation results without storing secret values in the case record.

## Artifact and downstream scoping

Build a release ledger for the full exposure window:

| Artifact | Source revision | Workflow revision | Runner/image | Digest | Signature or provenance | Published/deployed to | Status |
|---|---|---|---|---|---|---|---|
|  |  |  |  |  |  |  |  |

For every suspect output:

- identify direct and transitive consumers;
- determine whether the consumer executed install-time scripts or privileged build steps;
- review whether the artifact reached developer workstations, CI runners, production or customer environments;
- compare the artifact to a clean rebuild from the reviewed source;
- document differences before replacement or removal;
- provide downstream owners with immutable identifiers such as package version, container digest and artifact hash.

Do not use a filename or mutable tag as the sole indicator of identity.

## Provenance and reproducibility checks

SLSA defines provenance as verifiable information describing where, when and how an artifact was produced. Use provenance to support trust decisions, but validate the builder and policy behind the attestation.

Ask:

- Does the attestation bind the artifact digest to the expected repository, revision and build workflow?
- Is the builder identity one the organization actually controls and monitors?
- Were external inputs and dependencies resolved from approved sources?
- Can the artifact be rebuilt in a clean environment with a matching or explainable result?
- Was provenance generated inside the same potentially compromised environment, and if so, what independent evidence can corroborate it?

An attestation created by an untrusted builder is evidence about what that builder claimed, not automatic proof that the output is safe.

## GitHub Actions hardening after containment

GitHub's secure-use guidance recommends pinning third-party actions to full-length commit SHAs because the full SHA is the immutable reference for an action release.

After immediate containment:

- reduce default `GITHUB_TOKEN` permissions and grant job-level permissions explicitly;
- pin third-party actions to reviewed full-length commit SHAs;
- restrict which actions and reusable workflows may run;
- protect deployment environments with required reviewers where appropriate;
- separate build, sign and deploy roles so one job cannot silently control the whole release path;
- treat pull requests from forks and untrusted contributors as unable to access release credentials;
- review organization, repository and environment secrets for unnecessary reach;
- prefer ephemeral, clean runners for sensitive release jobs.

## npm publishing hardening

npm's current trusted-publishing guidance uses OIDC so supported CI/CD workflows can publish with short-lived, workflow-specific credentials instead of long-lived npm tokens.

Where supported and appropriate:

- migrate publishing to a narrowly defined trusted publisher;
- disable traditional token-based publishing after the OIDC path is validated;
- use stage-only publishing with human review when the release model permits it;
- keep private-dependency credentials read-only and separate from publish authority;
- verify the package's repository identity and the exact workflow authorized to publish;
- retain package provenance and review unexpected version or distribution-tag changes.

OIDC reduces reusable secret exposure; it does not make a compromised authorized workflow trustworthy. Protect the workflow file, repository identity and release approvals.

## Clean recovery

Restore trust in this order:

1. Establish a clean administrative and evidence-analysis environment.
2. Revoke exposed credentials and invalidate unauthorized sessions.
3. Review and repair source, workflow, registry and cloud trust policies.
4. Rebuild runners and release infrastructure from known-good definitions.
5. Rebuild affected artifacts from reviewed source and locked dependencies.
6. Generate and verify new hashes, signatures and provenance.
7. Replace or withdraw suspect releases through an approved, documented process.
8. Validate downstream systems and rotate any credentials they exposed.
9. Resume publishing in a limited canary path before restoring full automation.
10. Document residual uncertainty and the evidence supporting the decision to reopen releases.

## Recovery validation checklist

- [ ] A complete exposure window is documented.
- [ ] Every artifact built or published in that window has an owner and status.
- [ ] Suspect credentials, sessions, deploy keys and app grants are revoked or constrained.
- [ ] Source and workflow protections are restored and independently reviewed.
- [ ] Clean runners and trusted base images are in use.
- [ ] Release actions and dependencies are pinned or governed by approved policy.
- [ ] Rebuilt artifacts have reviewed digests, signatures and provenance.
- [ ] Downstream consumers received actionable immutable identifiers and guidance.
- [ ] Monitoring covers unexpected publishing, secret access and release-policy changes.
- [ ] The incident commander has recorded the decision and evidence for resuming releases.

## Short incident decision record

```text
Incident:
Decision time (UTC):
Authorized by:

Affected repositories/workflows:
Affected registries/packages/artifacts:
Exposure window:
Credentials revoked or constrained:
Downstream environments notified:

Evidence supporting containment:
Evidence supporting clean rebuild:
Residual uncertainty:

Release decision:
[ ] remain frozen
[ ] limited canary
[ ] resume with conditions

Next review time:
Owner:
```

## Sources and further guidance

- [U.S. Department of Justice - Australian Man Indicted for TeamPCP Cyberattacks on Software Supply Chain](https://www.justice.gov/usao-ndca/pr/australian-man-indicted-teampcp-cyberattacks-software-supply-chain)
- [Ars Technica - Authorities arrest two alleged TeamPCP members](https://arstechnica.com/security/2026/08/authorities-arrest-2-alleged-members-of-prolific-hacking-group-teampcp/)
- [KrebsOnSecurity - Two alleged TeamPCP hackers arrested in Australia](https://krebsonsecurity.com/2026/08/two-alleged-teampcp-hackers-arrested-in-australia/)
- [GitHub Docs - Secure use reference for GitHub Actions](https://docs.github.com/en/actions/reference/security/secure-use)
- [npm Docs - Trusted publishing for npm packages](https://docs.npmjs.com/trusted-publishers/)
- [SLSA v1.2 - Provenance](https://slsa.dev/spec/v1.2/provenance)
- [MANDID analysis - The Shai-Hulud Supply-Chain Worm and the TeamPCP Arrests](https://mandidsecurity.com/teampcp-supply-chain-attacks-arrests-shai-hulud-worm/?utm_source=github&utm_medium=repository&utm_campaign=mandid-threat-intelligence-notes&utm_content=ci-cd-supply-chain-compromise-response)

## Need help with an active incident?

MANDID supports remote incident containment, investigation, recovery and hardening for websites, web applications, servers, online accounts and delivery pipelines.

[Request cybersecurity help from MANDID](https://mandidsecurity.com/cyber-help/?utm_source=github&utm_medium=repository&utm_campaign=mandid-threat-intelligence-notes&utm_content=ci-cd-supply-chain-compromise-response)

## Responsible use

Authorized and defensive use only. This note provides general defensive information, not legal, regulatory or incident-specific advice.
