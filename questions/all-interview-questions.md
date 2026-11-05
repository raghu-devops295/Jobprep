# Interview Question Banks – Raghwendra Mishra

Contents: 1. Allianz prep  2. Allianz JD questions (1–7 yrs)  3. AKS & Databricks (100)  4. Resume-based (120 + 20 scenarios)

---
# Allianz Global Investors – DevOps & Cloud Engineer (Investment Platform)
Interview prep for Raghwendra Mishra. Focus: Databricks and AKS.

---

## 0. Before tomorrow: things to know about this role

- **Location conflict:** The JD says "This position will be based in our Frankfurt office." You described it as remote. Confirm remote vs. onsite and any relocation expectations in the interview, and don't assume.
- **Stack match:**
  - Strong: Azure, AKS, Kubernetes, Databricks administration, CI/CD, platform reliability, monitoring, RBAC/security.
  - Partial: GitHub Actions (you use GitLab CI and Azure DevOps; the concepts transfer directly), Databricks Asset Bundles (learn the basics, see Q18–Q20), Helm (know the concepts, verify your hands-on depth).
  - Gaps to be honest about: production Python, TypeScript, SQL depth, JFrog, FastAPI/Angular deployment. Terraform is limited for you, so don't overclaim it.
- **Unity Catalog:** Your records say theory only. Say so plainly and show that you understand the model. Don't claim production UC administration.
- **Interviewer focus = Databricks and AKS.** Spend most of your prep time there.

---

## 1. "Tell me about yourself" (about 60–90 seconds)

> I'm a Senior Platform and DevOps Engineer with about seven years in the banking domain. At UBS I lead a team of five engineers, and we run five shared Azure Kubernetes Service and Azure Databricks platforms that support 18 banking applications.
>
> My work splits into three areas. First, platform operations: I handle Databricks workspace administration, including clusters, compute pools, and user and group access, and I run AKS workloads with Helm, monitoring, and incident ownership. Second, automation: I build reusable GitLab CI/CD pipeline templates, policy and RBAC automation, and alert integration with BigPanda and ServiceNow. Third, cost and migration: I moved VM-based workloads to containers, which saved about $22K a year, and I do regular cost estimation for the platforms.
>
> Security and reliability matter a lot in banking, so I work with Key Vault, private endpoints, network security groups, and RBAC, and I monitor with Prometheus, Grafana, Azure Monitor, and KQL.
>
> I'm now moving into more CI/CD-as-code and data-platform automation, which is why this role appeals to me: Databricks and AKS at the centre, with the investment-data context.

