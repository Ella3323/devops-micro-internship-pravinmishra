# Assignment 4 — Auto-Scaling & Auto-Healing: Let Kubernetes React for You

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

Create Deployments, test Pod auto-healing, configure CPU requests and limits, verify Metrics Server, and create a Horizontal Pod Autoscaler.

Optionally generate CPU load to observe how the HPA responds.

---

# Task 0 — Check the Existing Cluster Resources

## Goal

Confirm that you are connected to your learning cluster and inspect existing Deployments and HPAs.

### Evidence

No screenshots required.

---

# Task 1 — Set Up Your Lab Directory

## Goal

Create and enter a dedicated directory for the auto-scaling lab.

### Evidence

#### Screenshot 1 — Terminal showing that you are working inside `~/k8s-labs/autoscaling`

Add your screenshot here.

---

### Notes

**1. What is the difference between auto-healing and auto-scaling?**

Auto-healing keeps the number of Pods the same. If a Pod fails, crashes or is deleted, Kubernetes automatically creates a replacement to restore the desired count. In a Deployment, its ReplicaSet does this: if replicas: 2 is set and one Pod disappears, a new one is created straight away.

Auto-scaling changes the number of Pods. It increases or decreases the desired count based on demand, such as CPU usage. When traffic rises, more Pods are added to handle the load. When traffic falls, extra Pods are removed so resources aren’t wasted.

In short, auto-healing restores the current target, while auto-scaling adjusts the target itself.

---

**2. Which Kubernetes feature will you use for CPU-based auto-scaling?**

The Horizontal Pod Autoscaler (HPA). It reads CPU usage from Metrics Server and adjusts the Deployment’s replica count between a set minimum and maximum. In this lab that’s 2 to 5 Pods, keeping average CPU utilization close to the target of 50% of each Pod’s requested CPU.

---

# Task 2 — Test Auto-Healing with a Deployment

## Goal

Create a Deployment with two replicas, delete one Pod, and observe Kubernetes create a replacement Pod.

### Evidence

#### Screenshot 2 — Completed `autohealing-deployment.yaml` manifest

Add your screenshot here.

---

#### Screenshot 3 — Output of `kubectl get pods` showing two running Pods before deletion

Add your screenshot here.

---

#### Screenshot 4 — Output of the `kubectl delete pod` command

Add your screenshot here.

---

#### Screenshot 5 — Output of `kubectl get pods` showing the replacement Pod and two running Pods

Add your screenshot here.

---

### Notes

**1. What happened after you deleted one Pod?**

I deleted the Pod demo-autohealing-758c898b7-fg6k8, and it was terminated. Kubernetes immediately created a replacement Pod, demo-autohealing-758c898b7-wmrdj, without any action from me. When I checked again, the Deployment had two Pods in the Running state with 1/1 ready: the original Pod demo-autohealing-758c898b7-mf7tm, aged 2 minutes 43 seconds, and the new replacement demo-autohealing-758c898b7-wmrdj, aged only 12 seconds. The desired count of two Pods was restored automatically.

---

**2. What Kubernetes resource maintains the desired replica count for this Deployment?**

The ReplicaSet that the demo-autohealing Deployment created automatically. It continuously compares the desired count (replicas: 2) with the number of running Pods matching its selector (app: autoheal), and creates a new Pod whenever one is missing

---

**3. What proves that auto-healing worked?**

After the deletion, kubectl get pods again showed two demo-autohealing Pods in the Running state. One of them had a new name and a very recent age, proving it was created automatically to replace the deleted Pod.

---

# Task 3 — Create a Deployment for HPA

## Goal

Create a Deployment with CPU resource requests and limits so that HPA can calculate CPU utilization.

### Evidence

#### Screenshot 6 — Completed `hpa-deployment.yaml` manifest showing CPU requests and limits

Add your screenshot here.

---

#### Screenshot 7 — Output showing `nginx-deployment` and its running Pods

Add your screenshot here.

---

### Notes

**1. What does `cpu: "100m"` mean?**

A CPU request is the amount of CPU guaranteed to the container. Kubernetes uses it when scheduling, placing the Pod only on a node with enough free CPU, and reserves that amount for it. In this Deployment, each Pod is guaranteed 100m.

---

**2. What is the difference between a CPU request and a CPU limit?**

A CPU request is the amount of CPU guaranteed to the container. Kubernetes uses it when scheduling, placing the Pod only on a node with enough free CPU, and reserves that amount for it. In this Deployment, each Pod is guaranteed 100m.

A CPU limit is the maximum CPU the container is allowed to use. If the container tries to use more, Kubernetes throttles it (slows it down) instead of letting it take more. In this Deployment, each Pod can use up to 200m.

In short, the request is the minimum it can count on, and the limit is the most it may use.

---

**3. Why does CPU-based HPA require CPU requests?**

A CPU request is the amount of CPU guaranteed to the container. Kubernetes uses it when scheduling, placing the Pod only on a node with enough free CPU, and reserves that amount for it. In this Deployment, each Pod is guaranteed 100m.

A CPU limit is the maximum CPU the container is allowed to use. If the container tries to use more, Kubernetes throttles it (slows it down) instead of letting it take more. In this Deployment, each Pod can use up to 200m.

In short, the request is the minimum it can count on, and the limit is the most it may use.

---

# Task 4 — Enable and Verify Metrics Server

## Goal

Ensure the cluster can collect resource metrics for the HPA.

### Evidence

#### Screenshot 8 — Output showing Metrics Server running, or `kubectl top pods` displaying Pod metrics

