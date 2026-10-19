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

6.  ## 📄 Example Output
Check out [`adversarial_review.md`](./adversarial_review.md) to see exactly what the Red Team agent produces. It highlights a real "infinite money glitch" (a rounding exploit) it caught in a stock trading app simulation that had passed all standard unit tests

## How to use these files
1. Copy `agents.yaml` and `tasks.yaml` into your CrewAI project's `config/` directory.
2. In your `crew.py`, register the `adversarial_reviewer` agent and the `adversarial_task`.
3. To enforce the gate: Your orchestrator (e.g., a Webhook Server) should trigger the Crew, and then read the `output/adversarial_review.md` file created by the Adversarial Reviewer Agent. 
4. **The Fail-Safe:** If the report contains `ADVERSARIAL_REVIEW: FAIL`, the orchestrator should halt the pipeline and route the Azure DevOps / JIRA ticket to a `Human-in-the-Loop` state, attaching the report for review.

## Framework Agnosticism
While this implementation uses CrewAI, the core pattern—an independent LLM acting as a hostile judge—is framework-agnostic. You can apply this exact same prompt architecture and gating logic using LangGraph, AutoGen, or a custom orchestration script with minor syntax modifications.
