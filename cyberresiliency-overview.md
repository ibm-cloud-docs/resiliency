---

copyright:
  years: 2021, 2024
lastupdated: "2024-10-31"


keywords: cyber resiliency overview, resilient app, cyber recovery

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Understanding cyber resiliency
{: #cyber-resiliency-overview}

Cyber resilience is the capacity of an organization to prevent, withstand and recover from cybersecurity incidents, including ransomware attacks and other malicious actions. In an environment where threats and threat actors are continuously evolving, adopting a secure-by-design approach is essential to prepare for the inevitability of an attack. Business continuity planning in these scenarios should prioritize the safe and secure restoration of critical applications and infrastructure to maintain operational stability, which is the focus of cyber recovery. {{site.data.keyword.cloud_notm}} could host both your production and recovery environments or the recovery environment for an on-prem or remote production environment or be a secure vault to store your backup copies of golden images, configuration and data for recovery. 

## How is it different from Disaster Recovery?

While both cyber recovery and Disaster recovery deal with recovering services from backups, their goals differ significantly and this drives different requirements, processes, tools and techniques needed. It is good to consider cyber-resiliency as the third and separate pillar from backup and disaster recovery of your operational resiliency. Some of the significant differences are

<ul>
  <li>Nature - Unlike disasters which are typically random, natural events, cyber attacks are targetted, well-planned and engineered for maximum impact.</li>
  <li>RPO Considerations - While Disaster recovery aims at reducing RPO and returning services with minimum data loss, some effects of ransomware attacks could be latent and so the goal is to restore to a clean state even if it would mean a greater RPO and data loss.</li>
  <li>Backup Copies - Ransomware attacks are known to target backup copies, either destroying or corrupting them. Unlike disaster recovery, the latest backup copy could be inappropriate for cyber recovery. Multiple backups are maintained and a typical 3-2-1 rule of backups is extended to 3-2-1-1-0, i.e., 3 copies of data of which 2 are stored in separate media with 1 in a different location <em>and</em> 1 offline with 0 errors. The offline copy ensures a physical air-gap and having atleast 1 valid recovery point ensures that we can recover successfully. </li>
  <li>Isolation - A valid recovery point also dictates that we periodically restore and test backups for recoverability in an clean/isolated environment to ensure that any malicious code does not affect the rest of the environment.</li>
</ul>

For a more detailed treatment to how these two are complementary, please refer to a broader [https://www.ibm.com/think/topics/cyber-recovery-vs-disaster-recovery](article).
