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
