# AI-Driven Job Matching Automation

This project automates the process of finding and evaluating relevant job opportunities. It runs every 8 hours on an Azure Virtual Machine, analyzes job postings using an LLM, and sends Telegram notifications whenever a suitable position is found.

## How it works

- **Scheduled Execution:** The workflow is automatically triggered every 8 hours using n8n.

- **Job Scraping:** An Apify actor collects recent job postings from LinkedIn.

- **Initial Filtering:** A JavaScript node filters the collected jobs based on predefined criteria such as location, job type, technical keywords, and a blacklist for unrelated positions (e.g. Sales or First-Level Support). This reduces unnecessary LLM requests and API costs.

- **AI-Based Job Evaluation:** Relevant job postings are sent to **Llama 3.3 (via the Groq API)**. The model compares each position with a predefined career profile and returns a match score, category, recommendation, and a short explanation.

- **Duplicate Detection:** Before storing or sending a notification, **PostgreSQL** checks whether the job has already been processed using unique constraints. This prevents duplicate notifications and repeated LLM evaluations.

- **Telegram Notification:** If a job reaches the required match score, a Telegram message is sent containing the evaluation results and a direct link to the job posting.

## Tech Stack

- **Workflow Automation:** n8n
- **Infrastructure:** Azure Virtual Machine (Ubuntu)
- **Programming Language:** JavaScript
- **LLM:** Llama 3.3 (70B) via Groq API
- **Database:** PostgreSQL
- **Notifications:** Telegram Bot API

## Monitoring

A separate n8n workflow uses the **Error Trigger** node to monitor the system. If an unexpected error occurs, a Telegram alert is sent containing the error message, workflow ID, and the last executed node.

## Repository Contents

- **job_matcher.json** – Main workflow including job filtering, AI evaluation, database integration, and Telegram notifications.
- **error_handler.json** – Error monitoring workflow for automatic failure notifications.
