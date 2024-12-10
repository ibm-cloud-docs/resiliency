---

copyright:
  years: 2024
lastupdated: "2024-12-10"


keywords: cyber resiliency overview, resilient app, cyber recovery

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Understanding cyber resiliency
{: #cyber-resiliency-overview}

Cyber resiliency is part of an organization's ability to prevent, withstand and recover from cybersecurity incidents, including ransomware attacks and other malicious actions.

Protect against such threats by using a 2-pronged approach: Cybersecurity focuses on preventing attacks in the first place, while cyber resiliency focuses on recovery and continuity after a breach. In an environment where threats and threat actors are continuously evolving, adopting a secure-by-design approach is essential to prepare for the inevitability of an attack. In business continuity planning, prioritize the safe and secure restoration of critical applications and infrastructure to maintain operational stability.

![Diagram depicting the scope of cybersecurity and cyber resiliency](images/cyber-resiliency-vs-security.svg "Diagram depicting the scope of cybersecurity and cyber resiliency"){: caption="Diagram depicting scope of cyber security vs cyber resiliency" caption-side="bottom"}

{{site.data.keyword.cloud_notm}} can host both your production and recovery environments. Host the recovery environment for an [on-premises](#x4561212){: term}  or remote production environment or use {{site.data.keyword.cloud_notm}} as a vault to securely store and verify your backup copies of golden images, configuration, and data for recovery.

## What's the difference between cyber resiliency and disaster recovery?
{: #cyber-dr-difference}

While both cyber resiliency and disaster recovery deal with recovering from backups, their goals differ significantly. Each needs different requirements, processes, tools, and techniques. It is beneficial to consider cyber resiliency as the third and distinct pillar from backup and disaster recovery of your operational resiliency. Review the following differences between cyber resiliency and disaster recovery:

Nature
:   Unlike disasters, which are typically random natural events, cyberattacks are targeted, well-planned, and engineered for maximum impact.

Recovery Point Objective considerations
:   Disaster recovery aims to reduce the [Recovery Point Objective](#x3429911){: term} (RPO) and return services with minimum data loss, but cyberattacks can have latent effects, which make the recovery point unclear. The goal of cyber resiliency is to restore to a clean state even if tha means a greater RPO and data loss.

Backup copies
:   Ransomware attacks are known to target backup copies, either destroying or corrupting them. Unlike disaster recovery, the latest backup copy might be inappropriate for cyber resiliency. Follow an enhanced version of the 3-2-1 backup rule, called 3-2-1-1-0. This means that you maintain three copies of data, with two stored on different types of media, one in a separate location, and another kept offline to provide a physical air-gap. At least one backup is verified to be error-free, helping ensure a reliable recovery point. The offline copy helps protect against cyberthreats, while the error-free backup helps ensure successful data recovery when needed.

Isolation
:   A valid recovery point means periodically restoring and testing backups for recoverability in a clean, isolated environment to help ensure that any malicious code doesn't affect the rest of the environment. You might need a separate isolated environment for forensic analysis to analyze the extent of the attack and the threat vector itself.

For more information, see [Cyber recovery vs. disaster recovery: What’s the difference?](https://www.ibm.com/think/topics/cyber-recovery-vs-disaster-recovery){: external}.
