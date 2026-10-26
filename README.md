# AI Support Message Classifier

## Problem
Support messages arrive in Mongolian and English and are sorted by hand, which is slow and inconsistent.

## What it does
- Receives a message through a webhook (Flask, JSON in/out)
- Classifies it with the Gemini API into category, urgency, and a one-line summary
- Validates the model's output before saving (allowed values, required fields)
- Stores results in SQLite; failed items are marked and logged instead of crashing the run
- Generates a summary report from SQL results
- Includes a small agent that looks up order status through tool calling

## Testing
- **Tuned set:** 16 labeled messages (Mongolian and English, plus empty input, gibberish, and a prompt-injection attempt). Accuracy rose from 69% to 94% (15/16) after improving the prompt.
- **Fresh set:** 10 new messages not used for tuning, scored 10/10.
- **Remaining miss:** "I was charged for an order that never arrived" was labeled billing and the model said shipping. Both answers are defensible.
- **Caveat:** I tuned the prompt on the first set, so 94% is optimistic, and both sets are small. A production system would need hundreds of real messages.
- **Agent checks:** an unknown order ID returned "not found" without inventing a status, a refund request was declined with no promise made, and a valid order returned the correct status and date.
## Safety
- API key stored in Colab Secrets, never in code
- Webhook input validated and length-capped
- Prompt treats customer text as data; hardening reduces but does not eliminate injection risk
- Agent cannot promise refunds; a real system would need human approval for risky actions

## Impact (estimate)
Sorting 100 messages by hand takes about 30 minutes; this takes about 2 minutes.

## Limitations
- Small test sets (16 + 10 messages)
- Webhook runs only inside the notebook
- Free-tier rate limits

## Tools
Python, SQLite, Flask, Gemini API, Google Colab
