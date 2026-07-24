# Assignment 6 — Build an AI-Assisted Linux Health Check (AI-Assisted Linux Incident Triage)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will build a read-only Bash triage script that checks the health of your Ubuntu server and Nginx application, connect it to Claude Code as a reusable `/linux-triage` skill, simulate a controlled Nginx incident, use the skill to gather and analyze evidence, recover the service manually, and verify recovery. The workflow follows the Agentic Loop: Gather → Analyze → Human Act → Verify.

---

# Task 1 — Confirm the Healthy Baseline and Create the Workspace

## Goal

Confirm that Nginx and the React application are healthy before building the automation.

### Evidence

#### Screenshot 1 — Output of `systemctl is-active nginx`, `ss -ltn | grep ':80'`, and `curl -I http://localhost`

Add your screenshot here.

---

#### Screenshot 2 — Output of `pwd` and `find . -maxdepth 4 -type d | sort` showing the workspace folder structure

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What proves that Nginx is running?**

The command systemctl is-active nginx returned active, which proves that the Nginx service is running.

---

**2. What proves that the server is listening for HTTP traffic?**

The command ss -ltn | grep ':80' showed that port 80 was listening, and curl -I http://localhost returned HTTP 200 OK, proving that the server was accepting HTTP traffic.

---

**3. Why must you capture a healthy baseline before simulating an incident?**

Capturing a healthy baseline lets you confirm that the server is working correctly before creating a problem. This makes it easier to compare the healthy and failed states and verify that the recovery was successful.

---

# Task 2 — Create Project Context and Safety Rules in CLAUDE.md

## Goal

Tell Claude exactly what this project does and what it is not allowed to do.

### Evidence

#### Screenshot 3 — CLAUDE.md open in VS Code showing all four sections (Project Overview, Incident Workflow, Safety Rules, Output Rules)

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. Why should Claude receive project-specific operational rules?**

Project-specific operational rules tell Claude what it is allowed and not allowed to do. This helps it follow the correct workflow and keeps the server safe.

---

**2. Why is the human required to execute the recovery command?**

The human must execute the recovery command to make sure the action is reviewed and approved before making changes to the server.

---

**3. Which rule prevents Claude from making an unsupported diagnosis?**

The rule "Do not claim a root cause unless the report contains supporting evidence." prevents Claude from making an unsupported diagnosis.

---

# Task 3 — Use Agentic AI to Plan Before Writing the Script

## Goal

Use Claude Code to inspect the environment and produce a read-only plan before creating any Bash code.

### Evidence

#### Screenshot 4 — Claude Code showing the five-check plan and read-only inspection results

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. Which part of this task represents the Gather phase?**

The Gather phase is when Claude inspected the Ubuntu server using read-only commands and collected information about the server's health.

---

**2. Did Claude follow the instruction not to create files? How did you verify this?**

Yes. Claude followed the instruction and did not create or edit any files. I verified this because it only inspected the server and showed a health check plan without making any changes.

---

**3. Why is planning before coding useful in DevOps automation?**

Planning before coding helps you understand what needs to be checked, reduces mistakes, and makes it easier to build a reliable automation script.

---

# Task 4 — Build the Linux Triage Bash Script

## Goal

Create one Bash script that gathers consistent Linux and Nginx health evidence.

### Evidence

#### Screenshot 5 — Top section of `linux-triage.sh` showing variables, thresholds, and the checks array

Add your screenshot here.

---

#### Screenshot 6 — Middle section showing check functions and conditionals

Add your screenshot here.

---

#### Screenshot 7 — Bottom section showing the loop, summary function, and exit behavior

Add your screenshot here.

---

#### Screenshot 8 — Output of `bash -n scripts/linux-triage.sh` (no syntax errors) and `ls -l scripts/linux-triage.sh` showing executable permission

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What is stored in the checks array?**

The checks array stores the names of all the health check functions, such as checking the Nginx service, port 80, HTTP response, disk usage, and memory.

---

**2. How does the `for` loop use that array?**

The for loop goes through each function in the checks array and runs them one by one. This makes sure every health check is performed.

---

**3. Why are the health checks separated into functions?**

The health checks are separated into functions to keep the script organized, easier to read, and easier to update if changes are needed later.

---

**4. What is the purpose of `$(...)` in this script?**

$(...) is used to run a command and save its output into a variable so the script can use the result later.

---

**5. Why does the script use different exit codes for HEALTHY, WARN, and FAIL?**