(Adjust the numbers only if they are wrong. Don't say "led" if your role was narrower.)

---

## 2. Key work to highlight (your strongest stories)

1. **Shared multi-tenant Databricks and AKS platforms for 18 banking apps.** Your main story. Explain isolation, access control, capacity, and how teams onboard.
2. **Databricks workspace administration.** Clusters, compute pools, user and group access, cost control via pool and cluster policy choices.
3. **Reusable GitLab CI/CD pipeline templates.** Maps to the JD's "CI/CD pipelines for automated testing, linting, and deployment." Mention SonarQube quality gates.
4. **Monitoring and incident ownership.** Prometheus, Grafana, Azure Monitor, KQL, BigPanda/ServiceNow. Maps to "BAU operations, platform monitoring, health checks."
5. **VM-to-container migration (~$22K annual savings).** Maps to AKS deployment and cost-awareness.
6. **Policy and RBAC automation, Key Vault and HashiCorp Vault, private endpoints.** Maps to security expectations in banking.

---

## 3. Unity Catalog: what it is and how it works

**What it is:** Databricks' unified governance layer for data and AI assets across workspaces. It's the central place to manage who can see and use tables, views, volumes, models, and files, and it records lineage and audit logs.

**The three-level namespace:** `catalog.schema.table`. A catalog holds schemas, a schema holds tables/views/volumes/models.

**How it works (core pieces):**
- **Metastore:** one per region, attached to one or more workspaces. It holds the metadata and the permission grants.
- **Catalogs and schemas:** organize data by domain or environment (for example `dev`, `prod`, or by business area).
- **Privileges:** `GRANT SELECT, MODIFY, USE CATALOG, USE SCHEMA, CREATE TABLE ...` to users, groups, or service principals. Groups are managed centrally (for example through SCIM or Entra ID sync).
- **Storage credentials and external locations:** how Unity Catalog reaches cloud storage (for example ADLS Gen2) without giving users direct storage keys.
- **Managed vs external tables:** managed tables are stored in a UC-controlled location; external tables point to a path you own.
- **Lineage and audit:** column- and table-level lineage, and audit logs of access.
- **Volumes:** governed storage for non-tabular files.
- **Delta Sharing:** share governed data with other organizations or platforms.

**Why it matters for this role:** It's the answer to "how do multiple workspaces share governed data safely," which is exactly the multi-tenant problem you solve on the platform side.

**Honest framing for the interview:** "I've studied and worked through the Unity Catalog model: metastore, catalogs, grants, storage credentials, and external locations. My hands-on experience is mostly workspace administration and access management; I haven't owned a full UC rollout end to end." (Use your own accurate wording.)

---

## 4. The 100 questions and answers

Answers are short and interview-ready. Where you lack hands-on experience, the answer says so and describes how you'd approach it.

### A. Databricks fundamentals (Q1–Q15)

**Q1. What is Azure Databricks?**
A managed Apache Spark-based analytics and AI platform on Azure, with notebooks, jobs, clusters, Delta Lake, and Unity Catalog for governance.

**Q2. What is the difference between the control plane and the data plane?**
The control plane (Databricks-managed) handles the UI, notebooks metadata, and job scheduling. The data plane runs in your Azure subscription: clusters, storage, and networking. In a VNet-injected workspace, the clusters run in your VNet.

**Q3. What is a cluster in Databricks?**
A set of VMs running Spark. Types: all-purpose clusters (interactive) and job clusters (created per job, terminated after). Job clusters are cheaper for scheduled work.

**Q4. What are compute pools?**
Pre-warmed pools of idle VM instances that clusters can draw from, so start-up time drops. You pay for idle instances, so size and idle timeout matter for cost.

**Q5. How do you control cost on Databricks?**
Auto-termination, job clusters over all-purpose clusters, cluster policies that cap node types and sizes, spot instances where tolerable, pools with sensible idle limits, and monitoring DBU spend per workspace or team.

**Q6. What are cluster policies?**
Rules that restrict what users can create: allowed node types, max workers, mandatory tags, auto-termination. They enforce standards and cost control.

**Q7. How do you manage user and group access in a Databricks workspace?**
Groups synced from Entra ID (SCIM), permissions granted to groups not individuals, and workspace-level entitlements (for example "allow cluster creation") separated from data permissions.

**Q8. What is Delta Lake?**
An open table format on Parquet with a transaction log. It provides ACID transactions, schema enforcement, time travel, and efficient upserts (MERGE).

**Q9. What is time travel in Delta?**
Querying earlier versions of a table by version or timestamp, using the transaction log. Retention is governed by table properties and VACUUM.

**Q10. What does VACUUM do?**
Removes data files no longer referenced by the transaction log and older than the retention threshold. Lowering retention below the default has consequences for time travel and concurrent readers.

**Q11. What is OPTIMIZE and Z-ORDER?**
OPTIMIZE compacts small files. Z-ORDER co-locates related data to speed up selective queries on the chosen columns.

**Q12. What are Databricks Jobs and how do you run them?**
Scheduled or triggered workflows of notebooks, Python scripts, JARs, or SQL tasks, with dependencies, retries, and alerts. They can run on job clusters or serverless compute, depending on availability.

**Q13. How do you deploy notebooks and jobs across environments?**
Source-controlled code in Git, promoted through pipelines, with environment-specific configuration. For jobs, use bundles (see Q18–Q20) or API/Terraform-based definitions.

**Q14. How do you secure secrets used in Databricks notebooks?**
Use secret scopes backed by Azure Key Vault. Never hard-code secrets in notebooks. Reference them with `dbutils.secrets.get`.

**Q15. How do you connect Databricks to ADLS Gen2 securely?**
Prefer Unity Catalog external locations and storage credentials (managed identity or access connector) over account keys. For legacy setups, use service principals with the least-privilege role on the storage container.

### B. Databricks networking, security and operations (Q16–Q30)

**Q16. What is VNet injection?**
Deploying the Databricks data plane into your own VNet, with public and private subnets. It gives you control of NSGs, routing, and private connectivity.

**Q17. Why use Secure Cluster Connectivity (no public IP)?**
Clusters have no public IPs and connect outbound only to the control plane via relay, reducing exposure. It's a common banking requirement.

**Q18. What are Databricks Asset Bundles (DABs)?**
A way to define jobs, pipelines, and related resources as YAML code, version them in Git, and deploy them to targets (dev, test, prod) with the Databricks CLI. The JD names this specifically.

**Q19. How would you set up a CI/CD pipeline for a bundle?**
Validate the bundle (`databricks bundle validate`), run unit tests and lint, deploy to a dev target from feature branches, promote to prod via approval, and use per-environment variables and service principals for the deploy identity.

**Q20. What's the difference between a bundle target and a workspace?**
A target is a deployment configuration (workspace host, variables, mode such as development or production). The same bundle can deploy to several targets.

**Q21. How do you monitor Databricks jobs?**
Job run history, alerts on failure or duration, cluster and driver logs, and forwarding diagnostic logs to Azure Monitor or Log Analytics for KQL queries and dashboards.

**Q22. How do you troubleshoot a slow Spark job?**
Check the Spark UI for skew, spill, shuffle size, and idle stages. Then look at file sizes (small-file problem), partitioning, broadcast joins, and whether the cluster is undersized or oversized.

**Q23. What is data skew and how do you fix it?**
Uneven key distribution means a few tasks do most of the work. Fixes: salting keys, adaptive query execution (AQE), repartitioning on better keys, or broadcasting the small side of the join.

**Q24. What is Photon?**
A vectorized query engine in Databricks, faster for SQL and DataFrame workloads. It's enabled per cluster and can affect cost.

**Q25. What is autoscaling and when is it a problem?**
Clusters add and remove workers based on load. It's helpful for variable workloads, but it can hurt predictable cost and cause start delays if pools are not used.

**Q26. How do you handle a Databricks workspace outage or a failed cluster start?**
Check the cluster event log, quota limits in the subscription, VNet and NSG rules, and DNS. Then check whether a recent policy or network change caused it. Communicate status through the incident process.

**Q27. What are the quotas you watch for Databricks on Azure?**
vCPU quotas per region and VM family, public IP limits in the VNet design, and workspace-level limits on concurrent jobs and clusters.

**Q28. How do you upgrade or migrate Databricks runtimes?**
Test the job in a dev workspace on the new runtime, check library and Spark-version changes, plan a staged rollout by team, and keep a rollback path.

**Q29. How do you handle library management?**
Prefer cluster-scoped or workspace-scoped libraries from a controlled artifact feed. Pin versions, and avoid installing unvetted packages on shared clusters.

**Q30. How do you isolate teams on a shared Databricks platform?**
Separate workspaces or catalogs per team or environment, group-based permissions, cluster policies per team, cost tags, and quotas. You can describe the isolation model you run on the 18-application platform.

### C. Databricks + data and AI platform topics (Q31–Q40)

**Q31. What's the difference between a warehouse and a Lakehouse?**
A warehouse stores curated, structured data for BI. A lakehouse combines low-cost open storage with warehouse features like ACID tables and governance, on one platform.

**Q32. What is Auto Loader?**
An incremental file ingestion source that detects new files in cloud storage efficiently, with schema inference and evolution.

**Q33. What is Delta Live Tables (Lakeflow Declarative Pipelines)?**
A declarative way to define data pipelines with expectations for data quality, managed orchestration, and recovery. Naming has evolved, so mention both if the interviewer uses the newer name.

**Q34. What are expectations in pipelines?**
Data quality rules applied to rows: warn, drop, or fail the pipeline when a rule breaks.

**Q35. What's the difference between MERGE and INSERT OVERWRITE?**
MERGE applies upserts and deletes based on a match condition. INSERT OVERWRITE replaces the target partition or table contents.

**Q36. What is a Databricks SQL warehouse?**
Compute optimized for SQL and BI workloads. Serverless or classic options exist. Size and auto-stop control cost.

**Q37. How do you give BI users access to data?**
Grant SELECT on governed catalogs and schemas to groups, expose them through SQL warehouses, and keep access reviews regular.

**Q38. What is MLflow in Databricks?**
Tracking, model registry, and deployment for ML experiments and models. Relevant to the JD's AI-solutions context.

**Q39. What are model serving endpoints?**
Managed endpoints that serve registered models over REST. They need cost and scaling planning, like any online service.

**Q40. How would you deploy an API or web app next to Databricks workloads?**
Containerize the app, deploy to AKS behind an ingress, use managed identity to reach data, and keep configuration in Key Vault and Helm values. This connects the AKS and Databricks parts of the role.

### D. Unity Catalog (Q41–Q55)

**Q41. What is Unity Catalog?**
Databricks' central governance layer for data and AI assets across workspaces: access control, auditing, lineage, and discovery.

**Q42. What is the three-level namespace?**
`catalog.schema.object`, for example `prod_investments.research.daily_prices`.

**Q43. What is a metastore?**
The top-level container that holds UC metadata and permissions for attached workspaces. Typically one per region.

**Q44. What is the difference between a managed and an external table in UC?**
A managed table's data lives in a UC-controlled location and is dropped with the table. An external table points to a path you own; dropping it removes metadata only.

**Q45. What are storage credentials and external locations?**
A storage credential is the identity used to access cloud storage. An external location maps a storage path to that credential so UC can govern it.

**Q46. How do you grant access in UC?**
With `GRANT` statements or the UI/API, on catalogs, schemas, and tables, to groups. Users also need `USE CATALOG` and `USE SCHEMA` to see the objects.

**Q47. How does lineage work in Unity Catalog?**
UC captures table and column lineage from queries and pipelines, visible in the catalog explorer. It helps impact analysis and audit.

**Q48. How do you share data across organizations with UC?**
Delta Sharing: a provider creates a share, a recipient consumes it, with governance kept on the provider side.

**Q49. What are Volumes in UC?**
Governed storage for files (CSV, images, models, unstructured data) with the same permission model as tables.

**Q50. How would you migrate from the legacy Hive metastore to Unity Catalog?**
Inventory tables and jobs, create catalogs and external locations, use the upgrade tooling or CTAS/clone, update code to three-level names, test in dev, and cut over per team with a rollback plan.

**Q51. What is a service principal and why use it for jobs?**
A non-human identity for automation. Jobs running as a service principal avoid dependence on a person's account and make permissions auditable.

**Q52. How does UC help with compliance in banking?**
Central access control, audit logs of data access, lineage for data origin, and separation of duties through group-based grants.

**Q53. What's the difference between workspace-level and account-level UC configuration?**
Account-level handles metastores, identity sync, and cross-workspace governance. Workspace-level covers compute and workspace-specific settings. Be accurate about which level you administer.

**Q54. How do you handle row- and column-level security in UC?**
Row filters and column masks defined as functions and applied to tables, so the same table returns different data depending on the user's group.

**Q55. Be honest: how much UC have you run in production?**
Strong answer: "I've worked through the model and administered access on the workspaces I own. I haven't led an end-to-end UC migration yet, but here is how I'd run one: inventory, catalog design, storage credentials, grants via code, and staged cutover." (Adjust to your actual experience.)

### E. AKS and Kubernetes (Q56–Q75)

**Q56. What is AKS?**
Azure's managed Kubernetes service. Azure manages the control plane. You manage node pools, networking choices, add-ons, and workloads.

**Q57. What are node pools?**
Groups of VMs with the same configuration within a cluster. Use system and user pools, and separate pools for different workloads or VM families.

**Q58. What networking models does AKS support?**
Kubenet and Azure CNI (and Azure CNI Overlay). Azure CNI gives pods VNet IPs and direct connectivity, but needs more IP planning.

**Q59. How do you plan IP addresses for AKS with Azure CNI?**
Count nodes times max pods per node plus node IPs and reserved addresses. Size subnets for growth, and use separate subnets per node pool when needed.

**Q60. What is a Kubernetes Deployment, StatefulSet, and DaemonSet?**
Deployment: stateless replicas with rolling updates. StatefulSet: stable identity and storage for stateful apps. DaemonSet: one pod per node, for agents like log collectors.

**Q61. What is a Service and an Ingress?**
A Service gives stable networking to a set of pods (ClusterIP, NodePort, LoadBalancer). Ingress routes external HTTP(S) traffic to services by host and path, through an ingress controller.

**Q62. What is Helm?**
A package manager for Kubernetes. Charts template manifests, values files set per environment, and releases are versioned and rollback-capable.

**Q63. How do you manage Helm values across environments?**
Base values plus environment overrides, stored in Git, and deployed through the pipeline. Secrets come from Key Vault via CSI driver or external secrets, not plain values files.

**Q64. What are resource requests and limits?**
Requests drive scheduling. Limits cap usage. Set both for predictable scheduling and to avoid noisy-neighbour problems on shared clusters.

**Q65. What are liveness and readiness probes?**
Liveness restarts unhealthy containers. Readiness controls whether a pod gets traffic. Wrong probes can cause restart loops or outages.

**Q66. How do you do zero-downtime deployments?**
Rolling updates with readiness probes, PodDisruptionBudgets, multiple replicas, and graceful shutdown handling.

**Q67. What is a PodDisruptionBudget?**
A limit on how many pods of an app can be voluntarily disrupted at once, protecting availability during node upgrades or drains.

**Q68. How do you handle AKS cluster upgrades?**
Check version support, review deprecated APIs, upgrade the control plane, then node pools one at a time with surge settings, and watch PDBs and workloads.

**Q69. How do you secure an AKS cluster?**
Azure AD (Entra ID) integration with RBAC, private clusters or restricted API access, network policies, Azure Policy add-on, pod security standards, image scanning, and secrets from Key Vault.

**Q70. What is the Key Vault CSI driver?**
A driver that mounts Key Vault secrets into pods, using a workload identity or managed identity, so secrets don't live in manifests.

**Q71. What is workload identity in AKS?**
Pods get Entra ID tokens through federated credentials, so they can access Azure resources without storing secrets.

**Q72. How do you monitor AKS?**
Container insights and Azure Monitor, Prometheus and Grafana for cluster and app metrics, and logs in Log Analytics queried with KQL.

**Q73. How do you troubleshoot a pod stuck in CrashLoopBackOff?**
`kubectl describe pod`, `kubectl logs --previous`, check probes, resource limits (OOMKilled), config and secret mounts, then the image and entrypoint.

**Q74. How do you troubleshoot a pod stuck in Pending?**
Check events for insufficient CPU or memory, node selectors or taints, PVC binding, and quota limits.

**Q75. How do you handle autoscaling in AKS?**
Cluster autoscaler for nodes, and Horizontal Pod Autoscaler for pods based on CPU or custom metrics. KEDA for event-driven scaling.

### F. Azure networking, security and identity (Q76–Q85)

**Q76. What is a VNet, subnet, NSG?**
VNet is the private network. Subnets divide it. NSGs filter traffic at subnet or NIC level with allow and deny rules.

**Q77. What are private endpoints and why do banks use them?**
A private IP in your VNet for an Azure PaaS service, so traffic stays on the private network. Usually paired with private DNS zones and public access disabled.

**Q78. How does private DNS work with private endpoints?**
A private DNS zone linked to the VNet resolves the service name to the private IP. Missing or misconfigured links are a common cause of connectivity issues.

**Q79. What is Azure Firewall vs. NSG?**
NSGs are layer-4 filters per subnet or NIC. Azure Firewall is a managed, centralized, stateful firewall with FQDN and threat-intel filtering. Both are often used together.

**Q80. What is RBAC in Azure and how do you apply least privilege?**
Role assignments (role, principal, scope). Grant the smallest role at the narrowest scope, use groups, and review assignments regularly.

**Q81. What is Azure Policy?**
Rules that enforce or audit configuration, for example allowed regions, required tags, or private endpoints only. You used policy-based automation in your work; describe that.

**Q82. What is managed identity?**
An Entra ID identity managed by Azure for a resource, so you don't store credentials. System-assigned or user-assigned.

**Q83. How do you rotate secrets?**
Key Vault with rotation policies or automated rotation, consumers reading the latest version, and monitoring for expired secrets.

**Q84. What is the difference between Key Vault and HashiCorp Vault?**
Key Vault is Azure-native and integrates with Entra ID. HashiCorp Vault is multi-cloud and has dynamic secrets. You have used both; describe the use case you know best.

**Q85. How do you handle a network connectivity issue between AKS and a PaaS service?**
Check DNS resolution from the pod, private endpoint and DNS zone links, NSG and firewall rules, route tables, and the service's firewall or public-access settings.

### G. CI/CD, IaC and DevOps practice (Q86–Q92)

**Q86. How would you build CI/CD for AKS deployments?**
Build and scan the image, push to ACR, lint and validate Helm charts, deploy to dev, run smoke tests, promote with approval to higher environments, and keep environment values in Git.

**Q87. GitHub Actions vs. GitLab CI: how do you transfer your experience?**
The concepts are the same: pipelines, jobs, runners or agents, environments, secrets, and approvals. Your reusable GitLab templates map to GitHub reusable workflows and composite actions.

**Q88. What is infrastructure as code and where have you used it?**
Declarative definitions of infrastructure in version control. You use ARM and have limited Terraform experience; say that honestly and mention what you have built or are learning.

**Q89. How do you do a safe Terraform change?**
Plan and review, run in a pipeline with state locking, apply with approval, and avoid manual drift.

**Q90. How do you handle pipeline secrets?**
Use the CI platform's secret store or federated identity to Azure, never plain variables in code. Rotate and scope them.

**Q91. How do you ensure code quality in pipelines?**
Linting, unit tests, SonarQube quality gates, dependency and image scanning, and blocking merges on failures.

**Q92. What is GitOps and would you use it here?**
Git as the source of truth for cluster state, with a controller like Argo CD or Flux syncing it. Useful for AKS; say what you know and what you'd evaluate.

### H. Behavioural and experience-linking (Q93–Q100)

**Q93. Tell me about a production incident you owned.**
Use STAR: situation, your action, the fix, and the follow-up (runbook, alert, or automation). Pick a real one.

**Q94. Tell me about a cost reduction you delivered.**
The VM-to-container migration (~$22K annual savings). Explain the baseline, the approach, the risk control, and the result.

**Q95. How did you lead your team of five?**
Give a concrete example: how you split work, reviewed pipelines, or onboarded a new engineer.

**Q96. How do you handle a request from an application team that conflicts with platform standards?**
Explain the requirement, offer a compliant option, document the exception with a risk owner, and set a review date.

**Q97. Why are you interested in this role?**
Databricks and AKS at the centre of an investment platform, with investment-research and AI use cases. Tie it to your platform background and your interest in the data side.

**Q98. Where are your biggest gaps for this role?**
Be honest: production Python and TypeScript depth, and hands-on Unity Catalog ownership. Then say what you are doing about each.

**Q99. Where do you see yourself in two years?**
Owning a data and AI platform area end to end, with stronger software-engineering depth.

**Q100. Do you have questions for us?**
Ask: How is the platform team split between Databricks and AKS? What does on-call look like? How is Unity Catalog deployed? What is the expected onsite requirement?

---

### I. Gap-filling questions from the JD (Q101–Q115)

**Q101. The JD asks for Azure SQL and Data Lake. Where have you used them?**
Be specific about what you did with them: access control, networking (private endpoints), backups or retention, and how apps connect via managed identity. If your hands-on use was limited, say so and describe the security and networking setup you know.

**Q102. How would you secure access from Databricks to Azure SQL?**
Private endpoint for SQL, managed identity or service principal with a least-privilege database role, no SQL passwords in notebooks, and secrets (if any) from Key Vault.

**Q103. What is the difference between ADLS Gen2 hierarchical namespace and blob storage?**
ADLS Gen2 adds a true directory hierarchy and POSIX-style ACLs on top of blob storage, which matters for analytics performance and permissions.

**Q104. What is JFrog Artifactory and why use it?**
A universal artifact repository. It stores Python packages, Docker images, and Helm charts, with access control and scanning, and it gives a controlled source for dependencies. The JD asks for familiarity; be honest about your depth.

**Q105. How do you use a private artifact feed for Python packages in a pipeline?**
Point pip at the internal index with a service credential from the pipeline's secret store, pin versions, and scan packages before use.

**Q106. How would you deploy a FastAPI service on AKS?**
Containerize with a slim base image, run with a production ASGI server (for example Uvicorn with Gunicorn workers), expose via a Service and Ingress, set probes on a health endpoint, and manage config and secrets through Helm values and Key Vault.

**Q107. How would you deploy an Angular front end?**
Build static assets in CI, serve them from an NGINX container or a storage static site behind the ingress or CDN, and set cache headers and environment configuration at deploy time.

**Q108. What SQL would you expect a DevOps engineer to write or read?**
Enough to check data, troubleshoot pipelines, and verify deployments: SELECT with joins and aggregates, row counts before and after a migration, and basic MERGE logic. Be honest that you are not a SQL developer if that's the case.

**Q109. What TypeScript would you use in this role?**
Reading and changing small services, typed API clients, and pipeline or tooling scripts. Say what you have actually done, not more.

**Q110. How do you run BAU operations and health checks on a shared platform?**
Scheduled health checks on clusters, AKS nodes, and key endpoints; dashboards and alerts; a daily or weekly review; and a runbook for common incidents. Mention the monitoring stack you use (Prometheus, Grafana, Azure Monitor, KQL).

**Q111. How do you work with investment engineering teams that need new pipelines?**
Gather requirements, agree on a standard pipeline template, onboard them with examples, then collect feedback and fix gaps in the template. This maps directly to the JD's "engage regularly with investment engineering teams."

**Q112. How much do you know about investment management data?**
Be honest. Explain what you know (for example market data, research datasets, and why lineage and access control matter for them) and say you are keen to learn the financial domain.

**Q113. How would you support AI and research solutions on the platform?**
Give the research and AI teams a governed workspace with Unity Catalog grants, GPU or sized compute policies, MLflow tracking, and a serving endpoint with monitoring and cost limits.

**Q114. Whiteboard: design a Databricks and AKS platform for a new investment team.**
Walk through: a dedicated workspace or catalog per team in one environment, Unity Catalog metastore and storage credentials, Entra ID groups, cluster policies, a VNet-injected workspace with private endpoints, an AKS cluster for APIs and web apps with ingress and workload identity, CI/CD with bundles and Helm, and cost tags and monitoring.

**Q115. How do you handle Agile delivery in a DevOps role?**
Work in sprints with the product team, keep platform work on the same board (Jira), use definition of done that includes pipelines and monitoring, and make platform tickets visible to the business.

---

## 5. Last-minute checklist

- Re-read the Databricks, Unity Catalog, and AKS sections, and be ready to explain them in your own words.
- Prepare two STAR stories from the 18-application platform.
- Have your three-line "tell me about yourself" memorised, not word-for-word.
- Confirm remote vs. Frankfurt before the call.
- Have one clear, honest sentence ready for each gap.

---

## 6. How a VM-to-AKS container migration works (template)

**Important:** This is a general template for how such migrations are run. Replace each step with what you actually did on the platform. Don't quote the template's numbers or steps as your own without checking.

**Step 1: Discovery and inventory**
List every VM, its application, owner, dependencies (databases, queues, file shares, other services), OS and runtime, and its usage pattern. Identify which apps are containerizable and which should stay on VMs.

**Step 2: Assessment and prioritization**
Score apps by effort, risk, and savings. Start with stateless or low-risk apps to build the pattern, then move the harder ones.

**Step 3: Baseline the cost**
Record current VM spend (compute, disks, licences, backup, support time). Model the target AKS cost (node pools, right-sizing, reserved instances, shared capacity). The saving is the difference, minus new platform costs.

**Step 4: Containerize**
Write a Dockerfile per app, move configuration out of the image into environment variables or config maps, move secrets into Key Vault, and make logs go to stdout for collection.

**Step 5: Package for AKS**
Write Helm charts with per-environment values, define requests and limits, probes, PodDisruptionBudgets, and ingress rules. Use workload identity for Azure access.

**Step 6: Networking and security**
Place AKS in the right subnet, set NSG rules, configure private DNS for dependencies, and align ingress with the bank's existing traffic and certificate patterns.

**Step 7: Data and state**
Stateless apps need nothing extra. For stateful parts, decide between persistent volumes, moving data to a PaaS service, or keeping that component on a VM.

**Step 8: CI/CD and environments**
Build a pipeline template: build, scan, push to ACR, deploy to dev, test, promote. Reuse the template for each app.

**Step 9: Testing and cutover**
Run in parallel with the VM where possible, compare outputs, load test, then cut over with a DNS or ingress switch. Keep the VM running for a rollback window.

**Step 10: Monitoring and handover**
Set dashboards and alerts before cutover, write runbooks, and train the application team.

**Step 11: Decommission and savings tracking**
Stop and delete VMs only after the rollback window, remove orphaned disks and public IPs, and track actual savings against the baseline each month.

### Your version (fill these in before the interview)

- Number of VMs and apps moved: [your number]
- Which apps first, and why: [your answer]
- Cutover approach (parallel run, DNS switch, blue/green): [your answer]
- How the $22K saving was calculated and over what period: [your answer]
- Biggest problem you hit and how you fixed it: [your answer]
- What you would change next time: [your answer]

Tell me these details and I'll rewrite Section 6 in your own words, with the real numbers and the STAR story for it.

---

## 7. Last-minute checklist (final)

- Re-read the Databricks, Unity Catalog, and AKS sections, and be ready to explain them in your own words.
- Prepare two STAR stories from the 18-application platform, one of them the VM-to-AKS migration.
- Have your three-line "tell me about yourself" memorised, not word-for-word.
- Confirm remote vs. Frankfurt before the call.
- Have one clear, honest sentence ready for each gap.

- Re-read the Databricks, Unity Catalog, and AKS sections, and be ready to explain them in your own words.
- Prepare two STAR stories from the 18-application platform.
- Have your three-line "tell me about yourself" memorised, not word-for-word.
- Confirm remote vs. Frankfurt before the call.
- Have one clear, honest sentence ready for each gap.

---

# Allianz GI – Interview Question Bank (1 to 7 years)
Covers: your resume, the JD (Databricks, AKS, Python/SQL, CI/CD, IaC, JFrog, Helm, Azure, Kubernetes, BAU, Unity Catalog).

How to read each item:
- **Q** = the question
- **Answer** = what a strong candidate says
- **Simple** = the same idea, explained like you're explaining it to a child

---

## LEVEL 1: About 1 year experience (basics)

**Q1. What is Kubernetes?**
Answer: A system that runs and manages containers across many machines, restarting them if they fail and scaling them when needed.
Simple: It's like a school principal who makes sure every classroom (container) has a teacher, and sends a new teacher when one is sick.

**Q2. What is a container and how is it different from a VM?**
Answer: A container shares the host OS kernel and packages only the app and its libraries. A VM runs a full operating system on top of virtual hardware, so it's heavier.
Simple: A VM is a whole house. A container is a room inside a house that shares the same plumbing.

**Q3. What is Git branching and why use it?**
Answer: Branches let people work on changes separately, then merge them back after review, so the main code stays stable.
Simple: Like writing your homework in a draft notebook first, then copying the final version into the main book once the teacher checks it.

**Q4. What is CI/CD?**
Answer: CI (continuous integration) builds and tests code on every change. CD (continuous delivery or deployment) releases that tested code to environments automatically or on approval.
Simple: Every time you finish a drawing, a friend checks it (CI). Then it goes on the wall for everyone to see (CD).

**Q5. What is Azure Resource Group?**
Answer: A logical container that groups related Azure resources so you can manage, secure, and track them together.
Simple: A folder on your computer. You put all the files for one project in one folder.

**Q6. What is a Key Vault used for?**
Answer: Storing secrets, keys, and certificates securely, with access control and audit logs, so passwords aren't written in code.
Simple: A locked box. Only people with the key can open it, and the bank writes down every time someone opens it.

**Q7. What is a SQL query used for?**
Answer: SELECT retrieves data, WHERE filters rows, JOIN combines tables, GROUP BY summarises.
Simple: Asking a question to a big list: "show me only the red ones (WHERE), and count them per box (GROUP BY)."

**Q8. What does a Terraform state file do?**
Answer: It records what Terraform created, so the next plan knows what already exists and what to change.
Simple: A notebook of what you built, so next time you know what's already there.

---

## LEVEL 2: About 2 to 3 years experience

**Q9. What is a Kubernetes Deployment and why use one?**
Answer: It declares how many replicas of a pod should run and manages rolling updates and rollbacks.
Simple: A rule book that says "always keep 3 cookies on the plate," and if one breaks, make a new one.

**Q10. What's the difference between a Service and an Ingress?**
Answer: A Service gives pods a stable internal address. An Ingress routes outside HTTP traffic to services by hostname or path.
Simple: The Service is a classroom number inside the school. The Ingress is the school gate that directs visitors to the right classroom.

**Q11. What are liveness and readiness probes?**
Answer: Liveness checks if the app is alive and restarts it if not. Readiness checks if it can take traffic and removes it from load balancing if not.
Simple: Liveness asks "are you breathing?" Readiness asks "are you ready to play?"

**Q12. What is Helm?**
Answer: A package manager for Kubernetes. It templates manifests and lets you set values per environment.
Simple: A cake recipe with blanks. You fill in the flavour (values) for each birthday (environment).

**Q13. What is a Databricks job?**
Answer: A scheduled or triggered run of notebooks, scripts, or SQL tasks, with dependencies, retries, and alerts.
Simple: An alarm clock that starts a set of chores in order and tells you if one fails.

**Q14. What is a Databricks cluster vs a job cluster?**
Answer: An all-purpose cluster is for interactive work and stays up. A job cluster is created for one job and deleted afterwards, which is cheaper.
Simple: All-purpose is a shared kitchen that stays open. A job cluster is a food truck that comes for one order and leaves.

**Q15. What is Azure Monitor and Log Analytics?**
Answer: Azure Monitor collects metrics and logs from resources. Log Analytics stores logs, and you query them with KQL.
Simple: Azure Monitor is the CCTV. Log Analytics is the recording room, and KQL is how you search the recordings.

**Q16. What is KQL?**
Answer: Kusto Query Language, used to search and summarise logs in Log Analytics, for example `ContainerLog | where LogEntry contains "error" | count`.
Simple: A search button for a giant diary.

**Q17. What is a network security group (NSG)?**
Answer: A set of allow and deny rules that filter network traffic to and from subnets or network interfaces.
Simple: A guard at the gate with a list of who can enter and who can't.

**Q18. What is RBAC in Azure?**
Answer: Role-Based Access Control: you give a role (like Reader or Contributor) to a user or group at a scope (resource, resource group, or subscription).
Simple: Giving a child a key to only their own room, not the whole house.

---

## LEVEL 3: About 4 to 5 years experience

**Q19. What is Databricks Asset Bundles?**
Answer: A way to define jobs, pipelines, and resources as YAML, version them in Git, and deploy to dev, test, and prod targets with the Databricks CLI.
Simple: A recipe card for a whole meal. You can cook it in your kitchen or the restaurant's kitchen with the same card.

**Q20. How do you control Databricks cost?**
Answer: Job clusters over all-purpose, auto-termination, cluster policies that cap size and node type, instance pools with idle limits, and cost tags per team.
Simple: Don't leave the lights on, use the small car for small trips, and put a label on each family's bill.

**Q21. What is an instance pool and why use it?**
Answer: A set of ready VMs kept warm so clusters start faster. You pay for idle VMs, so set sensible idle limits.
Simple: A waiting room with chairs already taken, so the next customer sits down immediately.

**Q22. How do you isolate teams on a shared Databricks platform?**
Answer: Separate pools per team, AD groups for access, cluster policies, job tags to route jobs to the right pool, and cost tags.
Simple: Each family gets its own table in the canteen, and only they can sit there.

**Q23. What is Delta Lake?**
Answer: An open table format on Parquet with a transaction log. It gives ACID transactions, schema enforcement, time travel, and MERGE.
Simple: A notebook where every change is written in ink with a date, so you can see old pages and fix mistakes.

**Q24. What is time travel in Delta?**
Answer: Querying an older version of a table by version number or timestamp, based on the transaction log.
Simple: Rewinding a video to see the scene again.

**Q25. What is a private AKS cluster?**
Answer: The API server only has a private IP inside the VNet, so it can't be reached from the internet.
Simple: A school office that only takes calls from inside the school.

**Q26. How do private endpoints and private DNS work together?**
Answer: A private endpoint gives a PaaS service a private IP in your VNet. A private DNS zone linked to the VNet makes the service name resolve to that private IP.
Simple: A private door plus a directory that tells everyone "use the private door."

**Q27. How do you upgrade AKS without downtime?**
Answer: Upgrade the control plane, then node pools one at a time with surge capacity. Cordon and drain nodes, and respect PodDisruptionBudgets.
Simple: Change one bus at a time, and move the passengers to the next bus before you take the old one off the road.

**Q28. What is a PodDisruptionBudget?**
Answer: It sets the minimum number of pods that must stay running during voluntary disruptions like node drains.
Simple: A rule that says "at least 2 of the 3 players must stay on the field during a break."

**Q29. How do you rotate a secret used by a Databricks job without downtime?**
Answer: Create a new secret while the old one still works, store it as a new Key Vault version, let new runs read the latest value, then disable the old secret after running jobs finish.
Simple: Hand over a new key to a new visitor before taking the old key back from the old visitor.

**Q30. What is a service principal?**
Answer: A non-human identity in Entra ID used by apps and pipelines to access Azure resources.
Simple: A robot badge for a machine, instead of a person's badge.

---

## LEVEL 4: About 6 years experience

**Q31. How do you design a multi-tenant Databricks platform?**
Answer: Isolate by workspace or catalog, use AD groups and service principals, cluster policies per team, instance pools per team, and Unity Catalog for data governance.
Simple: A block of flats where each family has its own flat, its own key, and a shared lift that everyone can use safely.

**Q32. What is Unity Catalog and why move to it?**
Answer: Central governance for data across workspaces: catalogs, schemas, and tables with grants, plus lineage and audit logs. It replaces per-workspace permissions.
Simple: One library card for the whole city instead of a separate card for every branch.

**Q33. How do you handle data access in Unity Catalog?**
Answer: Grant privileges to groups on catalogs, schemas, and tables, use row filters and column masks for sensitive data, and audit access.
Simple: Some books are for everyone, some are for adults only, and the librarian hides certain pages.

**Q34. How would you migrate a Databricks workspace to Unity Catalog?**
Answer: Inventory tables and jobs, design catalogs, set up storage credentials and external locations, migrate tables, update code to three-level names, test in dev, then cut over team by team with a rollback plan.
Simple: Move house room by room. Label every box, check the new room works, then move the next room.

**Q35. How do you secure AKS workloads?**
Answer: Entra ID integration with RBAC, private cluster, network policies, Azure Policy, pod security standards, image scanning, and workload identity for secrets.
Simple: A building with a guard, locked floors, a visitor list, and no key lying around.

**Q36. How do you troubleshoot a pod that won't start?**
Answer: Check events with `kubectl describe pod`, read logs with `kubectl logs --previous`, check probes, OOMKilled limits, missing secrets or config, and the image.
Simple: Like a doctor: check the chart, ask what hurts, and check the medicine cupboard.

**Q37. What is the difference between CI in GitHub Actions and GitLab CI?**
Answer: Same ideas: workflows or pipelines, jobs, runners or agents, environments, secrets, and approvals. GitHub uses workflows and reusable workflows; GitLab uses YAML pipelines with includes.
Simple: Two schools with different timetables that teach the same subjects.

**Q38. How do you deploy one pipeline template to many teams?**
Answer: Version the template with tags, consumers pin a version, test changes in a non-production consumer first, then roll out in rings.
Simple: A cake recipe card. Everyone uses version 2, you test version 3 in one kitchen first.

**Q39. What is JFrog Artifactory used for?**
Answer: A repository for packages, container images, and Helm charts, with access control and scanning, so teams pull from one controlled source.
Simple: A school library where only approved books are kept.

**Q40. How would you deploy a FastAPI service on AKS?**
Answer: Containerise with a slim image, run with Uvicorn and Gunicorn, expose with a Service and Ingress, use health endpoints for probes, and pass config through Helm values and Key Vault.
Simple: Put the restaurant in a food truck, park it at the right spot, and check the sign on the door to see if it's open.

---

## LEVEL 5: About 7 years experience

**Q41. How would you run a platform used by 18 applications?**
Answer: Clear ownership and onboarding, standard templates, tenant isolation, SLOs with error budgets, alerting by severity, a monthly capacity and cost review, and a runbook for common incidents.
Simple: A busy airport. Every airline has its own gate, a clear schedule, a control tower, and a plan for when a flight is late.

**Q42. How do you define and track SLOs?**
Answer: Pick a measurable indicator (SLI) like request success rate, set a target (SLO) like 99.9%, and track the error budget. Alerts fire on fast burn.
Simple: A promise of "99 out of 100 days will be good," and a notebook that counts how many bad days you've used.

**Q43. How do you handle a production incident as the lead?**
Answer: Declare severity, assign roles, communicate status on a fixed cadence, mitigate first, then find root cause, then write a blameless post-incident review with actions.
Simple: A fire drill. First put out the fire, then count who's safe, then work out why it started.

**Q44. How do you control cloud cost at scale?**
Answer: Tag every resource, review cost by tag weekly, right-size, use reserved capacity for steady workloads, shut down idle environments, and set budgets with alerts.
Simple: Every family writes their name on their own electricity bill, and the house checks the bills every week.

**Q45. How do you manage Terraform for a large estate?**
Answer: Remote state with locking, modules with versions, plan in pipeline, apply with approval, drift detection, and environment separation.
Simple: One big lego plan, split into smaller boxes, with a sign-off before anyone builds.

**Q46. How do you approach a legacy environment?**
Answer: Inventory first, fix risk (access, exposure, secrets), then cost, then governance with policy, and communicate every change.
Simple: Clean your room: first throw away anything dangerous, then the rubbish, then organise the shelves.

**Q47. How do you mentor a team?**
Answer: Pair on hard tasks, review code with teaching comments, share runbooks, and give people ownership of small pieces before large ones.
Simple: Teach by holding the bike seat, then letting go slowly.

**Q48. How do you balance platform stability and new feature requests?**
Answer: Use error budgets and a clear intake process, plan a fixed share of capacity for reliability work, and say no with a reason and a date.
Simple: A cook who keeps a fixed slot for cleaning the kitchen, even on a busy night.

---

## JD-SPECIFIC (any level)

**Q49. What is BAU and how do you run it?**
Answer: Business As Usual: monitoring, incident handling, health checks, patching, and requests from teams.
Simple: The daily chores that keep the shop open.

**Q50. How do you work with investment engineering teams?**
Answer: Gather requirements, agree on a standard pipeline template, onboard them with examples, and collect feedback to improve the template.
Simple: Ask the customer what they want, show them how to order it, then improve the menu from their feedback.

**Q51. How would you deploy an Angular front end?**
Answer: Build static assets in CI, serve from NGINX in a container or static hosting behind ingress or CDN, and set config at deploy time.
Simple: Bake the cake in one place, then deliver it to the table in a box.

**Q52. What SQL would a DevOps engineer write?**
Answer: Checks after deployments, row counts before and after migrations, simple joins, and MERGE logic.
Simple: Counting the apples before and after you move the basket, to make sure none went missing.

**Q53. Where do you stand on Python?**
Answer: Be honest: you write scripts and automation and use Python for pipelines, but production application coding is an area you're building.
Simple: "I can build the road and the signs. I'm learning to build the cars that drive on it."

**Q54. Why are you interested in investment management?**
Answer: Data-driven decisions and research platforms need reliable, governed infrastructure, and that's the work you want to do.
Simple: You want to keep the library safe and running so the researchers can read the books.

---

## Quick self-check before tomorrow
- Can you explain each answer without reading it?
- Can you give one real example from UBS for Databricks, AKS, and CI/CD?
- Can you say "I'm not sure, here's how I'd find out" without sounding stuck?

---

# AKS and Databricks: 100 Interview Questions (easy words)

Sources used: your resume, the Allianz JD, the awesome-devops-interview Kubernetes list (70 topics).
Note: the Google Doc you shared could not be opened from here (it needs JavaScript). Paste its questions into the chat and I'll add them.

How to read each item:
- **Answer** = what to say.
- **Simple** = the same idea in very easy words.

---

## A. Kubernetes basics (1 to 20)

**1. What is Kubernetes?**
Answer: A system that runs containers across many machines, restarts failed ones, and scales them.
Simple: A manager for many small apps. It keeps them running and moves them if a machine breaks.

**2. What is a container?**
Answer: A package with an app and its libraries. It shares the host OS kernel.
Simple: A lunch box with food and a fork. Everything the meal needs is inside.

**3. What is a Pod?**
Answer: The smallest unit in Kubernetes. One or more containers that share network and storage.
Simple: A room. The containers are people sharing the room and the Wi-Fi.

**4. What is a Node?**
Answer: A machine (VM) in the cluster that runs pods.
Simple: A house. Pods are the rooms inside.

**5. What is a Cluster?**
Answer: Control plane plus nodes, managed together.
Simple: A whole apartment building, with a manager and many flats.

**6. What is the control plane?**
Answer: The brain: API server, scheduler, controller manager, etcd.
Simple: The office that decides where everything goes.

**7. What is the API server?**
Answer: The front door. Every kubectl command and every tool talks to it.
Simple: The reception desk. Everyone asks reception first.

**8. What is etcd?**
Answer: The database that stores all cluster state.
Simple: The notebook where the office writes down everything.

**9. What is the scheduler?**
Answer: Picks which node a new pod runs on, based on free CPU, memory, and rules.
Simple: A seat planner. It finds a free seat for each new person.

**10. What is kubelet?**
Answer: An agent on each node that makes sure the containers in its pods are running.
Simple: The node's supervisor, who checks each room every few seconds.

**11. What is kube-proxy?**
Answer: Handles network rules so a Service IP reaches the right pods.
Simple: A switchboard operator who connects calls to the right person.

**12. What is a Namespace?**
Answer: A way to split one cluster into separate groups (for example, one per tenant).
Simple: Folders inside one drive. Each tenant gets its own folder.

**13. What are Labels and Selectors?**
Answer: Labels are tags on objects. Selectors find objects by tag.
Simple: Stickers on boxes. A selector says "give me all boxes with a red sticker."

**14. What is a ReplicaSet?**
Answer: Keeps a set number of identical pods running.
Simple: "Always keep 3 copies of this toy on the shelf."

**15. What is a Deployment?**
Answer: Manages ReplicaSets, rolling updates, and rollbacks.
Simple: The boss of the ReplicaSet. It handles new versions and undo.

**16. What is a StatefulSet?**
Answer: For apps that need a stable name and stable storage, like databases.
Simple: Seats with name tags. Each person keeps the same seat and notebook.

**17. What is a DaemonSet?**
Answer: Runs one pod on every node, like log collectors.
Simple: One cleaner in every room.

**18. What is a Job and a CronJob?**
Answer: A Job runs a task until it finishes. A CronJob runs a Job on a schedule.
Simple: A Job is "wash the car once." A CronJob is "wash the car every Sunday."

**19. What are ConfigMap and Secret?**
Answer: ConfigMap holds normal settings. Secret holds sensitive values (base64-encoded, not encrypted by default).
Simple: ConfigMap is a notice board. Secret is a locked drawer, but the key is in the office.

**20. What is kubectl?**
Answer: The command line tool to talk to the cluster.
Simple: The remote control for the cluster.

---

## B. Pods, probes, and YAML (21 to 40)

**21. Write a simple Pod YAML.**
Answer:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web
spec:
  containers:
  - name: web
    image: nginx:1.25
    ports:
    - containerPort: 80
```
Simple: Name the pod, say which image to run, and which port to open.

**22. Write a Deployment YAML with 3 replicas.**
Answer:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: web
        image: nginx:1.25
        ports:
        - containerPort: 80
```
Simple: "Keep 3 copies of this app, and here is how to make one copy."

**23. Write a Service YAML.**
Answer:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: web
spec:
  selector:
    app: web
  ports:
  - port: 80
    targetPort: 80
  type: ClusterIP
```
Simple: "Give the web pods one stable address inside the cluster."

**24. What are the Service types?**
Answer: ClusterIP (inside only), NodePort (opens a port on each node), LoadBalancer (creates a cloud load balancer).
Simple: A private phone number, a door on each house, or a public shop front.

**25. Write an Ingress YAML for a tenant.**
Answer:
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: tenant1
spec:
  ingressClassName: nginx
  tls:
  - hosts: [tenant1.example.com]
    secretName: tenant1-tls
  rules:
  - host: tenant1.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: web
            port:
              number: 80
```
Simple: "When someone visits tenant1's address, send them to the web service on port 80, using this certificate."

**26. What is an Ingress?**
Answer: Routes outside HTTP(S) traffic to services by host name and path.
Simple: The gate with signs that point visitors to the right room.

**27. What is an Ingress controller?**
Answer: The software that actually runs the gate, like NGINX or Azure Application Gateway.
Simple: The gate guard. The Ingress is the list of rules he follows.

**28. What is a readiness probe?**
Answer: Checks if a pod can take traffic. If it fails, the pod is removed from the Service.
Simple: "Are you ready to play?" If no, the coach keeps you off the field.

**29. What is a liveness probe?**
Answer: Checks if a pod is alive. If it fails, the pod is restarted.
Simple: "Are you breathing?" If no, restart you.

**30. What is a startup probe?**
Answer: Gives a slow app time to start before liveness checks begin.
Simple: "Wait, the app is slow to wake up. Give it time before you check."

**31. Write a probe YAML.**
Answer:
```yaml
readinessProbe:
  httpGet:
    path: /health
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 10
```
Simple: "Every 10 seconds, knock on /health. Start knocking after 5 seconds."

**32. What are requests and limits?**
Answer: Requests are what a pod needs to be scheduled. Limits are the maximum it can use.
Simple: Requests are the seat you book. Limits are the most food you can eat at the buffet.

**33. Write a resource YAML.**
Answer:
```yaml
resources:
  requests:
    cpu: 250m
    memory: 256Mi
  limits:
    cpu: 500m
    memory: 512Mi
```
Simple: "I need a quarter of a CPU and 256 MB to start. I can use up to half a CPU and 512 MB."

**34. Why does a pod get OOMKilled?**
Answer: It used more memory than its limit, so the container is stopped.
Simple: You ate more than the limit, so the waiter takes your plate.

**35. What is a rolling update?**
Answer: Replaces old pods with new ones a few at a time, so the app stays up.
Simple: Change the team players one by one, not all at once.

**36. How do you roll back a Deployment?**
Answer: `kubectl rollout undo deployment/web`.
Simple: Press undo to go back to the last version.

**37. What is maxSurge and maxUnavailable?**
Answer: maxSurge is how many extra pods can be made. maxUnavailable is how many can be down at once.
Simple: maxSurge is extra chairs. maxUnavailable is how many players can sit out.

**38. What is a PodDisruptionBudget?**
Answer: Says the minimum pods that must stay running during voluntary disruption, like node drains.
Simple: "At least 2 out of 3 players must stay on the field."

**39. What is an HPA?**
Answer: Horizontal Pod Autoscaler adds or removes pods based on CPU or other metrics.
Simple: More customers come, so open more checkouts.

**40. What is a node selector or taint and toleration?**
Answer: Node selector says "run me on these nodes." Taint blocks pods from a node. Toleration lets a pod ignore that block.
Simple: A sign that says "only tall people here" (taint). A tall-person badge (toleration).

---

## C. Storage and secrets (41 to 55)

**41. What is a PV (Persistent Volume)?**
Answer: A piece of storage in the cluster, created by an admin or dynamically.
Simple: A disk in the building.

**42. What is a PVC (Persistent Volume Claim)?**
Answer: A pod's request for storage: how much and what type.
Simple: "I need a 10 GB locker." The claim asks for the locker.

**43. What is a StorageClass?**
Answer: Defines the type of storage and creates PVs on demand.
Simple: A catalogue of locker types. You pick one and it's made for you.

**44. Write a PVC YAML.**
Answer:
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: logs-pvc
spec:
  accessModes: [ReadWriteOnce]
  storageClassName: managed-csi
  resources:
    requests:
      storage: 10Gi
```
Simple: "Give me 10 GB of disk that one pod can write to."

**45. Use the PVC in a pod.**
Answer:
```yaml
volumes:
- name: logs
  persistentVolumeClaim:
    claimName: logs-pvc
containers:
- name: app
  volumeMounts:
  - name: logs
    mountPath: /var/log/app
```
Simple: "Attach the locker to this room, at this door."

**46. What is ReadWriteOnce vs ReadWriteMany?**
Answer: RWO: one node can write. RWX: many nodes can write (needs Azure Files or similar).
Simple: RWO is one key holder. RWX is a shared folder for everyone.

**47. Why do we need PVs for logs?**
Answer: Pods are temporary. Data on the pod disappears when it restarts, so logs go to a PV or a log service.
Simple: Writing on a whiteboard that gets wiped at night. A notebook keeps it.

**48. How do you get secrets into AKS?**
Answer: Use the Key Vault CSI driver, which mounts Key Vault secrets into the pod at start, using workload identity.
Simple: The pod asks the safe for the key at the door, without storing a copy.

**49. What is workload identity?**
Answer: The pod gets an Entra ID token with a federated credential, so it doesn't need a stored password.
Simple: The pod shows an ID badge that the bank already trusts, so it doesn't need a password.

**50. Why not put secrets in YAML?**
Answer: YAML is stored in Git and shared. Anyone with access can read it.
Simple: Writing your PIN on the back of your card.

**51. How are Kubernetes Secrets stored?**
Answer: Base64-encoded in etcd. Encrypt etcd at rest and restrict access with RBAC.
Simple: A note written in code. Anyone who has the code can read it, so lock the notebook.

**52. What is a ConfigMap update problem?**
Answer: Pods don't always reload a ConfigMap automatically. A restart or a rolling update is often needed.
Simple: Changing the menu board doesn't change food already served.

**53. What is a secret rotation with no downtime?**
Answer: Create the new secret, store it as a new Key Vault version, let new pods read it, then disable the old one after the old pods are gone.
Simple: Give the new key to the new guest before taking the old key back.

**54. What is kubeconfig?**
Answer: A file with cluster address and login details for kubectl.
Simple: The address card that tells kubectl where the office is and who you are.

**55. How do you connect AKS to Azure Key Vault from DevOps?**
Answer: Use a service connection with workload identity federation, so the pipeline gets a short token and stores no secret.
Simple: The pipeline shows its badge at the vault door, and the badge expires quickly.

---

## D. AKS operations and upgrades (56 to 70)

**56. How do you upgrade AKS with zero downtime?**
Answer: Upgrade the control plane first. Then upgrade node pools one at a time with surge nodes. Cordon and drain each old node, respecting PodDisruptionBudgets.
Simple: Build a new bus, move passengers across, then remove the old bus. One bus at a time.

**57. What is cordon and drain?**
Answer: Cordon stops new pods landing on a node. Drain moves existing pods off it.
Simple: Cordon puts a "closed" sign on a lane. Drain sends the cars to other lanes.

**58. What is a node pool?**
Answer: A group of nodes with the same size and settings.
Simple: A team of players with the same jersey.

**59. What is AKS surge upgrade?**
Answer: Adds extra nodes during an upgrade so pods can move before old nodes are removed.
Simple: Park a spare car before you take one off the road.

**60. How do you do a node pool blue/green upgrade?**
Answer: Create a new node pool with the new version, move workloads, then delete the old pool.
Simple: Build a second house next door, move the family, then knock down the old house.

**61. How do you handle AKS version end of life?**
Answer: Track the support calendar, test the new version in dev, upgrade higher environments on a schedule, and keep a rollback plan.
Simple: Check the milk date. Test a new carton first, then use it.

**62. What is a private AKS cluster?**
Answer: The API server has only a private IP inside your VNet.
Simple: A school office that only takes calls from inside the school.

**63. What are the node pool types?**
Answer: System pools run system pods. User pools run your apps.
Simple: One team runs the building. Another team runs the shops.

**64. How do you give each tenant its own node pool?**
Answer: Create a pool per tenant, use node labels and taints, and map the tenant's namespace to that pool.
Simple: Each family gets its own table, and a sign says "only this family sits here."

**65. How do you charge tenants for cost?**
Answer: Tag the node pool and namespace, use AKS cost analysis or Azure Cost Management, and report usage per tag.
Simple: Put a name label on each shelf so you can bill each family for what they used.

**66. What is cluster autoscaler?**
Answer: Adds nodes when pods can't be scheduled, and removes nodes when they're idle.
Simple: Open a new room when the hall is full, close it when empty.

**67. What is KEDA?**
Answer: Scales pods based on events, like queue length or Event Hub lag.
Simple: Hire more cashiers when the queue gets long.

**68. How do you monitor AKS?**
Answer: Container insights, Azure Monitor, Prometheus and Grafana, and KQL on Log Analytics.
Simple: Cameras (metrics), a diary (logs), and a dashboard on the wall.

**69. What is a healthy alert on AKS?**
Answer: Pod restarts, pods not ready, node not ready, and high CPU or memory for a long time.
Simple: Alarms for "someone fell," "someone is missing," and "room is too full."

**70. How do you troubleshoot a pod stuck in Pending?**
Answer: `kubectl describe pod` to see events. Check for not enough CPU or memory, node taints, or PVC not bound.
Simple: The pod is waiting for a seat. Check if there are no seats, or the seat is reserved.

---

## E. Networking (71 to 80)

**71. What is a VNet and a subnet?**
Answer: VNet is your private network. Subnets divide it.
Simple: A big plot of land (VNet) cut into smaller plots (subnets).

**72. What is an NSG?**
Answer: Allow or deny rules for traffic on a subnet or NIC.
Simple: A guard with a list of who can enter.

**73. What is a private endpoint?**
Answer: A private IP inside your VNet for an Azure service, like storage or Databricks.
Simple: A private door from your house to a shop, so you never walk on the street.

**74. What is a private DNS zone?**
Answer: A DNS zone linked to your VNet, so names resolve to private IPs.
Simple: An address book that says "use the back door," not the street address.

**75. How does AKS connect to a Databricks private endpoint?**
Answer: Pods use the VNet's DNS. The Databricks name resolves to the private IP through the linked private DNS zone. Traffic stays inside the VNet.
Simple: The pod looks up the address in the house address book, finds the back door, and walks through it.

**76. Why does private connectivity often fail?**
Answer: Most often a DNS problem: missing private DNS zone link, or a wrong record.
Simple: The address book has the wrong address.

**77. How do you test DNS from a pod?**
Answer: `kubectl run -it --rm dnstest --image=busybox -- nslookup <name>`.
Simple: Send a little test visitor to ask the address book.

**78. What is kube-proxy and CNI?**
Answer: kube-proxy routes service traffic. CNI gives pods their IP addresses (Azure CNI gives VNet IPs).
Simple: kube-proxy is the switchboard. CNI hands out the room numbers.

**79. What is a Network Policy?**
Answer: Rules that say which pods can talk to which pods.
Simple: A friend list. Only friends can call you.

**80. Give a simple Network Policy YAML.**
Answer:
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-web
spec:
  podSelector:
    matchLabels:
      app: web
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: gateway
```
Simple: "Only the gateway pods can talk to the web pods."

---

## F. Databricks (81 to 90)

**81. What is Azure Databricks?**
Answer: A managed Spark platform for data and AI work.
Simple: A big workshop with machines for crunching data.

**82. What is a Databricks job?**
Answer: A scheduled or triggered run of notebooks or code, with retries and alerts.
Simple: An alarm clock that starts a set of tasks.

**83. What is an instance pool?**
Answer: Ready VMs kept warm, so clusters start fast. You pay for idle VMs.
Simple: A waiting room with chairs kept warm for the next customer.

**84. How do you upgrade a Databricks runtime with zero downtime?**
Answer: Test the new runtime on a dev job, pin the runtime per job, roll out by team, and keep the old runtime for a rollback window.
Simple: Test the new recipe in a small kitchen first. Keep the old recipe until the new one works.

**85. How do you isolate teams in Databricks?**
Answer: Separate pools per team, AD group permissions, cluster policies, and job tags that send jobs to the right pool.
Simple: Each team has its own tray, and the tray only fits their dishes.

**86. What is Delta Lake?**
Answer: A table format with transaction logs. It gives ACID transactions and time travel.
Simple: A notebook that writes every change in ink with the date.

**87. What is Unity Catalog?**
Answer: Central governance for data across workspaces, with grants, lineage, and audit logs.
Simple: One library card for every branch of the library.

**88. How do you give a job access to Key Vault?**
Answer: Use a secret scope backed by Key Vault, and a managed identity or service principal with a limited role.
Simple: Give the robot a key that only opens one drawer.

**89. What is a Databricks secret scope?**
Answer: A named place that holds secrets, read in code with `dbutils.secrets.get`.
Simple: A labelled box inside the safe that the job can open.

**90. What is VNet injection for Databricks?**
Answer: Deploys the Databricks compute into your own VNet, with public and private subnets.
Simple: Building the workshop inside your own gated plot of land.

---

## G. Ops, incidents, and the JD (91 to 100)

**91. How do you handle a production incident?**
Answer: Acknowledge, assess impact, mitigate first, communicate status, find root cause, then write a post-incident review with actions.
Simple: Stop the leak first, tell everyone, then find where the pipe broke.

**92. What is an SLO?**
Answer: A target for a measure, like 99.9% of requests succeed. The error budget is the allowed failure.
Simple: A promise: "99 out of 100 days will be good."

**93. How do you manage a production change safely?**
Answer: Test in dev and staging, get review and approval, deploy in stages, have a rollback plan, and watch metrics after the change.
Simple: Practise the dance in a small room before the big stage.

**94. How do you handle a tenant that asks for an urgent change?**
Answer: Check impact and risk, agree a timeline, follow the change process, and document the decision.
Simple: Say yes to the request, but check the lock on the door first.

**95. What is BAU?**
Answer: Business As Usual: daily running, monitoring, support, and patching.
Simple: The daily chores that keep the shop open.

**96. How would you deploy a FastAPI app on AKS?**
Answer: Containerise it with a small base image, expose with a Service and Ingress, use a health endpoint for probes, and load config from Key Vault.
Simple: Put the app in a box, give it a door (Service), and a street sign (Ingress).

**97. What is JFrog Artifactory?**
Answer: A store for packages, images, and Helm charts, with access control.
Simple: A school library with only approved books.

**98. How do you handle Python in production?**
Answer: Use virtual environments, pinned dependencies, tests, and the same container image in every environment.
Simple: Pack the same lunch box every day, with the same food, so it tastes the same everywhere.

**99. What is Helm?**
Answer: A package manager for Kubernetes. Templates plus values per environment.
Simple: A recipe with blanks, where you fill in the flavour for each family.

**100. What do you do when you don't know an answer in an interview?**
Answer: Say you don't know, explain how you'd find out, and give what you do know about the topic.
Simple: "I don't know that one, but here is how I'd check it." That's the honest and strong answer.

---

## Prep tips
- Practise the YAML from memory: Pod, Deployment, Service, Ingress, PVC.
- Be ready to say how a request travels: DNS → ingress → service → pod.
- Be ready for the zero-downtime question for both AKS and Databricks.

---

# Resume-Based Interview Questions (120) – 1 to 7 Years

Each question is tied to something on your resume or the Allianz JD.
- **[Resume]** = the claim on your resume it tests.
- **Answer** = what to say, in short form.
- **Simple** = the same idea in very easy words.

---

## LEVEL 1 – About 1 year (Q1–30)

**Q1. [Resume: Azure platform] What is Azure, and what does an Azure resource group do?**
Answer: Azure is Microsoft's cloud. A resource group holds related resources so you can manage them together.
Simple: Azure is a rented computer warehouse. A resource group is one shelf for one project.

**Q2. [Resume: AKS] What is AKS?**
Answer: Azure's managed Kubernetes. Azure runs the control plane; you run workloads on nodes.
Simple: A kitchen that Microsoft manages, where you cook the meals.

**Q3. [Resume: Databricks] What is Azure Databricks?**
Answer: A managed Apache Spark platform for data engineering and analytics.
Simple: A big factory for sorting and counting huge piles of data.

**Q4. [Resume: Key Vault] What is Azure Key Vault?**
Answer: A service to store secrets, keys, and certificates with access control and audit logs.
Simple: A locked safe with a log book that writes who opened it.

**Q5. [Resume: RBAC] What is RBAC?**
Answer: Give people roles (Reader, Contributor) at a scope (resource, group, or subscription).
Simple: Giving a child a key to their room only.

**Q6. [Resume: Azure DevOps] What is Azure DevOps?**
Answer: Tools for repos, pipelines, boards, and artifacts.
Simple: A toolbox for building, testing, and shipping code.

**Q7. [Resume: GitLab CI/CD] What is a CI/CD pipeline?**
Answer: Automated steps that build, test, and deploy code on every change.
Simple: An assembly line that checks and packs each product.

**Q8. [Resume: Git] What is a branch?**
Answer: A separate line of work in Git, merged back after review.
Simple: A draft copy of your homework before you hand in the final.

**Q9. [Resume: pipeline templates] What is a pipeline template?**
Answer: Reusable pipeline code that many pipelines call, so you write steps once.
Simple: A recipe card that many cooks use.

**Q10. [Resume: SonarQube] What does SonarQube do?**
Answer: Scans code for bugs, security issues, and code smells, and can block a pipeline on failure.
Simple: A spell-checker for code that stops bad writing from being printed.

**Q11. [Resume: Terraform] What is Terraform?**
Answer: Infrastructure as code: you describe Azure resources in files and Terraform creates them.
Simple: A blueprint that a builder follows to build the same house each time.

**Q12. [Resume: ARM templates] What is an ARM template?**
Answer: Azure's JSON format to describe and deploy resources.
Simple: A shopping list in Azure's own language.

**Q13. [Resume: Helm] What is Helm?**
Answer: A package manager for Kubernetes with templates and values.
Simple: A recipe with blanks you fill per family.

**Q14. [Resume: Prometheus] What is Prometheus?**
Answer: A tool that collects metrics (numbers over time) from apps and clusters.
Simple: A person writing down the temperature every minute.

**Q15. [Resume: Grafana] What is Grafana?**
Answer: A dashboard tool that draws graphs from metrics and logs.
Simple: The TV screen that shows the graphs.

**Q16. [Resume: Azure Monitor] What is Azure Monitor?**
Answer: Azure's service for metrics, logs, and alerts.
Simple: The CCTV for your Azure resources.

**Q17. [Resume: KQL] What is KQL?**
Answer: The query language for searching logs in Log Analytics.
Simple: A search box for a huge diary.

**Q18. [Resume: KQL alerts] How do you use KQL for alerts?**
Answer: Write a query that finds a known error, set a rule to run it every few minutes, and fire an alert when results appear.
Simple: "Tell me when the word ERROR appears more than 5 times in 10 minutes."

**Q19. [Resume: incidents] What is an incident?**
Answer: An unplanned event that stops or slows a service.
Simple: The shop lights going off during business hours.

**Q20. [Resume: ServiceNow] What is ServiceNow used for?**
Answer: Ticket and incident management: logging, assigning, and tracking issues.
Simple: The office's to-do board for problems.

**Q21. [Resume: BigPanda] What does BigPanda do?**
Answer: Groups many alerts into one incident, so teams see fewer, clearer alerts.
Simple: Turns a hundred ringing bells into one clear bell.

**Q22. [Resume: Network] What is a VNet?**
Answer: Your private network in Azure.
Simple: Your own fenced yard.

**Q23. [Resume: Network] What is a subnet?**
Answer: A part of a VNet with its own address range.
Simple: A room inside the yard, with its own gate number.

**Q24. [Resume: NSG] What is an NSG?**
Answer: Rules that allow or deny traffic to a subnet or NIC.
Simple: A guard list at the gate.

**Q25. [Resume: Private Endpoint] What is a private endpoint?**
Answer: A private IP in your VNet for an Azure service.
Simple: A private back door into a shop.

**Q26. [Resume: Secrets] Why keep secrets out of code?**
Answer: Code is shared and stored in Git. Anyone with access could read the secret.
Simple: Writing your bank PIN on the back of your card.

**Q27. [Resume: Key Vault secrets] How does an app read a Key Vault secret?**
Answer: The app uses a managed identity or service principal with read access, then fetches the secret.
Simple: The app shows its ID badge to get the key from the safe.

**Q28. [Resume: Service principal] What is a service principal?**
Answer: A non-human identity in Entra ID used by apps and pipelines.
Simple: A robot's login.

**Q29. [Resume: Containers] What is a Docker image?**
Answer: A packaged template for a container: app, libraries, settings.
Simple: A cake mould. Each container is a cake made from it.

**Q30. [Resume: Linux] What is a shell script used for?**
Answer: Automating commands on Linux or in pipelines.
Simple: A list of instructions you write once and run many times.

---

## LEVEL 2 – About 2 to 3 years (Q31–60)

**Q31. [Resume: AKS onboarding] How do you onboard a new tenant onto a shared AKS cluster?**
Answer: Create a namespace, quotas, RBAC for their group, a node pool if needed, and network policies.
Simple: Give a new family a room, a key, a meter, and a fence.

**Q32. [Resume: Tenant isolation] How do you keep tenants apart?**
Answer: Namespaces, ResourceQuotas, NetworkPolicies, and node pools.
Simple: Separate rooms, separate doors, and separate electricity meters.

**Q33. [Resume: Chargeback] How do you charge tenants?**
Answer: Tag resources and node pools per tenant, then report cost per tag.
Simple: Name stickers on each shelf for the bill.

**Q34. [Resume: IaC modules] What did your Terraform/ARM modules do?**
Answer: Built reusable modules for resource groups, storage, Key Vault, and access, so each tenant gets the same setup.
Simple: A standard set of Lego pieces used for every new house.

**Q35. [Resume: 40% faster onboarding] How did you measure the 40% faster onboarding?**
Answer: Compared the time to onboard a tenant before and after modules, using past tickets.
Simple: "Before, it took 10 days. Now it takes 6."

**Q36. [Resume: Pipelines] How did you cut deployment time to 15 minutes?**
Answer: Reusable templates, parallel steps, cached dependencies, and removing manual waits.
Simple: Cooking with more hands, and prepping ingredients before the guests arrive.

**Q37. [Resume: 85% faster] What did "85% faster" mean?**
Answer: Deployments went from hours to about 15 minutes, measured by pipeline run time.
Simple: The trip used to take 2 hours. Now it takes 15 minutes.

**Q38. [Resume: Approval gates] What is an approval gate?**
Answer: A check where a person must approve before the pipeline moves to the next environment.
Simple: A teacher signs your notebook before you go to the next class.

**Q39. [Resume: Environment promotion] How do you promote a release from dev to prod?**
Answer: Build once, deploy the same artifact through dev, test, and prod, with approvals.
Simple: Bake one cake, then carry the same cake to each party.

**Q40. [Resume: SonarQube gates] What is a quality gate?**
Answer: A rule that fails the build if code coverage or bug count is too high.
Simple: A speed bump that stops bad cars leaving the car park.

**Q41. [Resume: Databricks admin] What do you do as Databricks workspace admin?**
Answer: Create clusters and pools, manage users and groups, set policies, and watch job health.
Simple: The school principal who sets the timetable and the rules.

**Q42. [Resume: Instance pools] What is an instance pool?**
Answer: Pre-warmed VMs that clusters can start from faster.
Simple: A waiting room with seats already reserved.

**Q43. [Resume: Cluster policies] What is a cluster policy?**
Answer: Rules that limit which cluster sizes and settings users can pick.
Simple: A menu with some dishes removed, so no one orders the most expensive one.

**Q44. [Resume: Job failures] How do you alert on a Databricks job failure?**
Answer: Set a failure alert on the job, send it to the incident tool, and check the run logs.
Simple: A bell that rings when the job stops.

**Q45. [Resume: Monitoring AKS] What do you monitor on AKS?**
Answer: Node health, pod restarts, pods not ready, CPU, memory, and failed deployments.
Simple: The temperature and heartbeat of each room.

**Q46. [Resume: Secrets levels] How do you decide where a secret goes?**
Answer: Low-sensitivity config goes in a shared library. App secrets go in Key Vault. High-risk credentials go in HashiCorp Vault with stricter access.
Simple: Shopping list on the fridge, house keys in a drawer, bank keys in a safe.

**Q47. [Resume: HashiCorp Vault] What is HashiCorp Vault?**
Answer: A secrets tool that issues and manages secrets, often across clouds.
Simple: A second safe with its own rules.

**Q48. [Resume: Secret access] How does a pipeline get a secret from HashiCorp Vault?**
Answer: The pipeline authenticates with a certificate or identity, then reads the secret at runtime.
Simple: The pipeline shows a card and the vault hands over the key only for that job.

**Q49. [Resume: Key rotation] How do you rotate a credential safely?**
Answer: Create new, update the store, roll out, verify, then disable old.
Simple: Hand over the new key before taking the old one back.

**Q50. [Resume: Log retention] How do you keep logs for a set time?**
Answer: Set retention on Log Analytics and archive if needed.
Simple: A diary with a rule that old pages move to the attic after a set time.

**Q51. [Resume: Runbooks] Why write runbooks?**
Answer: So anyone can fix a known issue without waiting for the expert.
Simple: A recipe for fixing the lights, so the family can do it at night.

**Q52. [Resume: Incidents as SME] What does SME mean in incidents?**
Answer: Subject-matter expert: the person who knows the platform best and joins to fix it.
Simple: The person the whole class calls when the projector breaks.

**Q53. [Resume: Copilot] How do you use GitHub Copilot?**
Answer: For scripts, pipeline templates, and troubleshooting, with your team's rules on review.
Simple: A helper who suggests words, but you still check the sentence.

**Q54. [Resume: Terraform state] Where does Terraform keep state?**
Answer: In a remote backend, usually Azure Blob Storage, with locking.
Simple: A notebook in a safe that only one person can write in at a time.

**Q55. [Resume: Drift] What is drift?**
Answer: When the real resource no longer matches the code or state.
Simple: The house has a new door, but the blueprint still shows the old wall.

**Q56. [Resume: ARM incremental] What is incremental mode in ARM?**
Answer: Deploys the template and leaves other resources alone.
Simple: Add the new items to the shelf without removing the rest.

**Q57. [Resume: Ingress] What does an ingress do for tenants?**
Answer: Routes each tenant's host name or path to their service.
Simple: The gate sign that sends visitors to the right house.

**Q58. [Resume: Probes] What are readiness and liveness probes?**
Answer: Readiness says "ready for traffic." Liveness says "still alive, else restart."
Simple: "Are you ready to play?" and "Are you breathing?"

**Q59. [Resume: Resource limits] Why set CPU and memory limits?**
Answer: So one app can't starve the others on a shared cluster.
Simple: A rule that no one takes more than their share of the cake.

**Q60. [Resume: Node pools] Why separate node pools?**
Answer: For different workloads, sizes, or tenants, and for cost tracking.
Simple: Separate trucks for different loads.

---

## LEVEL 3 – About 4 to 5 years (Q61–90)

**Q61. [Resume: 99.9% SLO] What is an SLO?**
Answer: A target for a service measure, like 99.9% of requests succeed.
Simple: A promise: 999 out of 1000 days will be fine.

**Q62. [Resume: SLI] What is an SLI?**
Answer: The actual measure, like the percentage of successful requests.
Simple: The score on the test. The SLO is the score you promised.

**Q63. [Resume: Error budget] What is an error budget?**
Answer: The allowed failures before the SLO is broken.
Simple: How many mistakes you're allowed this month.

**Q64. [Resume: Uptime tracking] How did you track availability?**
Answer: Dashboards in Grafana and Azure Monitor with KQL, compared to the target.
Simple: A wall screen showing how many minutes the lights stayed on.

**Q65. [Resume: Cost 24%] How did you get the 24% cost cut?**
Answer: Moved VMs to AKS containers, removed idle VMs, and right-sized pools.
Simple: Stopped renting a big house for one person and moved into a flat that fits.

**Q66. [Resume: VM migration] How did you move from VMs to containers?**
Answer: Inventory, containerise, write Helm charts, test, cut over, then shut down VMs after a rollback window.
Simple: Pack the house, move in, then sell the old house after a week.

**Q67. [Resume: Certificates in containers] How did you handle certificates for the in-house deploy tool?**
Answer: Stored in Key Vault and mounted at runtime, not kept on a VM.
Simple: The certificate lives in the safe and is handed to the worker when needed.

**Q68. [Resume: Compliance upgrades] What upgrade work did compliance require?**
Answer: OS upgrades such as RHEL 7 to 8, and patching for security policy.
Simple: Replacing old locks with new ones that the bank requires.

**Q69. [Resume: AKS upgrades] How do you upgrade AKS without downtime?**
Answer: Control plane first, then node pools one at a time with surge, cordon, drain, and PodDisruptionBudgets.
Simple: One bus at a time, passengers moved first.

**Q70. [Resume: Databricks runtime] How do you upgrade a Databricks runtime?**
Answer: Test on dev, pin per job, roll out by team, and keep the old runtime for rollback.
Simple: Test the new recipe in a small kitchen first.

**Q71. [JD: Databricks Asset Bundles] What are Databricks Asset Bundles?**
Answer: Define jobs and pipelines as YAML, deploy to dev, test, and prod targets with the CLI.
Simple: A recipe card that works in any kitchen.

**Q72. [JD: CI/CD for bundles] How would you set up CI/CD for a bundle?**
Answer: Validate, lint, test, deploy to dev, then promote to prod with approval and a service principal.
Simple: Check the card, cook a test, then serve the dish after approval.

**Q73. [Resume: Private connectivity] How does AKS reach Databricks privately?**
Answer: Pods use VNet DNS. The Databricks name resolves to a private IP through a linked private DNS zone.
Simple: The address book points to the back door.

**Q74. [Resume: Private DNS] What breaks private connectivity most often?**
Answer: Missing private DNS zone link or a wrong record.
Simple: Wrong address in the book.

**Q75. [Resume: Network design] Why did you use separate subnets for AKS and Databricks?**
Answer: Each service needs its own range and rules, so you can control traffic per service.
Simple: Separate rooms, each with its own door rules.

**Q76. [JD: Azure Data Lake and SQL] What is ADLS Gen2?**
Answer: Azure's data lake storage with folders and access control lists.
Simple: A big filing cabinet with real folders.

**Q77. [JD: Azure SQL] How do you secure Azure SQL?**
Answer: Private endpoint, Entra ID login, least-privilege roles, and no public access.
Simple: A safe in a private room with a card lock.

**Q78. [JD: Unity Catalog] What does Unity Catalog give you?**
Answer: Central grants on catalogs, schemas, and tables, plus lineage and audit.
Simple: One library card for all branches.

**Q79. [JD: Unity Catalog plan] Why move to Unity Catalog in 2027?**
Answer: Central control of data access, fewer per-workspace settings, and better audit.
Simple: Stop keeping separate key books for each door.

**Q80. [Resume: Team of 5] How do you lead a team of five?**
Answer: Split work by platform area, review pipeline changes, and set clear priorities for urgent work.
Simple: Give each person a lane, check the traffic, and move someone when a crash happens.

**Q81. [Resume: Incident leadership] How do you run a major incident?**
Answer: Assign roles, communicate updates on a fixed cadence, mitigate first, then root cause.
Simple: One person shouts orders, one writes notes, one calls the customers.

**Q82. [Resume: Post-incident review] What is a post-incident review?**
Answer: A blameless write-up of what happened, why, and what we'll change.
Simple: A class review of a test, to learn, not to blame.

**Q83. [JD: BAU] How do you run BAU on the platform?**
Answer: Daily health checks, alert review, patching, and tenant support.
Simple: Daily chores that keep everything working.

**Q84. [Resume: Vendor/stakeholders] How do you handle a tenant asking for an urgent change?**
Answer: Check risk and impact, agree a date, follow the change process, and document it.
Simple: Say yes to the request, but check the lock before opening the door.

**Q85. [Resume: Capacity] How do you plan capacity for pools?**
Answer: Track usage trends, set idle limits, and raise quota before the peak.
Simple: Buy more chairs before the wedding, not during it.

**Q86. [Resume: Quota] What happens when a region runs out of VM quota?**
Answer: Jobs fail to start. You request a quota increase or move the job to another pool or region.
Simple: The car park is full. Find another car park or ask for more spaces.

**Q87. [Resume: Job routing] How do you route a job to a specific pool?**
Answer: Tag the job with the pool it should use.
Simple: Put a label on the box that says which shelf it goes on.

**Q88. [Resume: Security reviews] How do you handle a security finding?**
Answer: Triage by severity, fix the risk, record the decision, and retest.
Simple: A doctor checks the scan, treats the worst first, then re-checks.

**Q89. [Resume: Network security] How do you lock down a new subnet?**
Answer: Add NSG rules that allow only needed ports, use private endpoints, and deny the rest.
Simple: Lock all doors, then open only the ones you need.

**Q90. [Resume: Governance] How do you enforce tags and regions?**
Answer: Azure Policy with deny or audit effects.
Simple: A rule book the system checks every time you build something.

---

## LEVEL 4 – About 6 years (Q91–105)

**Q91. [Resume: Platform design] How do you design a shared platform for many tenants?**
Answer: Clear isolation, standard templates, quotas, cost tags, SLOs, and an onboarding process.
Simple: An apartment block with rules for each flat.

**Q92. [Resume: Governance at scale] How do you keep five platforms consistent?**
Answer: Shared templates, version pins, the same policies, and one monitoring standard.
Simple: Five restaurants using the same recipe book.

**Q93. [Resume: Cost ownership] How do you own cost at platform level?**
Answer: Budgets, anomaly alerts, monthly reviews, and right-sizing.
Simple: A family budget checked every month.

**Q94. [JD: Monitoring] How do you design alerting that doesn't flood people?**
Answer: Alert on symptoms (user impact), group alerts, and set severity levels.
Simple: Ring the bell only for fire, not for every spark.

**Q95. [Resume: KQL incidents] How do you avoid false incident alerts?**
Answer: Tune thresholds, add time windows, test the query on past data.
Simple: Set the smoke alarm to ignore toast.

**Q96. [Resume: Incident handover] How do you hand over a platform to another team?**
Answer: Runbooks, ownership map, alert list, and a shadow period.
Simple: Teach the new person the house rules and stand next to them for a week.

**Q97. [Resume: Terraform scale] How do you manage Terraform for many tenants?**
Answer: Modules with versions, remote state per tenant, plan in pipeline, apply with approval.
Simple: One blueprint, many houses, one signature per house.

**Q98. [Resume: GitLab templates] How do you version shared templates?**
Answer: Tag releases, consumers pin a tag, and roll out in rings.
Simple: Use recipe version 2 until the team tests version 3.

**Q99. [JD: FastAPI on AKS] How would you deploy a FastAPI service?**
Answer: Container image, Deployment, Service, Ingress, probes, config from Key Vault.
Simple: Put the app in a box, give it a door, and a sign on the street.

**Q100. [JD: Angular on AKS] How would you deploy an Angular app?**
Answer: Build static files in CI, serve with NGINX in a container, route through ingress.
Simple: Bake the bread, then serve it in a bakery window.

**Q101. [JD: JFrog] Why use JFrog Artifactory?**
Answer: One controlled source for packages and images, with scanning and access rules.
Simple: A school library with only approved books.

**Q102. [JD: Helm values] How do you manage Helm values per environment?**
Answer: Base values plus per-environment overrides in Git, secrets from Key Vault.
Simple: One recipe, with a different seasoning for each restaurant.

**Q103. [Resume: Secrets at scale] How do you audit secrets access?**
Answer: Key Vault diagnostic logs, access reviews, and alerts on unusual reads.
Simple: The safe's guest book, checked every week.

**Q104. [Resume: Least privilege] How do you cut back an over-broad service principal?**
Answer: List what it needs, create a narrower role, move it, test, and remove the broad role.
Simple: Trade a master key for keys to only the rooms it needs.

**Q105. [Resume: Mentoring] How do you grow engineers on your team?**
Answer: Pair on hard tasks, review code with teaching comments, and give ownership step by step.
Simple: Hold the bike seat, then let go slowly.

---

## LEVEL 5 – About 7 years (Q106–120)

**Q106. [Resume: Platform strategy] How would you set a 12-month platform roadmap?**
Answer: Talk to tenants, list risks and cost, prioritise by risk then value, and tie each item to a measure.
Simple: Make a plan for the year based on what could break and what costs the most.

**Q107. [JD: Stakeholders] How do you handle a disagreement with a tenant?**
Answer: Listen, show data, offer options, and agree on a decision with a review date.
Simple: Show the evidence, offer two choices, and agree to check again next month.

**Q108. [Resume: Reliability] How do you reduce incident count over time?**
Answer: Track top causes, fix the top three, add alerts for early signs, and review trends monthly.
Simple: Fix the leak that drips most often first.

**Q109. [Resume: Business continuity] What is BCM?**
Answer: Business continuity management: plans for keeping services running during disasters.
Simple: A fire plan for the building.

**Q110. [Resume: DR] How would you design disaster recovery for AKS and Databricks?**
Answer: Backups of config and data, a second region if needed, tested runbooks, and agreed recovery time and point targets.
Simple: A spare key, a second house, and a drill to check it works.

**Q111. [Resume: Regulated environments] How do you run releases in a bank?**
Answer: Change approvals, evidence logs, segregation of duties, and staged rollouts.
Simple: Every change needs a signature and a receipt.

**Q112. [Resume: Red zone] What is a red zone in a bank?**
Answer: A strict area with sensitive data, with tighter controls.
Simple: A room with the strongest lock in the building.

**Q113. [JD: Investment data] Why does investment data need strict control?**
Answer: Errors can affect decisions, and access rules protect confidential data.
Simple: Wrong numbers in the wrong hands cause big losses.

**Q114. [JD: Team growth] How do you grow your team's skills?**
Answer: Set learning goals, rotate people across areas, and run short internal demos.
Simple: Move people between stations so everyone can cook.

**Q115. [Resume: Hiring] How would you hire for your platform team?**
Answer: Write clear role needs, use a practical test, and check both skill and teamwork.
Simple: Hire by asking them to do a small real task.

**Q116. [Resume: Budget] How do you justify platform spend to leaders?**
Answer: Show cost per tenant, savings made, risk avoided, and forecast.
Simple: Show the bills, the savings, and the danger you prevented.

**Q117. [Resume: Vendor choice] How do you decide build versus buy?**
Answer: Compare cost, risk, skills, and time to value.
Simple: Make it yourself only if it's cheaper and you can maintain it.

**Q118. [Resume: Process] How do you improve a slow release process?**
Answer: Measure lead time, remove manual steps, automate checks, and review monthly.
Simple: Watch where the queue forms and remove the bottleneck.

**Q119. [Resume: Ownership] What is your biggest technical decision?**
Answer: Pick a real one: for example, moving VMs to AKS, with the reason and result.
Simple: Tell the story of one big choice and why it worked.

**Q120. [JD: Honest gaps] What are your gaps for this role?**
Answer: Python production depth, Terraform depth, and Unity Catalog ownership. Say what you're doing about each.
Simple: "I'm good at X. I'm learning Y. Here's my plan."

---

---

## SCENARIO QUESTIONS (S1–S20) – fuller answers

**S1. How do you know AKS has a problem?**
Answer: I'd check node status, pods not ready, pod restarts, and recent warning events, then confirm with alerts in Azure Monitor. In practice the first signal is usually an alert on failed pods or a node going NotReady. I'd then drill into the node or pod that's affected.
Simple: The dashboard lights up red. You open the room with the red light and look at what's wrong.

**S2. How does AKS report autoscaling?**
Answer: The cluster autoscaler writes events when it adds or removes a node, and its status is stored in a ConfigMap (cluster-autoscaler-status). I'd check those events and the status for pending pods that couldn't be scheduled. HPA shows its decisions in `kubectl describe hpa`.
Simple: The helper writes a diary entry every time it opens or closes a room, and says why.

**S3. A pod shows Pending. How do you find why?**
Answer: Run `kubectl describe pod` and read the Events section. Common reasons: insufficient CPU or memory, a node taint with no matching toleration, or a PVC that isn't bound. Then fix the cause (add capacity, fix the toleration, or fix the storage).
Simple: The pod is waiting for a seat. The event log says whether there are no seats, or the seat is reserved for someone else.

**S4. What is horizontal scaling?**
Answer: Adding more pod copies so the load is shared. In Kubernetes this is done by the HPA, based on CPU, memory, or custom metrics. It needs CPU requests set on the pods, because the percentage is calculated from requests.
Simple: More cashiers when the queue grows.

**S5. What is vertical scaling?**
Answer: Giving one pod more CPU or memory. The VPA can recommend or change requests and limits. Changing a pod's size usually restarts it, so it needs planning.
Simple: Giving one cashier a bigger till, not hiring a second cashier.

**S6. How does HPA decide when to scale?**
Answer: It compares the current average metric to the target, for example average CPU at 80% when the target is 60%. It then scales the replica count up or down, within min and max limits, with a cooldown to avoid flapping.
Simple: If the team is working too hard, hire more people. If they're idle, send some home, but not every minute.

**S7. Why might HPA not scale even when CPU is high?**
Answer: The metrics server might not be running, the pods might have no CPU requests (so the percentage can't be calculated), or the replica count may already be at the maximum. I'd check `kubectl get hpa` and its conditions.
Simple: The scale can't measure the load, or it's already at the maximum number of workers.

**S8. How do you check the cluster autoscaler is working?**
Answer: Look for pending pods that stay pending, check the autoscaler status and events for scale-up attempts, and check quota. If it tries to scale and fails, the reason is usually quota or the node pool's max count.
Simple: Look at the waiting line. If it's long and no new rooms open, the helper is stuck, often because there's no more quota for rooms.

**S9. A node is NotReady. What do you check?**
Answer: Check the node conditions (MemoryPressure, DiskPressure, PIDPressure), kubelet status, and the VM health in Azure. Then check if the node was being upgraded or repaired. Cordon the node if it's unstable so no new pods land on it.
Simple: The house supervisor is off duty. Check if the house has power, water, and space.

**S10. A pod keeps restarting. How do you find the cause?**
Answer: Run `kubectl logs --previous` to see the last run's logs, and `kubectl describe pod` for the exit code and last state. Exit code 137 usually means OOMKilled. Failing liveness probes also cause restarts.
Simple: Read the last note the worker wrote before they fell, and check whether they were pushed (memory) or just failed their check (probe).

**S11. How do you tell if a pod is out of memory?**
Answer: The last state shows OOMKilled, and memory usage in Container insights or Prometheus climbs up to the limit then drops. The fix is either a higher memory limit or fixing the leak in the app.
Simple: The bucket overflowed, so the pod was emptied and restarted.

**S12. How do you check CPU throttling?**
Answer: Use the container CPU throttling metric (`container_cpu_cfs_throttled_periods_total` in Prometheus). High throttling means the CPU limit is too low for the load, even if the node has spare CPU.
Simple: The speed limit is too strict, so the car keeps slowing down even on an empty road.

**S13. How do requests and limits affect scaling?**
Answer: HPA uses requests to calculate the percentage. Scheduling uses requests too. Limits cap what the pod can use. Setting requests too low makes HPA react strangely and can pack too many pods onto one node.
Simple: The request is the price you pay to get a seat. The limit is the most you're allowed to eat.

**S14. How would you scale a Databricks job cluster when it runs out of capacity?**
Answer: Check the instance pool and the regional VM quota first. Then move the job to another pool using the job's tag, raise the pool's max size, or request a quota increase. For urgent runs, I'd run the job in a smaller batch.
Simple: The workshop is full, so move the work to another room, or ask for more space before the next shift.

**S15. A tenant says their app is slow. Where do you start?**
Answer: Start at the front: check ingress latency and errors, then the pods' CPU, memory, and throttling, then probe failures, then dependencies like the database or Key Vault. Check for recent deployments in the same window. Tell the tenant what I've checked so far.
Simple: Walk the path from the front gate to the room, and check each stop. Note the time something changed.

**S16. How do you know a deployment is stuck?**
Answer: `kubectl rollout status deployment/<name>` shows it's waiting. Events show why the new pods aren't ready: failed probes, image pull errors, or a missing secret. If stuck, roll back with `kubectl rollout undo`.
Simple: The new bus is at the stop but won't leave. The driver's note says why.

**S17. How do you find which tenant uses the most CPU?**
Answer: Group CPU by namespace in Container insights or Prometheus. Each tenant has its own namespace, so the grouping gives per-tenant use. Pair this with node pool cost tags to explain the bill.
Simple: Count the cups used in each family's kitchen, then use the same count for the bill.

**S18. A pod can't reach Key Vault. What do you check?**
Answer: Check DNS resolution from the pod (does the vault name resolve to a private IP?), the private endpoint and its DNS zone link, the identity the pod uses, its role on the vault, and NSG rules on the subnet. Test with a debug pod to split network from permissions.
Simple: Check the address, the door, and the badge, in that order.

**S19. How do you prepare the platform for a peak event?**
Answer: Raise HPA minimum replicas and pre-scale node pools before the event, check quotas in advance, set alerts with lower thresholds, and assign someone to watch dashboards during the peak. Plan the rollback in case scale-up fails.
Simple: Hire extra staff before the rush, not during it.

**S20. How do you tell if a scaling change helped?**
Answer: Compare latency (p95 and p99), error rate, and cost for the same time window before and after the change. A change that lowers latency but doubles cost needs a decision, not just a celebration.
Simple: Did the line get shorter, and did the bill go up too much?

---

## Tomorrow: quick checklist
- Say numbers with what they measure (for example, 15 minutes for deployments).
- Say "I don't know" with a plan.
- Don't claim Terraform or Unity Catalog depth beyond what you did.

---

