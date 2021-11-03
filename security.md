---
layout: default
title: Security
nav_order: 20
---
# Data Science - Best Practices &middot; [![License](https://img.shields.io/badge/license-CC%20BY%204.0-blue)](./LICENSE.txt)

## Security

### General IT Security

Generally, AI systems at their core are IT systems, therefore they're threatened by the same security risk / issues as any other IT system.
As a first step it is recommended to think about the potential threads for the system.
The STRIDE model helps doing that:

<p align="center">
    <img src="https://img.ibxk.com.br/2021/01/11/ibm-11195508258441.png" alt="STRIDE"> <br/>
  <sub>&copy; <a href="https://www.tecmundo.com.br/software/209242-modelagem-ameacas-contexto-arquiteturas-de-microsservico.htm">Equipe TecMundo</a></sub>
</p>

Depending on the individual threats to the system the [IBM Secure Engineering Framework](https://www.redbooks.ibm.com/redpapers/pdfs/redp4641.pdf) can be used to mitigate these threats and their associated risk.

- [IBM Secure Engineering Portal](https://www.ibm.com/security/secure-engineering/)
- [IBM Secure Engineering Framework](https://www.redbooks.ibm.com/redpapers/pdfs/redp4641.pdf)
- [IBM Architecture Center - Application Security](https://www.ibm.com/cloud/architecture/architectures/securityArchitecture/security-for-application)
- [IBM Architecture Center - Data Security](https://www.ibm.com/cloud/architecture/architectures/securityArchitecture/security-for-data)
- [IBM Architecture Center - Secure DevOps](https://www.ibm.com/cloud/architecture/architectures/securityArchitecture/implement-secure-devops)

### AI Specific Security

Obviously there some key differences between AI and regular IT systems and the same is true security risks. The following article gives an introduction into the ML specific security risks - [Security Attacks: Analysis of Machine Learning Models](https://dzone.com/articles/security-attacks-analysis-of-machine-learning-mode)

- [Microsoft - Threat Taxonomy for ML](https://docs.microsoft.com/en-us/security/engineering/failure-modes-in-machine-learning)
- [Microsoft - Threat Modeling for AI/ML](https://docs.microsoft.com/en-us/security/engineering/threat-modeling-aiml)

## Privacy by Design

Most AI systems are very "data-hungry" and this might create a conflict with [GDPR](https://en.wikipedia.org/wiki/General_Data_Protection_Regulation) or [CCPA](https://en.wikipedia.org/wiki/California_Consumer_Privacy_Act).
To prevent any privacy issues later breaking the whole system, it is recommended to build in privacy  by design.
The cheat sheet below helps you when you design your AI systems:

<p align="center">
    <img src="https://img.ibxk.com.br/2021/01/11/ibm-11195749728445.png" alt="GDPR Basics"> <br/>
  <sub>&copy; <a href="https://www.tecmundo.com.br/software/209242-modelagem-ameacas-contexto-arquiteturas-de-microsservico.htm">Equipe TecMundo</a></sub>
</p>

> **Important:** GDPR does **not only** cover personal data but also **business sensitive data**!
