# **Customer Support Chatbot with Amazon Bedrock**

This is my Udacity project for the Future AWS Agent Engineer program. The chatbot routes customer messages into three paths: bug reports, platform questions, and other requests. The solution is built [...]

## **Project summary**

The chatbot classifies each incoming customer message into exactly one of three categories:

- `BUG_REPORT`
- `PLATFORM_QUESTION`
- `ANYTHING_ELSE`

The classifier output is used only for routing. The original customer message is passed to the selected branch for handling.

- The **bug report** branch collects issue details from the user.
- The **platform question** branch answers only from the embedded FAQ.
- The **anything else** branch redirects the user to human support.

## **Architecture**

```text
FlowInputNode
  |
  v
ClassifierPrompt
  |
  v
RouteByCategory
  |
  +-- BUG_REPORT ---------> BugReportPrompt ---------> BugReportOutput
  |
  +-- PLATFORM_QUESTION --> PlatformFAQPrompt -------> PlatformFAQOutput
  |
  +-- ANYTHING_ELSE -----> OtherRequestPrompt -------> OtherRequestOutput
```

## **Repository structure**

README.md — project overview and setup notes

system_prompt.txt — system instructions used for the chatbot

flow-tests.json — sample test prompts and expected behavior

evaluation_dataset.jsonl — evaluation test set

submission/ — screenshots, notes, and final submission evidence

prompts/ — saved copies of classifier and branch prompts

chat.py — optional local helper script or transcript evidence if used

## **Flow behavior**

1. **Classifier**

The classifier examines the customer message and returns exactly one label:

BUG_REPORT
PLATFORM_QUESTION
ANYTHING_ELSE
It must output only the label and nothing else.

2. **Bug report path**

The bug report branch is designed to collect the following information:

description
steps to reproduce
environment details
The prompt asks for only one missing item at a time and confirms completion once all required details are available.

3. **Platform question path**

The platform question branch answers the user only if the answer is clearly covered in the embedded FAQ.

If the answer is covered, the chatbot returns a short direct response.

If the answer is not covered, it responds with:
Please contact our human customer support team at 1-800-555-0199 (available Monday through Friday) for further assistance.

4. **Other request path**

The other-request branch handles unrelated or unsupported requests and responds with the same human-support escalation message.

## **Example test prompts**

**Covered FAQ question**

Can I change or cancel my order after placing it?
Expected behavior: returns a relevant answer from the FAQ.

**Uncovered platform question**

Do you offer gift wrapping for orders?
Expected behavior: directs the user to the support phone number.

**Other request**

Can you write a happy birthday message to my friend?
Expected behavior: routes to the separate other-request path and directs the user to the support phone number.

**Bug report**

My app crashes when I try to log in.
Expected behavior: routes to the bug report branch and asks for the next missing bug detail.

## **Evaluation**

I tested the chatbot with a mix of:

bug report prompts
covered FAQ questions
uncovered FAQ questions
unrelated requests
The routing behavior worked well overall. overed FAQ questions usually returned relevant answers, uncovered questions were redirected to support, and unrelated requests followed the separate fallback path.

## **Observation**

The flow routing performed well overall with the evaluation score of 0.82, but further enhancement might be needed (stricter system prompt for bug report cases). Covered FAQ questions generally returned relevant answers, uncovered platform questions were redirected to the support phone number, and unrelated requests followed the separate fallback path. Multi-turn bug report collection was designed correctly, although follow-up state handling was less consistent during testing.

## **Submission evidence**

The submission includes screenshots showing:

the full flow canvas
the classifier prompt
the routing conditions
the FAQ prompt with embedded FAQ content
a covered FAQ response
an uncovered FAQ response
an other-request response

## **Notes**

The FAQ content is embedded directly in the FAQ prompt node.
The classifier label is used only for routing.
The original customer message is passed into the selected specialist branch.
All testing and setup were completed in us-east-1.

## **Cleanup**

After testing, delete any unused flow versions and related AWS resources if needed.


This repository does not include private AWS account details, screenshots, or personal identifiers. Those should only be added in the final submission package after testing in your own AWS account.
