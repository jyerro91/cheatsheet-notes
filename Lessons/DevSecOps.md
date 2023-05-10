# DevSecOps

#### Measuring DevSecOps Success
1. Deployment Frequency (fast & frequent release)
2. Lead time (code to cash cycle)
3. Detection of threats, vulnerabilities and malware
4. Mean time to repair and remediation
5. Efficiancy of rollback and recovery


## Chapter 1: Introduction
---
##### Release Management

- Release management is the process of coordinating the deployment of finished application increments to production.
- Staging is the interim area where application sub-releases are tested prior to use by customers.
- In highly regulated industries, many approvals are required before code can be released into prod. 

##### Dynamic vs Static Scanning

`Dynamic scanning` is a method of code analysis that identifies vulnerabilities in a runtime environment.
- Dynamic tests monitor system memory, functional behavior, response time, and the overall performance of the system
- Automated scanning tools can be used to analyze applications for which you do not have access to the original source code.

`Static scanning` is a method of analysis performed in a non-runtime environment.
- Typically a static analysis tools will inspect progm code for all possible runtime behaviors and seek out flaws and potentially vulnerable code.
- Although they were developed separately, static and dynamic scanning are not in opposition to one another.
- There are strengths and weaknessess assocaited with both approaches, and many DevSecOps processes benefit from using both

##### Cloud Controls Matrix (CCM)

- the `Open Web Application Security Project (OWASP)` is an online community project that provides free articles, methodologies, documentation, tools, and technologies for web app security.
- The OWASP Foundation came online on December 1st 2001.
- It was established as a not-for-profit charitable organization in the United States on April 21, 2004.
- OWASP is an international organization and the OWASP Foundation supports OWASP efforts around the world.

##### Federal Information Processing Standards (FIPS)

- the `FIPS` are security standards develop by the U.S federal gov for use in non-mil gov computer systems.

##### Secure Automation
- in `DevSecOps` many of the processes required for software development, testing and deployment are automated.
- Security checks should be implemented for each server and tool to reduce the number of possible `attack vendors` and prevent malfeasance.
- Security practices are different for every team and system, but there are three mamin categories: server hardening, application hardening and identity and access management.

##### Identity and Access Management
- `Identity and Access Management (IAM)` is the process of granting or restricting access to computing resources for individual users group, or system.
- All modern software applications govern user and group authentication but their specific methoods vary.
- Systems that access applications through API's also have frameworks for managing access to those applications.
- To harden servers and applications in a DevSecOps pipeline, teams have to implement security practices based on the particular tooling being used.

##### Server Hardening
- `Server hardening` is the practice of enhancing each server's security
- Teams can consult benchmarks from CIS and applications such as OpenSCAP to review possible server vulnerabilities and determine what steps to take to mitigate risks.
- A server must be hardened before the applications and tooling hosted on the server can be secured.

##### Application Hardening
- `Application harderning` is the practice of enhancing an application or frameworks security according to the providers recom.
- Most DevSecOps tooling involves automated integration with other third-party systems through APIs.
- The process of hardening an application includes both the initial implementation of security assets and ongoing governance over time.

##### IAM Fundamentals
- `Indentity repositories` are systems that sotre information about all the users and groups within an enterprise in a single place.
- `Access keys` are encrypted keys that enable applications to securely access servers.
- `Signatures` and `certificates` allow programs to verify the source and authenticity of digital assets.
- `Vaults` store secrets and encrypt login credentials to prevent attackers from accessing them.


## Chapter 2: Secure Software Onboarding
---
#### Lesson 1: Clean Repositories

##### Security in Public Repositories
- `Attack vectors` are often exploited in public systems.
- Since the systems are not on-premise, they must be hardened and maintained by the organizations that provide them.

##### Malware
- `Malware` exists in most public repos, and some of the most popular software components in use today are know to be vulnerable.
- If the software in public repos is transferred to internal systems, the malware is brought into the on-prem infrastructure

##### Shared Repositories
- Check-in and check-out processsed track code versions and allow branching from the trun, facilitating sub-releases for release management
- Continous integration is a practice that encourages developers to check in their code daily to ensure that one change does not introduce a defect into the shared code base.


#### Lesson 2: Securing Public Repositories

##### Threats Facing Github

- `Hackers` can study application source code to learn how to attack the software when it's run in production
- Stealing source code gives hackers time to look for vulnerabilities that would be more difficult to detect through the penetration of production systems
- Attackers can even run code in production on their own systems and test attacks against it, refining their attacks to increase their speed, stealth and effectiveness.
- State-sponsored cybter terrorism and global corporate espionage are well funded effeorts that retain professionals to develop methods of intrusion and attack


#### Lesson 3: Secure Containerization
#### Lesson 4: Docker Trusted Repositories
#### Lesson 5: Docker Bench


