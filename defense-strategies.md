## Defense Strategies Against Prompt Injection Attacks

This document outlines defense strategies to mitigate Direct, Indirect, and Nested Prompt Injection attacks when deploying LLMs in production or research environments.

## 1. General Defensive Principles

- Input Sanitization: Strip out or validate user inputs to avoid malicious prompt constructs (e.g., "Ignore previous instructions").
- Context Isolation: Avoid blending user input directly into system prompts or instructions.
- Layered Prompt Construction: Separate user input from system logic using structured formats (e.g., JSON schemas).


## 2. Direct Prompt Injection Mitigations


Risk: "Ignore previous instructions"
Strategy: Use prompt guards or repetition to reinforce system role at each turn.

Risk: Unfiltered commands (e.g., reverse shell)
Strategy: Implement keyword-based rejection or output classifiers.

Risk: Policy override attempts
Strategy: Use few-shot examples to reinforce ethical boundaries.

Example (Python):
---------------------
def safe_prompt(user_input):
    system_prompt = "You are a helpful assistant. Do not execute harmful or unethical instructions."
    return f"{system_prompt}\nUser: {user_input}"
---------------------


## 3. Indirect Injection Mitigations

Risk: Injection via file metadata, JSON fields, etc.
Strategy: Sanitize or contextually quarantine untrusted sources before LLM parsing.

Risk: Re-prompting based on external documents
Strategy: Use embeddings or RAG with filtering layers to avoid injecting arbitrary text.

Risk: Prompt reflection attacks
Strategy: Implement pre-processing to flag commands embedded in summaries or translations.


## 4. Nested Injection Mitigations

Risk: "Ignore the above, but then ignore this too..."
Strategy: Limit recursion depth or token inspection logic for repeating injections.

Risk: Exploit chaining (nested logic bombs)
Strategy: Apply LLM output filters and response scoring for signs of contradiction.

Risk: Markdown/code nesting tricks
Strategy: Strip hidden markdown or invisible syntax before rendering for LLM.

## 5. Tooling & Monitoring

- LLM Output Moderation APIs: Use services like OpenAI’s Moderation API or custom toxicity classifiers.
- Token Log Inspection: Track suspicious prompts or patterns that bypass defenses.
- Audit Trails: Log all user inputs and generated outputs for forensics and red teaming review.


## 6. Testing & Validation

- Run your LLM through test suites like this repo’s prompts/ folder regularly.
- Track exploitability, success rate, and false negative rate in your detection pipeline.
- Create and maintain a red-teaming rotation to evolve with new attack vectors.


## Summary Table

Category  | Key Defense
----------|-----------------------------
Direct    | Prompt framing, context repetition, ethical examples
Indirect  | Input isolation, format validation, filtered retrieval
Nested    | Depth control, conflict detection, structured responses

──────────────────────────────────────────────

Maintained by Brayden Park – Red teaming prompts help build safer AI. Reach out with contributions or ideas!
