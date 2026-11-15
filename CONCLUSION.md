# Lab 05 — Conclusion

<!--
Read this before you write.

This file is graded by your instructor, by hand, and it is worth 6 of the 15 points.

- Type it yourself. Do not use a chatbot, an AI assistant, an AI autocomplete or a "rewrite my
  text" tool for any part of it, not even for grammar. English that is plain and a little
  clumsy but yours is better than polished text that is not.
- Use your own numbers. Every number you quote must be one your own notebook printed. Copy
  the number exactly as it appeared. A number your notebook did not print is marked as wrong.
- Answer in your own words, about 80-200 words per question. A short answer that uses your
  numbers beats a long answer that could have been written by anyone.
- Delete these comment lines if you like. Keep the three headings and the last line.
-->

## 1. Which layer changed your results the most?

Give the numbers before -> after for that layer (correct / wrong / no answer, out of 20) and say
whether it changed what the agent *can do* or only what it *admits*. If no layer changed the
number of correct answers, say so and explain what the layers did change instead.

Layer 1 changed my results the most. Out of 24 questions, correct went from 16 to 18. Wrong went from 4 to 6. No answer went from 4 to 0. Before layer 1, the program stopped at the first tool error. So the four alias questions (q21 to q24) had no answer. With layer 1 the error goes back to the model, and the model can try again. q23 and q24 became correct. q21 and q22 became wrong. So layer 1 changed what the agent can do, because now it can use list_regions after an error. But I did not open the traces of q21 and q22, so I do not know why they went wrong. Layer 2 changed nothing (18 / 6 / 0). Layer 3 only changed what the agent admits: wrong went from 6 to 4 and no answer from 0 to 2. Correct stayed 18.

## 2. One failure the full harness did not fix

Pick one question that the full harness (layer 3) still gets wrong or refuses. Give its id
(q01 ... q20). Open its trace in the notebook: at which step did it go wrong, and what did
the model do instead? Name one thing you would add to the harness, and what it would cost
(extra model calls, tokens, time, or a new way to fail).

I picked q08: the region with the lowest average wage in 2024. With layer 3 the answer is still wrong. In the trace, the model called compare_regions. Then it wrote only "Jambyl" and no ANSWER line. The harness asked for the line. The model wrote ANSWER: Jambyl. The harness said OK, because Jambyl is a real name in the tool result. But the answer is wrong. So the model went wrong when it chose the row. The harness only checks that the value is in a tool result. I do not know why the model chose Jambyl, because I saw only the start of the list. I would add simple code for rank questions: it takes the last row of the list for "lowest" and compares it with the answer. The cost is more code, and it works only for rank questions. A bad parse would be a new way to fail.

## 3. Workflow or agent for the monthly ranking?

Dala Insight wants to publish, every month, "which region is highest and which is lowest on one
indicator in one year". Would you ship the fixed workflow or the agent? Use the accuracy, the
number of model calls, the tokens and the time you measured in Part 1. Then say in one sentence
what kind of question would make you choose the other one.

I would use the workflow. In Part 1, on the 15 fixed questions, it got 13/15 correct. The bare agent got 9/15. The workflow used 15 model calls and the agent used 26. The workflow used 3852 tokens and the agent used 19835. The workflow needed 15.6 seconds and the agent needed 48.4 seconds. The workflow failed only q21 and q22, and those are alias questions, not ranking questions. The bare agent got two rank questions wrong (q08 and q11). A monthly ranking has the same shape every time, so the workflow is good for it. It is cheaper and faster. I would choose the agent for a question like a percent change between two years, because the workflow cannot do that.

---

I wrote this file myself, without any AI tool.