Add your screenshot here.

---

### Notes

**1. Why does HPA need Metrics Server?**

The HPA decides whether to add or remove Pods by comparing actual CPU usage with its target. It doesn’t measure CPU itself: it reads those numbers from the Metrics API, which Metrics Server provides by collecting resource usage from each node’s kubelet. Without Metrics Server, the HPA has no data to act on.

---

**2. What issue can occur if Metrics Server is not working?**

The HPA can’t read CPU usage, so its TARGETS column shows <unknown>, and it can’t scale the Deployment up or down, even during heavy load. kubectl top commands also fail with an error such as “Metrics API not available”.

---

**3. What does `kubectl top pods` help you check?**

Add your answer here.

---

# Task 5 — Create a Horizontal Pod Autoscaler

## Goal

Create an HPA that keeps at least two Pods running and can scale up to five Pods when average CPU utilization exceeds 50%.

### Evidence

#### Screenshot 9 — The imperative command output OR completed `hpa-nginx.yaml` manifest

Add your screenshot here.

---

#### Screenshot 10 — Output of `kubectl get hpa` and the relevant `kubectl describe hpa` command

Add your screenshot here.

---

### Notes

**1. What does `minReplicas: 2` mean?**

The HPA will never scale the Deployment below two Pods, even when there’s no traffic. That keeps a baseline of capacity and availability, so the application can survive one Pod failing.

---

**2. What does `maxReplicas: 5` mean?**

The HPA will never scale the Deployment above five Pods, however high CPU usage goes. This caps resource use and cost, and protects the cluster from one workload taking all its capacity.

---

**3. What does `averageUtilization: 50` mean?**

The HPA tries to keep average CPU usage across all the Pods at about 50% of their requested CPU. Each Pod requests 100m, so the target is about 50m per Pod. If the average rises above that, the HPA adds Pods. If it falls well below, the HPA removes Pods, staying between 2 and 5.

---

**4. Why should only one HPA manage a Deployment at a time?**

Each HPA independently sets the Deployment’s replica count. Two HPAs would compete, each overriding the other’s decisions, so the Pod count would keep jumping up and down unpredictably. One HPA per Deployment gives a single, consistent decision-maker.

---

# Task 6 — Optional: Generate CPU Load and Observe Scaling

## Goal

Generate CPU load in one NGINX Pod and observe how HPA responds.

### Evidence

If you skip this task, write “Not completed — optional” and leave the remaining Task 6 placeholders blank.

Task status: Add your status here.

#### Screenshot 11 — Terminal showing the CPU-load command running inside the selected Pod

Add your screenshot here if completed.

---

#### Screenshot 12 — Output of `kubectl get hpa` and `kubectl get pods` while HPA evaluates or changes the replica count

Add your screenshot here if completed.

---

### Notes

Complete these answers only if you perform the optional test.

**1. Did the HPA change the number of Pods? Explain what you observed.**

Add your answer here.

---

**2. If scaling did not happen, what are two possible reasons?**

Add your answer here, or write “Not applicable — scaling occurred.”

---

**3. Why is automatic scale-down useful after traffic decreases?**

Add your answer here.

---

# LinkedIn Requirement

### Evidence

#### Screenshot 13 — Published LinkedIn post showing your name, the required explanation, and the attached Kubernetes screenshot

Add your screenshot here.

#### Linkedin Post Link

Add your link here

---

# Submission Instructions

- Include required Screenshots 1–10 and Screenshot 13.
- Include Screenshots 11–12 only if you complete Task 6.
- Full Name must be visible in required screenshots.
- Ensure screenshots are clear and readable.
- Include the completed `autohealing-deployment.yaml` and `hpa-deployment.yaml` files.
- Include `hpa-nginx.yaml` if you selected the declarative HPA method.
- Complete the Notes sections for Tasks 1–5 in your own words.
- Complete Task 6 notes only if you perform the optional test.
- Do not expose passwords, tokens, private keys, kubeconfig files, or account IDs.

---

# Completion Checklist

- [ ] Checked the active context and existing cluster resources
- [ ] Created the autoscaling working directory (Screenshot 1)
- [ ] Created `autohealing-deployment.yaml` (Screenshot 2)
- [ ] Confirmed two auto-healing Pods were running (Screenshot 3)
- [ ] Deleted one managed Pod (Screenshot 4)
- [ ] Confirmed that the Pod was automatically replaced (Screenshot 5)
- [ ] Created `hpa-deployment.yaml` with CPU requests and limits (Screenshot 6)
- [ ] Verified `nginx-deployment` and its running Pods (Screenshot 7)
- [ ] Verified Metrics Server and confirmed Pod metrics are available (Screenshot 8)
- [ ] Created one HPA using the selected method (Screenshot 9)
- [ ] Verified minimum replicas of 2, maximum replicas of 5, and a CPU target of 50% (Screenshot 10)
- [ ] Completed Notes for Tasks 1–5
- [ ] Completed Task 6 evidence and notes, or marked it “Not completed — optional”
- [ ] Stopped the CPU-load command and exited the container shell, if Task 6 was completed
- [ ] Published the LinkedIn post with the required explanation and attached screenshot (Screenshot 13)
- [ ] Included the required manifest files
- [ ] No sensitive data exposed

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme  
- 🎓 University: https://university.pravinmishra.com?utm_source=github&utm_medium=readme  
- 💬 Discord Community: https://discord.pravinmishra.com?utm_source=github&utm_medium=readme  
- 📝 Blog: https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) — Agentic AI Track.*