## Chapter 3: Secure build Automation
---
#### Lesson 1: Automated Provisioning with PaaS Tooling

- `Provisioning` means providing something for use.
- In DevSecOps, the process of providing the developer with the software needed to develop an application is automated.
- It's important to ensure that the same software the developers use is provisioned in the test, staging and production environments.
- Complex system require automation to ensure that provisioned processes and components remain uniform and managed.

##### Platform as a Service (PaaS)

- The goal of Agile development is to give developers greater autonomy.
- `Shift left` is a process change in which operations and engineering teams shift processes upstream to developers
- Anything "as a service" allows consumers to request and receive deliverables on demand via a self-service portal
- `Platform as a Service` offerings provide developer platform and infrastructure via self-service tools and automation.

##### Templates

- Enterprise architects evaluate and test software components for production use.
- PaaS system facilitate the development of templates
- When developers require specific frameworks and infrastructures, they can select from available templates.
- `Infrastructure as Code` simplifies the configuration and deployment process and reduce maintenance.
- `Source to Image (S2I)` is a framework for building reproducible container images from application source code and dependent component libraries

#### Lesson 2: Securing the Automated Build

##### Jenkins

- `Jenkins` is a popular open-source build automation system.
- Jenkins originated from the open-source Hudson project.
- Jenkins is highly extensible and supports hundreds of plugins that handle build tasks.
- Jenkins supports Continous Integration by integrating with source code management tools
- Jenkins supports `webhoooks` that automatically trigger a build process when developers check in modified application souce code.


##### Typical CI/CD Pipeline Architecture (Using Jenkins)

Commit -> Build -> Test -> Stage -> Deploy

- A commit starts the process when the developer checks in code.
- The CI server launches a build process.
- Automated unit tests are typically part of the build process.
- Developers may review the build status via IDE or the Jenkins console.
- The build server delivers build artifacts to the appropriate target, typically a staging server.

##### Attach Surface of a Typical CI/CD Build Environment

- `Attack vectors` exist at the point of code check-in and anywhere an API session is establish to pull code.
- `Malware` and vulnerabilities exist in most component libraries pulled from third-party repos.
- Since it is necessary to use vulnerable code, vulnerability detection and monitoring must be done after the build process.
- The `OWASP Dependency Checker` is one open-source  tool that detects and reveals vulnerabilities and thier severity levels.

#### Lesson 3: Vulnerability Detection and Remediation

##### OWASP Dependency Checker

- `OWASP Dependency Check` is an open source scanning system.
- Dependency Check can be run from the commandline or through an automated build program such as Jenkins
- OWASP Dependency Check maintains a downloaded CVE database from the NIST NVD.
- To enforce security policy, automated builds can be configured to fail when policy thresholds are exceeded.

##### OWASP Top 10 - 2017

- `Prioritization` is vital to an effective security strategy
- it is impossible to eliminate vulnerabilities.
- The severity of a vulnerability indicates its potential threat
- The actual threat factors in its context
- The key facator for defining and automating a security policy is business impact
- Automation tolerates vulnerabilities but prevents critical vulnerabilities from promoting to upper environment until they have been `remediated`

1. Injection
2. Broken Authentication
3. Sensitive Data Exposure
4. XML External Entities (XXE)
5. Broken Access Control
6. Security Misconfiguration
7. Cross-Site Scripting (XSS)
8. Insecure Deserialization
9. Using Components with known Vulnerabilities
10. Insufficient Logging and Monitoring


##### DevSecOPs Detection and Remediation

1. Automate scanning throughout the pipeline.
2. Create well-defined security policy.
3. Implement rigorous gating.
4. Reduce the attack surface of DevOps infrastructures.
5. Continously monitor deployed applications
6. Establish traceability back to development
7. Practice ongoing hygiene
8. Utilize automated Configuration Management
9. Establish automated roll-back and recovery
10. Practice Continous Improvement with ongoing inspection and adaptaiong to accommodate security enhacements


## Chapter 4: Release Gating
---
#### Lesson 1: The 16 Gates

##### Release Gating

`Release gating` is a means of control that involves checks and balances in large enterprises
- Authorized stakeholders responsible for governance, risk management, audit and compliance review build artifacts prior to approving a software release for deployment
- While release gating may be doen at various stages of the pipeline it is most often performed in Pre-Production

##### The 16 Gates

1. Source code version control `Security Aspect`
2. Optimum branching strategy
3. Static analysis `Security Aspect`
4. At least 80% code coverage
5. Vulnerability scan `Security Aspect`
6. Open source scan `Security Aspect`
7. Artifact version control
8. Auto provisioning
9. Immutable servers `Security Aspect`
10. Integration testing
11. Performance testing
12. Build deploy testing automated for every commit `Security Aspect`
13. Automated rollback `Security Aspect`
14. Automated change order
15. Zero-downtime release
16. Feature toggle `Security Aspect`


