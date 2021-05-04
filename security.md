# Data Science - Best Practices &middot; [![License](https://img.shields.io/badge/license-CC%20BY%204.0-blue)](./LICENSE.txt)

## Table of Content

- [Chapter 1 - Introduction](./readme.md#chapter-1---introduction)
- [Chapter 2 - Project Team (Design)](./project_team.md#chapter-2---project-team)
- [Chapter 3 - Architecture (Deploy)](./architecture.md#chapter-3---architecture)
- [Chapter 4 - Source Code (Engineer)](./source_code.md#chapter-4---source-code)
- [Chapter 5 - Documentation (Engineer)](./documentation.md#chapter-5---documentation)
- [Chapter 6 - Versioning (Engineer)](./versioning.md#chapter-6---versioning)
- [Chapter 7 - Data Management (Engineer)](./data_management.md#chapter-7---data-management)
- [Chapter 8 - Dependency Management (Engineer)](./dependency_management.md#chapter-8---dependency-management)
- [Chapter 9 - Configuration Management (Engineer)](./configuration_management.md#chapter-9---configuration-management)
- [Chapter 10 - Testing (Engineer)](./testing.md#chapter-10---testing)
- [Chapter 11 - Quality Measurements (Monitor)](./quality_measurements.md#chapter-11---quality-measurements)
- [Chapter 12 - Model Training (Engineer)](./model_training.md#chapter-12---model-training)
- [Chapter 13 - Distribution (Deploy)](./distribution.md#chapter-13---distribution)
- [Chapter 14 - Cloud-Deployment (Deploy)](./cloud_deployment.md#chapter-14---cloud-deployment)
- [Chapter 15 - Edge Deployment (Deploy)](./edge_deployment.md#chapter-15---edge-deployment)
- [Chapter 16 - Monitoring (Monitor)](./monitoring.md#chapter-16---monitoring)
- [Chapter 17 - Automation (Scalability)](./automation.md#chapter-17---automation)
- [Chapter 18 - Scaling (Scalability)](./scaling.md#chapter-18---scaling)
- [Chapter 19 - Sizing (Scalability)](./sizing.md#chapter-19---sizing)
- [Chapter 20 - Security (Engineer)](./security.md#chapter-20---security)
- [Chapter 21 - Usage Recommendations (Scalability)](./recommendation.md#chapter-21---usage-recommendations)
- [License & Contributing](./license.md)

## Chapter 20 - Security

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
