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
- 16 labeled test messages, including Mongolian, empty input, gibberish, and a prompt-injection attempt
- Accuracy: first prompt 69% → improved prompt XX%
- Agent checks: unknown order ID, refund request, off-topic question

## Safety
- API key stored in Colab Secrets, never in code
- Webhook input validated and length-capped
- Prompt treats customer text as data; hardening reduces but does not eliminate injection risk
- Agent cannot promise refunds; a real system would need human approval for risky actions

## Impact (estimate)
Sorting 100 messages by hand takes about 30 minutes; this takes about 2 minutes.
69% → 94% on the tuned set, 10/10 on the fresh set, and the small-sample caveat

## Limitations
- Small test set (16 messages)
- Webhook runs only inside the notebook
- Free-tier rate limits

## Tools
Python, SQLite, Flask, Gemini API, Google Colab