Different exit codes make it easy to know the server's condition. 0 means everything is healthy, 1 means there is a warning, and 2 means there is a failure that needs attention.

---

# Task 5 — Run and Understand the Healthy-State Report

## Goal

Run the Bash script against the healthy server and verify that it creates a report.

### Evidence

#### Screenshot 9 — Output of `./scripts/linux-triage.sh` showing your Full Name and all five check results

Add your screenshot here.

---

#### Screenshot 10 — Output showing the captured exit code and final summary

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What is the overall status of your healthy baseline?**

The overall status of my healthy baseline was HEALTHY because all the health checks passed successfully.

---

**2. Which exact Linux evidence proves the application is serving traffic?**

The evidence was that the local HTTP check returned status 200, Nginx was active, and port 80 was listening. This showed that the application was serving traffic.

---

**3. Did your script return exit code 0 or 1? Explain why.**

My script returned exit code 0 because there were no warnings or failures. All the health checks passed, so the server was healthy.

---

**4. What is the difference between a warning and a failure in this script?**

A warning means the server is still working, but something needs attention, such as high disk usage or low memory. A failure means an important check failed, like Nginx not running or the website not responding, and it needs to be fixed.

---

# Task 6 — Create and Run the /linux-triage Skill

## Goal

Turn the Bash script into a reusable, manually invoked Agentic AI workflow.

### Evidence

#### Screenshot 11 — `SKILL.md` showing the frontmatter, allowed tool restrictions, and safety rules

Add your screenshot here.

---

#### Screenshot 12 — `/linux-triage` output for the healthy server

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. Why does this skill have Bash, Read, and Grep, but not Write?**

The skill uses Bash to run the health check script, Read to read the report, and Grep to find important information. It does not have Write permission so it cannot change files or make changes to the server.

---

**2. Why is `disable-model-invocation: true` useful for this skill?**

It makes sure the skill only follows the instructions in the skill file. This keeps the workflow safe, consistent, and focused on analyzing the report instead of doing extra actions.

---

**3. What part is performed by Bash, and what part is performed by Claude?**

Bash collects the server information and creates the health report. Claude reads the report, explains the results, finds the possible cause, and suggests a safe recovery command.

---

**4. Why is this better than asking Claude "Is my server healthy?" without giving it evidence?**

It is better because Claude uses real evidence from the Bash report instead of guessing. This makes the analysis more accurate and trustworthy.

---

# Task 7 — Simulate an Nginx Incident and Let the Skill Diagnose It

## Goal

Create a controlled service failure, gather evidence through Bash, and let Claude analyze the evidence without taking recovery action.

### Evidence

#### Screenshot 13 — Output showing Nginx is inactive and the HTTP request fails

Add your screenshot here.

---

#### Screenshot 14 — `/linux-triage` output showing failed evidence, most likely cause, and a suggested recovery command

Add your screenshot here.

---

#### Screenshot 15 — `incident-failure-report.txt` showing the failed checks and your Full Name

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. Which three checks failed?**

The three failed checks were:

1. Nginx service is not active.
2. Port 80 is not listening.
3. Local HTTP check returned status 000.

---

**2. What evidence supports the conclusion that Nginx is unavailable?**

The report showed that the Nginx service was inactive, port 80 was not listening, and the HTTP request returned status 000, which means the website could not be reached.

---

**3. Did Claude execute the recovery command? Why is that important?**

No. Claude only suggested the recovery command. This is important because the engineer should review and approve any changes before running them.

---

**4. Which phase of the Agentic Loop is represented by the Bash report?**

The Bash report represents the Gather phase because it collects information about the server.

---

**5. Which phase is represented by Claude's explanation?**

Claude's explanation represents the Analyze phase because it reads the report, explains the problem, and recommends a safe next step.

---

# Task 8 — Recover Manually, Verify Again, and Write the Incident Summary

## Goal

Recover the service as the human operator and prove that the system is healthy again.

### Evidence

#### Screenshot 16 — Output showing Nginx is active and `curl -I http://localhost` returns 200 OK

Add your screenshot here.

---

#### Screenshot 17 — Second `/linux-triage` output showing successful recovery with no FAIL results

Add your screenshot here.

---

#### Screenshot 18 — Output of `ls -lah reports` showing both `incident-failure-report.txt` and `recovery-report.txt`

Add your screenshot here.

---

#### Screenshot 19 — `incident-summary.md` showing all required sections and your Full Name

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What action did you execute manually?**

