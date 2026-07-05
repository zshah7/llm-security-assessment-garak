# LLM Security Assessment — GPT-3.5-Turbo

An independent adversarial robustness assessment of `gpt-3.5-turbo`, conducted using [NVIDIA Garak](https://github.com/NVIDIA/garak), an open-source LLM vulnerability scanner.

📄 **[Read the full report](./LLM_Security_Assessment_Report.pdf)**

## Overview

This project simulates a real-world AI red team engagement: scoping a target model, running automated adversarial probes against it, evaluating the results, and producing a structured findings report with severity ratings and remediation guidance.

- **Target:** `gpt-3.5-turbo` (OpenAI API)
- **Tool:** NVIDIA Garak v0.15.1
- **Attack surface tested:** Prompt injection, jailbreak/persona-override, encoding-based obfuscation
- **Scale:** 9,988 individual adversarial attempts across 12 probes and 5 detectors

## Key Findings

| Vulnerability Class | Peak Success Rate | Severity |
|---|---|---|
| Prompt injection (long-context burying) | 76.2% | Critical |
| Encoding obfuscation (Hex / Base16) | 67–69% | High |
| Jailbreak / persona override (DAN family) | 56–60% | High |
| Encoding obfuscation (Base64) | 28–51% | Medium |
| Encoding obfuscation (esoteric schemes*) | <0.5% | Negligible |

\* Base32, Base2048, Ascii85, UUencoding, Braille, Atbash

The model's resistance to encoding-based attacks was uneven — strong against esoteric schemes, weak against common ones like Hex and Base64. This suggests safety behavior tracks the prevalence of an encoding in training data rather than reflecting a general decoding-based defense, which is a relevant consideration when evaluating whether a model's safety training will generalize to novel obfuscation techniques.

## Methodology

Garak's probewise harness was run against the target with five generations sampled per prompt to account for response variance. Each attempt was scored by a corresponding detector (e.g., `dan.DAN`, `mitigation.MitigationBypass`, `promptinject.AttackRogueString`, `encoding.DecodeMatch`) that determines whether the model exhibited the targeted vulnerable behavior — adopting a jailbroken persona, leaking an injected string, or decoding and complying with an obfuscated payload.

## Why This Project

Built to apply a security analysis background to the emerging discipline of AI/LLM red teaming — translating vulnerability identification, evidence-based risk rating, and remediation reporting skills from traditional cybersecurity work into the AI security space.

## Disclosure Note

Findings reflect publicly known weaknesses in this model family and are presented as a demonstration of an adversarial testing and reporting workflow, not as a novel security disclosure. Raw attack logs are intentionally excluded from this repository; only the redacted, analysis-level report is published.

## Tools & Skills Demonstrated

`Python` · `NVIDIA Garak` · `Adversarial ML Testing` · `Prompt Injection Analysis` · `Vulnerability Reporting` · `Risk Rating` · `Technical Writing`

---

*Full methodology, detailed findings, and remediation recommendations are available in the [PDF report](./LLM_Security_Assessment_Report.pdf).*
