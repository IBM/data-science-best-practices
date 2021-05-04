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

## Chapter 19 - Sizing

The following table lists some of the most common drives of complexity and size of ML solutions and projects.
Not all are directly related to the ML model, but still heavily influence the sizing.

| Factor | Type | Description |
| ------ | ---- | ----------- |
| Business Problem Complexity | Business | The number of use cases and the use cases themselves are typically defining the business problem and therefore also its complexity. |
| Legal, Ethical & Regulatory Requirements | Business | This covers topics like [GDPR](https://en.wikipedia.org/wiki/General_Data_Protection_Regulation) or [CCPA](https://en.wikipedia.org/wiki/California_Consumer_Privacy_Act) but also the "[Rome Call for AI Ethics](https://www.romecall.org/wp-content/uploads/2021/02/AI-Rome-Call-x-firma_DEF_DEF_con-firme_.pdf)". (Further reading material on ethics: [Everyday Ethics for AI](https://www.ibm.com/watson/assets/duo/pdf/everydayethics.pdf)) |
| Non-Functional Requirements (NFRs) | Technical | NFRs cover topics like Performance (including Capacity), Volumetrics, Scalability, Security, Regulatory Compliance, Usability, Maintainability (including Flexibility), Availability (including Recoverability and Reliability), Manageability, Environmental (including Safety). |
| Stakeholder Complexity | Business | This refers to the stakeholders of the project. This is driven mainly by the number of stakeholders and their relationships towards the project and to each other. |
| User Complexity | Business | The users as such should be included in the stakeholders, however depending on the skills, disparity and needs they often drive the complexity by e.g., their interface requirements, explainability needs and availability demands. |
| Data Sourcing Complexity | Technical | The complexity of data sources is mainly driven by the amount of data sources, their availability, accessibility and disparity. |
| Data Complexity | Technical | The data complexity is mainly driven by the size of the data (e.g., row size, number of total rows, total storage size) and the complexity of the data model including its consistency within the data. Especially the uniqueness of (data) keys or its absence can be driving the complexity. |
| Hypothesis Complexity | Technical | The hypothesis is usually  directly derived from the use case. This covers the complexity of the hypothesis is self but also the complexity of testing / verifying the hypothesis.  |
| Interface Complexity | Technical | The interface complexity is driven by the amount, type disparity and individual complexity. It covers both the interfaces with data sources (see above) but also the interfaces during interference. |
| Solution Complexity | Technical | The complexity of the solution is mainly driven by the use cases, functional and non-functional requirements (see above) but also what is technically possible. This defines how complex the required architecture will need to be. For AI systems in particular the solution complexity is driven by e.g., the deployment type (cloud vs. edge) and the training mode (one-time vs. batch vs. continuous). |
| Model Complexity | Technical | The model complexity is driven by the use case and its solution outline. This is then often reflected in the number of models needed, their size and their individual complexity. |