I manually started the Nginx service by running:

sudo systemctl start nginx

---

**2. What evidence proves that the service recovered?**

The evidence showed that Nginx was active, port 80 was listening, the local HTTP check returned 200 OK, and the final report showed HEALTHY with no failed checks.

---

**3. Why is the second triage run necessary?**

The second triage run confirms that the problem has been fixed and that the server is working properly after the recovery.
---

**4. What could go wrong if an AI agent automatically restarted every failed service?**

It could restart the wrong service, make the problem worse, or hide the real cause of the issue without checking it first.

---

**5. In one sentence, explain the difference between using AI as a chatbot and using AI in this agentic workflow.**

A chatbot answers questions, while an agentic AI follows a workflow by gathering evidence, analyzing it, and helping the engineer make safe decisions.

---

# Incident Summary

Fill in all seven sections below in your own words.

**Full Name:**  Pamela Chidinma Ezeotika  

**Date:**  23/07/2026

---

**1. Reported Symptom**

The website was not opening because the Nginx service was stopped. The local HTTP request also failed.

---

**2. Evidence Collected**

The Bash report showed three failed checks:
- Nginx service is not active.
- Port 80 is not listening.
- Local HTTP check returned status 000.

---

**3. Most Likely Cause**

Nginx was stopped. Since the service was not running, port 80 was not listening, and the website could not be reached.


---

**4. Human-Approved Recovery Action**

After reviewing the evidence, I manually restarted the Nginx service using:

sudo systemctl start nginx


---

**5. Verification**

The AI skill was allowed to collect information and explain the problem, but it was not allowed to restart Nginx. This made sure that a human reviewed and approved the recovery action before making changes.

---

**6. Safety Decision**

Claude was only allowed to analyze the system and suggest a recovery action, while the actual execution was performed manually to maintain control and prevent unintended changes.

---

**7. Agentic Loop Mapping**

Gather: Bash collected system health data 
Analyze: Claude reviewed and explained the issue 
Human Act: I manually restarted the Nginx service 
Verify: I reran the triage script to confirm recovery



---

# LinkedIn Post (Required)

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`Add your URL here`

---

#### Screenshot — Published LinkedIn post

Add your screenshot here.

---

# GitHub Repository URL

Paste the URL of your GitHub folder or repository containing the assignment files here:

`Add your URL here`

---

# Submission Instructions

- Add all required screenshots in your submission
- Full Name must be visible in required screenshots and the Bash report
- All written answers must be in your own words
- Do not expose sensitive information (keys, passwords, AWS account IDs, tokens)
- GitHub URL must be included in this document

---

# Completion Checklist

- [ ] Task 1: Healthy baseline confirmed, workspace created (Screenshots 1–2, Notes answered)
- [ ] Task 2: CLAUDE.md created with all four sections (Screenshot 3, Notes answered)
- [ ] Task 3: Five-check plan produced by Claude using read-only tools (Screenshot 4, Notes answered)
- [ ] Task 4: `linux-triage.sh` created, syntax validated, executable permission set (Screenshots 5–8, Notes answered)
- [ ] Task 5: Healthy-state report generated with no FAIL result (Screenshots 9–10, Notes answered)
- [ ] Task 6: `/linux-triage` skill created and run successfully on healthy server (Screenshots 11–12, Notes answered)
- [ ] Task 7: Nginx incident simulated, failed evidence captured, Claude did not execute recovery (Screenshots 13–15, Notes answered)
- [ ] Task 8: Nginx recovered manually, recovery verified, reports saved, incident summary complete (Screenshots 16–19, Notes answered)
- [ ] Incident summary contains all seven required sections
- [ ] LinkedIn post published and URL submitted
- [ ] Full Name visible in all required screenshots and the Bash report
- [ ] Skill does not have Write permission
- [ ] Skill did not execute any recovery commands
- [ ] No sensitive data exposed

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://pravinmishra.com/dmi  
- 🎓 DevOps for Beginners (Udemy): https://www.udemy.com/course/devops-for-beginners-docker-k8s-cloud-cicd-4-projects/  
- 🎓 Agentic AI DevOps with Claude Code: https://www.udemy.com/course/ultimate-agentic-ai-devops-with-claude-code/  
- 🎓 DevOps with Claude Code: Terraform, EKS, ArgoCD & Helm: https://www.udemy.com/course/devops-with-claude-code-terraform-eks-argocd-helm/  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*