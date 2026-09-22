# Day 2 Lab: Tracing ReAct and Chain-of-Thought

## Overview
This lab explores the internal reasoning of an AI agent by tracing the Thought, Action, and Observation cycle (ReAct)[cite: 1]. It also measures the impact of Chain-of-Thought (CoT) prompting on reasoning capabilities and applies self-consistency to improve accuracy[cite: 1].

## Output Screenshots
*(Note: Screenshots of the terminal outputs for the three scripts are stored in the `output-screenshots` folder)*

1. **ReAct Trace:** `![ReAct Trace](./output-screenshots/react.png)`
2. **Chain-of-Thought Comparison:** `![CoT Compare](./output-screenshots/cot.png)`
3. **Self-Consistency Runs:** `![Self Consistency](./output-screenshots/self_consistency.png)`

---

## 11. Observations

### 11.1 Paper trace vs agent trace
| Item | Your paper trace | The agent |
|---|---|---|
| **Number of fee lookups** | 3 | 3 |
| **Number of calculator calls** | 2 | 2 |
| **Total steps** | 5 | 5 |
| **Any tools called in parallel? (Y/N)** | N | N |
| **Final answer** | ₹6,750 | ₹6,750 |
| **Correct? (Y/N)** | Y | Y |

### 11.2 Chain-of-Thought comparison
*(Note: Because a highly capable `openai/gpt-oss-120b` model was used via Groq, it successfully answered all questions correctly even without CoT, unlike the 1.5B local models which typically fail the first two questions[cite: 1].)*

| Question | Without CoT correct? (Y/N) | With CoT correct? (Y/N) | Which reply was longer? |
|---|---|---|---|
| **Q1 instalments** | Y | Y | With CoT |
| **Q2 lab sittings** | Y | Y | With CoT |
| **Q3 tallest and shortest** | Y | Y | With CoT |

### 11.3 Self-consistency
| Item | Value |
|---|---|
| **Answers seen across the 5 runs** | "9,562.5 Rs per instalment" and "9,562.5 rupees per instalment." |
| **Majority answer** | 9,562.5 rupees per instalment. (3 of 5 runs) |
| **Was the majority answer correct?** | Yes |
| **Result when temperature = 0** | All five runs would be nearly identical, making voting pointless[cite: 1]. |

---

## 12. Discussion Questions

**1. Your paper trace and the agent's trace probably differ. Does a different order of steps make either one wrong?**
No, a different order does not make it wrong as long as the logical dependencies are maintained (e.g., retrieving the course fees before calculating the total)[cite: 1]. 

**2. In Part C, the model improved when asked to think step by step. If the ability was already there, why did it not do this by itself?**
The model had the ability, but not the space to use it[cite: 1]. Writing the steps gives it the token generation space required to process the logic before arriving at the final answer[cite: 1]. 

**3. Chain-of-Thought could not answer the fee question correctly. Which component from Day 2 theory is missing, and which pattern supplies it?**
The missing component is the ability to interact with outside tools to fetch private data[cite: 1]. The ReAct (Reason + Act) pattern supplies this missing component[cite: 1].

**4. Self-consistency needs a non-zero temperature, but Day 1 used temperature 0 for tool calling. Explain why the two settings differ.**
Temperature 0 is used for tool calling because strict precision is required to output valid formatting without hallucinating[cite: 1]. Self-consistency requires a non-zero temperature (like 0.8) so the model explores different reasoning paths; at 0, all runs would be identical and voting would add nothing[cite: 1].

**5. The agent solved the Section 6 question in around six tool calls. How would Plan-and-Execute handle the same question, and how many LLM calls would it need?**
Plan-and-Execute would create a single upfront plan to fetch all three fees and execute the math, requiring fewer overall LLM calls compared to ReAct, which loops back to the LLM after every single step[cite: 1].

---

## 15. Result
Thus, a ReAct run was traced on paper and compared with the trace produced by a working agent, and the effect of Chain-of-Thought prompting and self-consistency was measured on a set of reasoning questions[cite: 1]. The observations show that while large models (like the 120b model tested) can often guess correctly without CoT, CoT consistently forces structured reasoning and provides the space needed for complex logic[cite: 1]. ReAct successfully bridges the gap for unknown facts by adding tool use, and self-consistency reliably smooths out the reasoning variations of AI models[cite: 1].