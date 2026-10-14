# CrewAI Build Report

**Name:** Bushra Afrin Mohammed  
**Topic I used:** How AI agents can improve supply chain risk management

## 1. What I changed

I used the provided CrewAI starter kit and made one meaningful change to `crewai-kit/main.py`. Specifically, I replaced the **Technical Writer** agent's `backstory`. In the original version, the writer was simply asked to turn technical material into clear, jargon-free prose. In my changed version, I described the writer as a supply chain risk communication specialist who explains technical topics to **small business owners**. I asked it to use plain language, practical examples, clear business risks, and actionable recommendations, while avoiding unnecessary jargon.

I chose this change because smaller companies may not have dedicated supply chain analysts. I wanted the same research topic to be explained in a way that a business owner could connect to everyday decisions, such as avoiding stockouts, planning inventory, and choosing backup suppliers. I kept the same topic, research task, writing task, editing task, and three-agent sequence so I could observe the effect of changing the writer's instructions rather than redesigning the whole project.

## 2. How CrewAI works

CrewAI organizes an AI workflow as a team of specialized **agents**. An agent is given a role, a goal, and a backstory that guide how it approaches the work. In my project, the three agents were the **Research Analyst**, **Technical Writer**, and **Editor**. All three used an LLM accessed through Groq.

A **task** describes what an agent must do and what output is expected. My Research Analyst's task was to prepare five key facts and two open questions on the topic. The Technical Writer's task was to turn that brief into an approximately 250-word explanation. The Editor's task was to improve the explanation and flag claims that need sources.

The **crew** connects these agents and tasks. In `main.py`, `Process.sequential` makes the agents work in order: the research brief informs the writer, and the writer's draft informs the editor. The final result is written to `output/explainer.md`.

My modification affected the writer's **backstory**, which is part of the instructions given to that agent. It did not change the code that passes work between agents. This showed me that agent behavior can be adjusted by changing a role's instructions, without adding more agents or changing the entire workflow. However, because the model generates text probabilistically, not every difference between two runs can be attributed only to my edit.

## 3. Evidence

I ran the same command for both versions:

`uv run python main.py "how AI agents can improve supply chain risk management"`

The evidence is saved in `run1_original.txt`, `explainer_run1.md`, `run2_changed.txt`, and `explainer_run2.md`.

In the original final explainer, the writing mostly presented broad technical capabilities, such as disruption forecasting, safety-stock adjustment, supplier risk scoring, and scenario planning. In the changed run, the Technical Writer gave a more concrete example of a **small retailer** who could use weather and port information to order extra stock before a shipment delay. The changed final output also included practical discussion of supplier alternatives and business decisions. One unexpected result was that the Editor produced a longer, more report-like final answer instead of just a short explainer. This means the changed run was not a perfectly controlled comparison of writing style.

During the changed run, CrewAI logged `Invalid response from LLM call - None or empty` while the Technical Writer was working. The framework retried, the writer continued, and the crew completed successfully. I kept this in the terminal log instead of hiding it because it shows a real limitation of relying on model responses. I did not need to change the code again to complete the run.

## 4. Choosing a framework

I would choose **CrewAI** for an early prototype of a multi-step supply chain analysis workflow because the work naturally divides into roles. One agent can summarize possible disruptions, a second can explain what those risks mean for a small business, and a third can review the explanation for missing evidence. The roles and tasks are easy to understand in the code, and CrewAI handles the sequential handoff. That made it convenient to test how one agent's instructions influenced the final output.

I would also consider **LangGraph**, another framework covered in the workshop. Its main strength is explicit control over workflow steps, branches, and state. For a production supply chain system, that could be useful when a possible disruption needs verification, when a low-confidence recommendation should be routed for review, or when a human must approve an action before it happens. Those steps would take more design effort in LangGraph than this simple sequential CrewAI example, but the explicit control could be worthwhile for higher-stakes decisions.

For this assignment, CrewAI was the simpler choice because I wanted to change one role and compare the outputs. For a real operational system that might reroute a shipment or change inventory orders, I would prioritize verified data, error handling, and human approval rather than allowing an LLM to act on its own.

## 5. Verification and AI use

I verified that the installation and connection worked by running `check_setup.py`, which ended with `CrewAI kit OK`. I checked that both executions reached `Crew Execution Completed`, examined the terminal logs, and compared the two generated explainers. I also checked that the modified writer's language was more oriented toward small businesses.

I **did not independently verify the numerical performance claims** produced by the agents. This starter kit has no web-search tool, so its apparent references to studies, named organizations, and percentages should not be treated as confirmed evidence. Some claims were marked `[source needed]`, but even claims with a source-like label still require checking against the original publication before being used in a real report.

I used **ChatGPT** to guide setup, troubleshoot Python syntax, revise the Technical Writer's backstory, organize my saved outputs, and help draft this report. I checked that the changed code ran on my computer and compared the actual outputs to make sure this report described what happened rather than claiming results that were never tested.
