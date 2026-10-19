# CrewAI Adversarial Testing Template

This repository contains the CrewAI configuration for an autonomous engineering team with a built-in **Adversarial Testing Gate** (LLM-as-a-judge). 

This setup was featured in my LinkedIn article: [Why Enterprise AI Needs Adversarial Testing](#).

## The Architecture
The team consists of 5 agents:
1.  **Engineering Lead:** Designs the architecture.
2.  **Backend Engineer:** Writes the Python backend.
3.  **Frontend Engineer:** Writes the Gradio UI.
4.  **Test Engineer:** Writes unit tests and validates syntax.
5.  **Adversarial Reviewer:** The Red Team. It independently audits the code, finds security flaws, and outputs a `FAIL` verdict to block deployment to production.

## How to use these files
1. Copy `agents.yaml` and `tasks.yaml` into your `config/` directory.
2. In your `crew.py`, register the `adversarial_reviewer` agent and the `adversarial_task`.
3. To enforce the gate, your Azure DevOps / JIRA -> CrewAI -> W&B webhook (or main script) should read the `output/adversarial_review.md` file created by the Adversarial Reviewer Agent. If it contains `ADVERSARIAL_REVIEW: FAIL`, halt the pipeline and route the Azure DevOps or JIRA ticket to a `Human-in-the-Loop` state.
