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
