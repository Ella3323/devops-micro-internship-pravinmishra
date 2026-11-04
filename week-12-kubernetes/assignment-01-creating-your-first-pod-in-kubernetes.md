# Assignment 1 — Creating Your First Pod in Kubernetes

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will create an NGINX Pod using both imperative and declarative methods. You will practice creating, checking, deleting, describing, viewing logs from, and opening a shell inside a Pod.

---

# Task 1 — Set Up Your Lab Folder

## Goal

Create a clean working directory for all Pod-related files used in this lab.

### Evidence

#### Screenshot 01 — Output of `pwd` showing the current directory ending in `/k8s-labs/pods`

![alt text](screenshots/ASSIGNMENT-01-SS1.png)

---

# Task 2 — Create a Pod Imperatively

## Goal

Create an NGINX Pod directly from the command line, verify that it is running, and delete it before continuing to the declarative method.

### Evidence

#### Screenshot 02 — Output of `kubectl run nginx-pod --image=nginx` showing the creation message, and `kubectl get pods` showing `nginx-pod` with `1/1` under `READY` and `Running` under `STATUS`, before deletion

![alt text](screenshots/ASSIGNMENT-01-SS2.png)

---

#### Screenshot 03 — Output of `kubectl delete pod nginx-pod` showing the successful deletion message

![alt text](screenshots/ASSIGNMENT-01-SS3.png)

---

# Task 3 — Create a Pod Declaratively Using YAML

## Goal

Define the NGINX Pod in a YAML file, apply the manifest, and verify the resulting Pod.

### Evidence

#### Screenshot 04 — The filename `nginx-pod.yaml` and the complete YAML manifest with clear, readable indentation

![alt text](screenshots/ASSIGNMENT-01-SS4.png)

---

#### Screenshot 05 — Output of `kubectl apply -f nginx-pod.yaml` showing the successful creation message

![alt text](screenshots/ASSIGNMENT-01-SS5.png)

---

#### Screenshot 06 — Output of `kubectl get pods` showing the declarative `nginx-pod` with `1/1` under `READY` and `Running` under `STATUS`

![alt text](screenshots/ASSIGNMENT-01-SS6.png)

---

### Notes

**1. What is the difference between imperative and declarative Pod creation?**

Imperative Pod creation means telling Kubernetes exactly what to do, step by step, using direct commands. For example, kubectl run nginx-pod --image=nginx creates the Pod immediately from the command line. It’s quick and useful for testing or troubleshooting, but the configuration isn’t saved anywhere, so it’s hard to repeat, review or track changes.

Declarative Pod creation means describing what you want in a YAML manifest, such as nginx-pod.yaml, and letting Kubernetes make the cluster match it with kubectl apply -f nginx-pod.yaml. The file defines the Pod’s name, labels, container, image and port. Because the desired state is stored in a file, it can be kept in Git, reviewed by others, reused, and applied the same way every time.

In short: imperative is “do this now”, which suits quick experiments. Declarative is “this is the state I want”, which suits repeatable, version-controlled environments like production.

---

# Task 4 — Inspect and Access the Pod

## Goal

Inspect the running Pod, view its logs, and open a shell inside the NGINX container.

### Evidence

#### Screenshot 07 — Output of `kubectl describe pod nginx-pod` showing the Pod name, `app=nginx` label, `Running` status, container name, and NGINX image

![alt text](screenshots/ASSIGNMENT-01-SS7.png)

---

#### Screenshot 08 — Output of `kFilename: `08-pod-logs.png`

![alt text](screenshots/ASSIGNMENT-01-SS8.png)

If no logs appear, add a caption stating that the command completed without output.

---

#### Screenshot 09 — Container shell opened using `kubectl exec -it nginx-pod -- /bin/bash`, showing `cd /usr/share/nginx/html`, `pwd`, and `ls -la`, with the directory path and default NGINX files visible before exiting

![alt text](screenshots/ASSIGNMENT-01-SS9.png)

---

### Task 5 — Share Your First Kubernetes Pod on WhatsApp Status

**Goal**

Share your Kubernetes learning progress on WhatsApp Status, including your generated DMI leaderboard progress link.

**Steps**

1. Go to the DMI Leaderboard.
2. Find your name and select **Share your progress**.
3. Click the **WhatsApp icon**.
4. Copy the automatically generated leaderboard message containing your rank and progress link.
5. Open WhatsApp and go to **Updates**.
6. Create a new text Status.
7. Copy and paste the message below.
8. Add the generated leaderboard message in the indicated place.
9. Review and publish your Status.

**WhatsApp Status Message**

I created my first Kubernetes Pod as part of my DevOps learning journey! 🚀

Today, I deployed an NGINX Pod using both imperative kubectl commands and a declarative YAML manifest.

I also checked its status, inspected its details, viewed its logs, and accessed the container shell.

[PASTE YOUR GENERATED LEADERBOARD MESSAGE AND LINK HERE]

#DMIByPravinMishra #DevOps #Kubernetes

**Screenshots Required**



---

# Submission Instructions

- Add all required screenshots to this submission template.
- Use the specified screenshot numbers and filenames.
- Ensure all screenshots are clear and readable.
- Submit the completed `nginx-pod.yaml` manifest as a separate file.
- Explain the difference between imperative and declarative Pod creation in your own words.
- Do not expose passwords, access tokens, or private keys.

---

# Completion Checklist

- [x] Created `~/k8s-labs/pods` (Screenshot 01)
- [x] Created `nginx-pod` imperatively and confirmed it reached the `Running` state (Screenshot 02)
- [x] Deleted the imperative Pod (Screenshot 03)
- [x] Created `nginx-pod.yaml` with the required manifest (Screenshot 04)
- [x] Applied the YAML manifest (Screenshot 05)
- [x] Confirmed the declarative Pod reached the `Running` state (Screenshot 06)
- [x] Inspected the Pod using `kubectl describe` (Screenshot 07)
- [x] Viewed container logs using `kubectl logs` (Screenshot 08)
- [x] Accessed the container shell and inspected the NGINX files (Screenshot 09)
- [x] Exited the container shell
- [x] Published the WhatsApp status (Screenshot 10)
- [x] Added Screenshot 10b if the caption was published separately
- [x] Submitted all required screenshots with the correct filenames
- [x] Submitted the `nginx-pod.yaml` file
- [x] Explained the difference between imperative and declarative methods
- [x] No sensitive data exposed

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
