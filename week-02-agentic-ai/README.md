# Week 02 — AGENTIC AI

## Assignment Overview

 Setup your First Agentic AI

---

## Task 1: Install and authenticate Claude Code CLI and VS Code extension, fork and clone the course starter repository, and observe how the Agentic Loop works before any configuration is in place

**Screenshot:**

Screenshot 1 — Terminal showing claude --version with the version number visible
![Task 1](./screenshots/Assignment1%20SS1.png)

Screenshot 2 — Claude Code authenticated and showing the terminal prompt
![Task 1](./screenshots/Assignment1%20SS2.png)

Screenshot 3 — VS Code with the project open, file tree visible showing index.html, style.css, images
![Task 1](./screenshots/Assignment1%20SS3.png)

Claude's response to the first question, showing it read the files (tool calls visible)
![Task 1](./screenshots/Assignment1%20SS4a.png)

![Task 1](./screenshots/Assignment1%20SS4b.png)


Claude's response to the second question, showing it ran a command and reported the line count
![Task 1](./screenshots/Assignment1%20SS5.png)





## Task 2: Teaching Claude Your Project

**Task:** Use /init to generate a starter CLAUDE.md, customize all 5 sections with project-specific details, and demonstrate through two live tests that Claude's behavior changes based on the file.


**Screenshot:**

 Claude’s generic response before CLAUDE.md exists (project contains only index.html, style.css, images/, README.MD, privacy.html, terms.html)
![Task 2](./screenshots/Assignment2%20SS1.png)


Screenshot 2 — The auto-generated CLAUDE.md open in VS Code showing its content
![Task 2](./screenshots/Assignment2%20SS2.png)

![Task 2](./screenshots/Assignment2%20SS2b.png)

![Task 2](./screenshots/Assignment2%20SS2c.png)


Screenshot 3 — Your customized CLAUDE.md in VS Code showing all 5 sections (scroll to show the full file)
![Task 2](./screenshots/Assignment2%20SS2c.png)

Screenshot 4 — Claude's specific, detailed answer after reading CLAUDE.md (Claude mentioning S3, CloudFront and Terraform)
![Task 2](./screenshots/Assignment2%20SS4.png)

Screenshot 5 — Claude refusing or warning against adding React because of the "No JavaScript" convention defined in CLAUDE.md



Screenshot 6 — CLAUDE.md visible in your GitHub repository after pushing the commit
![Task 2](./screenshots/Assignment2%20SS5.png)

---

## Task 3: Building Your Command Center

**Task:** Create the complete .claude/skills/ folder structure with all 4 skill files, understand why each skill has different tool restrictions, and trigger /scaffold-terraform to generate infrastructure code using a single slash command.



**Screenshot:**

Screenshot 1 — VS Code sidebar showing .claude/skills/ folder with all 4 subfolders visible
![Task 3](./screenshots/Assignment3%20SS1.png)

Screenshot 2 — .claude/skills/scaffold-terraform/ open in VS Code showing both SKILL.md and template-spec.md
![Task 3](./screenshots/Assignment3%20SS2.png)

Screenshot 3 — tf-plan/SKILL.md frontmatter showing allowed-tools: Bash, Read, Grep (no Write) and disable-model-invocation: true
![Task 3](./screenshots/Assignment3%20SS3.png)

Screenshot 4 — Claude's response showing the scaffold complete with the file list
![Task 3](./screenshots/Assignment3%20SS4.png)

Screenshot 5 — VS Code sidebar showing the terraform/ folder with all generated files inside
![Task 3](./screenshots/Assignment3%20SS5.png)

Screenshot 6 — Claude's /tf-plan response showing it ran the command and analyzed the result (pass or auAssignment3 SS6ath error both count)
![Task 3](./screenshots/Assignment3%20SS6a.png)

![Task 3](./screenshots/Assignment3%20SS6b.png)

---

## Task 4: LinkedIn Post

