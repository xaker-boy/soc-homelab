# soc-homelab
Open-source mini SOC lab: Wazuh SIEM, MITRE ATT&amp;CK detection rules, and automated incident response
# SOC Home Lab — Wazuh SIEM + MITRE ATT&CK Detection

Open-source mini Security Operations Center built on Oracle Cloud, featuring
a Wazuh SIEM deployment, a monitored endpoint, and a live brute-force attack
simulation mapped to MITRE ATT&CK.

## What this project demonstrates

- Deploying a production-grade open-source SIEM (Wazuh 4.14) on cloud
  infrastructure
- Connecting and monitoring an endpoint via the Wazuh agent
- Simulating a real SSH brute-force attack (Hydra) against the monitored
  endpoint
- Verifying automatic detection, severity scoring, and MITRE ATT&CK mapping
  (Technique: Brute Force, Tactic: Credential Access) with zero custom rules
- Diagnosing and resolving real infrastructure issues (disk exhaustion,
  low-memory host instability, OpenSearch read-only index locks)

## Architecture

Kali Linux (attacker) → Target server (Wazuh agent) → Wazuh manager
↓
Indexer → Dashboard

- **Wazuh server:** Oracle Linux 9, ARM (Ampere A1), Wazuh 4.14.8
  (indexer + manager + filebeat + dashboard, all-in-one)
- **Target server:** Oracle Linux 9, Wazuh agent, monitored endpoint
- **Attacker:** Kali Linux (local VM), Hydra for brute-force simulation

## Results

- Attack: 8-password SSH brute-force against a test account
- Detection: 256 events logged within the attack window, including a
  level-10 correlation rule (`id: 2502`, repeated login failures)
- MITRE mapping: automatically tagged as **Tactic: Credential Access**,
  **Technique: Brute Force** — no custom detection rules were needed for
  this baseline attack
- Compliance mapping included out of the box: PCI DSS, GDPR, HIPAA,
  NIST-800-53

See `screenshots/` for dashboard evidence.

## Troubleshooting log

Real issues encountered and resolved during deployment are documented in
[`docs/troubleshooting.md`](docs/troubleshooting.md) — includes root-cause
diagnosis for disk exhaustion, OpenSearch index locking, and low-RAM host
instability.

## Status

This is an active, ongoing bachelor's thesis project. Next phases: SOAR
automation (TheHive/Cortex), custom Sigma rules, and detection/response
time metrics.
