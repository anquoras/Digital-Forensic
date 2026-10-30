# Digital Forensics Lab Reports

A growing collection of hands-on digital forensics investigations, documented as structured lab reports. Each report walks through a scenario end to end — acquisition, analysis, findings, and conclusions — using industry-standard tooling and a repeatable methodology.

The labs span several forensic disciplines and are drawn from a mix of self-authored scenarios and platform-based challenges (HackTheBox / TryHackMe-style). The goal is to demonstrate practical DFIR capability: the ability to take raw evidence, reason about it methodically, and produce a clear, defensible write-up.

![Focus](https://img.shields.io/badge/focus-Digital%20Forensics-blue)
![Discipline](https://img.shields.io/badge/discipline-DFIR-informational)
![Status](https://img.shields.io/badge/status-actively%20updated-brightgreen)

---

## Categories

The repository is organised by forensic discipline. Labs are grouped under the category that best matches the primary evidence source.

### Disk & File System Forensics
Investigations centred on disk images and file systems — evidence acquisition and imaging, file carving and deleted-file recovery, partition and file-system structure analysis, and timeline reconstruction.

### Memory Forensics
Analysis of volatile memory captures — identifying running and hidden processes, detecting code injection and malicious artefacts, and recovering evidence that never touched disk.

### USB / Removable Media & Malware Forensics
Cases involving removable media and malicious files — tracing device usage and data movement, triaging suspicious binaries, and examining email and phishing artefacts.

---

## Lab Index

| Lab | Category | Source | Status |
|-----|----------|--------|--------|
| [Operation Spoiler - USB Forensic Analysis](./Operation-Spoiler-USB-Forensic-Analysis) | USB / Removable Media | Self-made scenario | ✅ Complete |

*More labs across all three categories are in progress and will be added here as they are completed.*

---

## Tools & Techniques

The reports draw on a standard DFIR toolkit, including:

- **Disk imaging and analysis**: Autopsy, The Sleuth Kit, FTK Imager, `dd`
- **Memory analysis**: Volatility
- **Network and packet analysis**: Wireshark, tshark
- **Malware triage**: Static and dynamic analysis, hashing, string and metadata inspection
- **Supporting utilities**: Hex editors, `exiftool`, `binwalk`, and command-line forensics tooling

Each lab lists the specific tools used for that investigation in its own report.

---

## Methodology

Every lab follows the same investigative flow, regardless of category:

1. Establish the scenario and define the questions the investigation must answer.
2. Acquire and verify the evidence, preserving integrity throughout.
3. Analyse the evidence systematically, following where the artefacts lead.
4. Document findings with supporting evidence, including any indicators of compromise.
5. Conclude with a clear answer to the original questions and lessons drawn from the case.

---

## Repository Structure

```
Digital-Forensic/
├── README.md
├── Operation-Spoiler-USB-Forensic-Analysis/
│   └── ...
└── <future labs, grouped by category>
```

Each lab folder is self-contained, holding its own report and any supporting artefacts.

---

## Disclaimer

All work here is for educational and portfolio purposes. Reports from platform-based or public challenges focus on methodology and approach rather than reproducing flags, solutions, or copyrighted scenario material. All investigations are conducted on evidence created or lawfully provided for forensic practice.

---

## About

Maintained by **anquoras** - MSc Cyber Security student at the University of Southampton, with hands-on experience in SOC analysis and offensive security. This repository documents ongoing practice in digital forensics and incident response.

Feedback and discussion are welcome via the repository's Issues tab.