**Post:** (https://www.linkedin.com/posts/pamela-ezeotika_dmibypravinmishra-agenticai-claudecode-ugcPost-7481239814198091776-5fUU?utm_source=share&utm_medium=member_android&rcm=ACoAAEDjJUYBrDNdac3TpJMBKPF08Q4-AeIrB8E)


![Task 4](./screenshots/Assignment%20Linkedin.png)
---



## Task 5: Building Your AI Team

**Task:**  Build all 3 subagent files, understand why each one uses a different model and tool set, run two live subagent delegations, and analyze the reports they produce.


**Screenshot:**

Screenshot 1 — VS Code sidebar showing .claude/agents/ with all 3 files
![Task 5](./screenshots/Assignment4%20SS1.png)


Screenshot 2 — security-auditor.md frontmatter showing model and tools configuration
![Task 5](./screenshots/Assignment4%20SS2.png)


Screenshot 3 — cost-optimizer.md frontmatter showing the model and tools configuration
![Task 5](./screenshots/Assignment4%20SS3.png)

Screenshot 4 — The delegation message showing Claude launched the security-auditor
![Task 5](./screenshots/Assignment4%20SS4.png)

Screenshot 5 — Security audit report output
![Task 5](./screenshots/Assignment4%20SS5a.png)

![Task 5](./screenshots/Assignment4%20SS5b.png)

Screenshot 6 — The full cost optimization report
![Task 5](./screenshots/Assignment4%20SS6.png)






## Task 6: Connecting Claude to the Outside World

**Task:** Understand why MCP exists, configure the GitHub MCP server in .mcp.json, store credentials securely in settings.local.json, verify the connection with /mcp, and run a live query that proves Claude is working with real external data — not training data.


**Screenshot:**

Screenshot 1 — GitHub token creation page showing the selected scopes (repo, read:user) — token value must NOT be visible
![Task 6](./screenshots/Assignment5%20SS1.png)


Screenshot 2 — .mcp.json open in VS Code showing the full configuration
![Task 6](./screenshots/Assignment5%20SS2.png)

Screenshot 3 — settings.local.json open in VS Code showing the env section — blur or cover the actual GitHub token value
![Task 6](./screenshots/Assignment5%20SS3.png)

Screenshot 4 — /mcp output showing github: connected
![Task 6](./screenshots/Assignment5%20SS4.png)

Screenshot 5 — Claude's response showing the GitHub MCP tool call and the retrieved README.md content.
![Task 6](./screenshots/Assignment5%20SS5.png)

![Task 6](./screenshots/Assignment5%20SSb.png)

![Task 6](./screenshots/Assignment5%50SS6.png)


## Task 7: Safety Rails for Your AI Agent

**Task:** Configure team-level permissions in settings.json, create a UserPromptSubmit hook that catches destructive intent before Claude processes the request, create a PreToolUse hook that blocks dangerous commands before they execute, and prove both hooks work through live tests.



**Screenshot:**


Screenshot 1 — .claude folder structure visible in VS Code Explorer
![Task 7](./screenshots/Assignment6%20SS1.png)

Screenshot 2 — user-prompt-guard.sh open in VS Code showing the hook script
![Task 7](./screenshots/Assignment6%20SS2.png)

Screenshot 3 — pre-tool-guard.sh open in VS Code showing the hook script
![Task 7](./screenshots/Assignment6%20SS3.png)

Screenshot 4 — post-tool-logger.sh open in VS Code showing the hook script
![Task 7](./screenshots/Assignment6%20SS4.png)

Screenshot 5 — settings.json open in VS Code showing permissions and hooks configuration
![Task 7](./screenshots/Assignment6%20SS5.png)

Screenshot 6 — UserPromptSubmit hook blocking the destructive prompt
![Task 7](./screenshots/Assignment6%20SS6.png)

Screenshot 7 — PreToolUse hook blocking terraform destroy
![Task 7](./screenshots/Assignment6%20SS7.png)

Screenshot 8 — Claude running terraform validate successfully
![Task 7](./screenshots/Assignment6%20SS9.png)

Screenshot 9 — .claude/deploy.log showing the logged command
![Task 7]


## Task 8: A Claude That Remembers


**Task:** Understand how auto-memory works and where the memory file lives, write a structured memory entry, close the session completely, and prove in a brand new session that Claude recalls the information — without being told again.

**Screenshot:**

Screenshot 1 — Memory file path shown by Claude
![Task 8](./screenshots/Assignment7%20SS1.png)

Screenshot 2 — Claude confirming the memory was saved
![Task 8](./screenshots/Assignment7%20SS2.png)

Screenshot 3 — The MEMORY.md file open in VS Code showing the saved content
![Task 8](./screenshots/Assignment7%20SS3a.png)

![Task 8](./screenshots/Assignment7%20SS3b.png)

![Task 8](./screenshots/Assignment7%20SS3c.png)

Screenshot 4 — VS Code reopened with a fresh Claude Code session showing no previous conversation
![Task 8](./screenshots/Assignment7%20SS4.png)

Screenshot 5 — Claude recalling hero section colors
![Task 8](./screenshots/Assignment7%20SS5.png)

Screenshot 6 — Claude refusing JavaScript request based on memory rule
![Task 8](./screenshots/Assignment7%20SS6.png)


## Task 9: LinkedIn Post

**Post:**  https://www.linkedin.com/posts/pamela-ezeotika_dmibypravinmishra-agenticai-claudecode-ugcPost-7481442776996454400-2ym7/?utm_source=share&utm_medium=member_android&rcm=ACoAAEDjJUYBrDNdac3TpJMBKPF08Q4-AeIrB8E


![Task 9](./screenshots/Assignment%20Linkedin2.png)


## Task 10: Week 2 Reflection Blog

**Task:** Write a short reflection blog about your Week 2 learning journey. This assignment is not about writing code. It is about thinking clearly, explaining what you learned, and identifying how your mindset changed while working with Agentic AI, Claude Code, skills, subagents, MCP, hooks, permissions, and memory.


## Task 11: LinkedIn Post 


![Task 11](./screenshots/Assignment%20Linkedin3.png)

**Post:** https://www.linkedin.com/posts/pamela-ezeotika_dmibypravinmishra-agenticai-claudecode-share-7481445688669704192-sM3Q/?utm_source=share&utm_medium=member_desktop&rcm=ACoAAEDjJUYBrDNdac3TpJMBKPF08Q4-AeIrB8E


## Task 12: BLOG POST

![Task 12

**POST:** 



## Key Learnings

<!-- 3-5 bullet points on what you learned this week -->

- 
- 
- 

---

*Part of the [DevOps Micro Internship with Agentic AI](https://www.linkedin.com/in/pravin-mishra-aws-trainer/) by Pravin Mishra — Join: https://discord.pravinmishra.com/*
