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