## Chapter 5: Continuous Delivery Release Automation
---
#### Lesson 1: Automated Deployment
`Automated Deployment` refers to the practice and tooling used to cause build artifacts to be installed on upper level environments including production systems
- Automation is key to improve throughput but also as a means of controlling outcomes and ensuring consistency
- Once systems have cleared gating, triggers in the tooling call programs that ready the infrastructure and deploy, or install the application so that it may be immediately executed.

#### Lesson 2: Configuration Management

##### Configuration Management

- `Configuration Management` is the practice and tooling required to fully automate the process of configuring target environments and deploying application workloads to them.
- While Configuration Management is useful at all stages of provisioning, it is especially important when dealing with production platforms.
- Configuration Management is an automated approach that replaces traditional scripting and improves control and consistency when deployig applications
- Configuration management within this lesson is focused on the domain of `Automated Deployment`


## Chapter 6: Production Monitoring and Ongoing Detection and Remediation
---
#### Lesson 1: Production Monitoring

- `Continuous monitoring` is the ongoing automated evaluation of workloads to assess their health and function

- `Production monitoring` may include threat detection, vulnerability detection and malware prevention.

- Since new threats and vulnerabilities are developed and discovered over time, it is important to monitor applications throughout their entire lifespan, not just while they're under development.

- When applications need to be refactored or upgraded with patched third-party libraries, feedback loops and automated reporting notify the development team that vulnerabilities or other defects require attention.

##### Static Application Security Testing (SAST) - Ongoing SAST is necessary to keep applications free from vulnerabilies

-`Static scanning` is used to test for new vulnerabilities that have become known since an application's release into production.

- Scanning tools use container image manifests and build time dependency lists to determine which known vulnerabilties are present in compiled applications and container images.

- Due to ongoing changes to operating systems, binary libraries, and third-party software components, applications need to be continually upgraded. Scanning is fundamental to hygiene, and as are redeployed, automated pipelines ensure persistent security.

##### Dynamic Application Security Testing (DAST) - Ongoing DAST helps keep production applications free of vulnerabilities.

- `Dynamic scanning` uses black box security testing methodology.

- A black box test involves testing an application from the outside in while it is running in production.

- In DAST, a security analyst attempts to hack an application in the same way a cyber attacker would.

- Unlike SAST, DAST does not involve examining source code to identify vulnerabilities from the inside out.

##### Penetration Tests

- `Penetration tests (pentest)` are a form of DAST that use external programs to interrogate applications through their exposed API and HTTP endpoints.

- Penetration tests simulate automated cyber attacks on production infrastructure.

- Penetration tests detect common vulnerabilties such as injection, cross-site scripting and flaws in authentication and identity and access management (IAM).

#### Lesson 2: Dashboards and Automated Vulnerability Detection

- `Dashboards` provide continual updates on the health of applications running in prod.

- Dashboards aggregate and analyze logs, providing a stream of real-time data that is presented visually based on predefined configurations.

- Extensible dashboard software systems support plugins that can be configured according to user preferences.

- Dashboards can set alerts

##### Automated Vulnerability Detection

- `DAST` and `SAST` tools can provide logs for aggregation and monitoring in dashboards systems.

- Many dashboard visualization systems generate benchmark threshold of normal performance and report anomalies that exceed normal limits.

- Advanced machine learning and artificial intelligence algorithms can be used to score workloads over time and provide alerts when threat increase.

##### Traceability and Rollback

- `Traceability` refers to the process of communicating information about threat detection back to stakeholders, including the development team.

- when release management and source code control is establish to track major releases and sub-releases of applications, vulnerability reporting can be traced back to the specific source code being evaluated.

- Alerts and threat detection can be used to automatically `rollback` vulnerable systems when new releases appear flawed.

- Configuration management provides an automatics way to upgrade and downgrade applications when necessary.

## Chapter 7: Conclusion
---
- `DevSecOps` is relevant to all aspects of modern software development and deployment
- Choose your areas of specialty and consider deep dives or certifications in those technologies

#### Areas of Specialty
 
##### Development
 
 - Source Code Management 
 - Agile Lifecycle Tools
 - Test-Driven Development
 - PaaS Tooling
 - Security Standards
 - Containerizations

##### Release Management

- Build Pipeline Automation
- Continous Integration
- Automated Testing
- Pre-Production Staging
- Release Automation
- Configuration Management

##### Operations

- Server hardening
- Application hardening
- Continous Deployment
- Continous Monitoring
- Security Analytics
- Cloud Orchestration

##### Measuring DevSecOps Success

- Deployment frequency (fast and frequent releases)
- Lead time (code-to-cash cycle)
- Detection of threats, vulnerabilities and malware
- Mean time to repair and remediation
- Efficiency of rollback and recovery

