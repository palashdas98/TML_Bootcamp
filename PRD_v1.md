# Product Requirements Document (PRD)

## Product Name

Vehicle Diagnostic RAG Chatbot – MVP

## Document Purpose

This PRD defines the **functional and UX requirements** for building a **1–2 day MVP** of a web-based, RAG-powered vehicle diagnostic chatbot for **non-technical drivers**.

This document is intended to be directly usable by:

* Frontend developers (Web UI)
* Backend/AI developers (RAG pipeline)

Business, monetization, and scalability considerations are **explicitly out of scope**.

---

## 1. Product Objective (MVP)

Enable a non-technical driver to:

1. Describe a vehicle problem using **voice or text**
2. Receive a **simple, grounded explanation** if knowledge exists
3. Receive an **explicit “I don’t know”** response if knowledge does not exist

The system must **never hallucinate** or provide unsafe mechanical advice.

---

## 2. Target User Profile

* Non-technical vehicle driver
* No understanding of:

  * Fault codes
  * Automotive terminology
* Uses a mobile phone or desktop browser
* Primary goal: *“Help me understand what might be wrong and how serious it is.”*

---

## 3. Platform & Access

### 3.1 Platform

* Web application only (MVP)
* Mobile-first responsive design

### 3.2 Supported Browsers

* Chrome (latest)
* Safari (latest)
* Edge (latest)

---

## 4. Core User Flow (End-to-End)

### Primary Flow: Successful Diagnosis

1. User opens the web page
2. User chooses **Voice** or **Text** input
3. User describes vehicle problem
4. System processes input
5. System checks vector store for relevant knowledge
6. System returns:

   * Plain-language diagnostic response

### Alternate Flow: No Knowledge Available

1. User submits problem
2. Vector similarity below threshold
3. System responds with:

   * Clear inability to help
   * Recommendation to seek professional inspection

---

## 5. Web UI Requirements

### 5.1 Landing / Main Screen

**Elements:**

* Page title: "Vehicle Diagnostic Assistant"

* Short instruction text:

  > “Describe the problem you’re facing with your vehicle.”

* Input area:

  * Large text box (editable)
  * Microphone button (voice input)

* Primary action button:

  * Label: **“Analyze Problem”**

---

### 5.2 Voice Input Interaction

**Behavior:**

* Clicking microphone button:

  * Requests browser microphone permission
  * Shows recording indicator (icon + text)
* While recording:

  * Live transcription shown in text box (optional but preferred)
* Stop recording:

  * Automatically fills text input

**Failure states:**

* Microphone permission denied → show fallback message:

  > “Microphone access denied. Please type your problem instead.”

---

### 5.3 Text Input Interaction

* User can directly type problem statement
* Minimum input length: ~5 words (soft validation)
* No technical validation required

---

### 5.4 Loading / Processing State

After user clicks **Analyze Problem**:

* Disable input controls
* Show loading indicator:

  * Spinner
  * Text: “Analyzing your issue…”

---

### 5.5 Response Display Area

The response area appears below the input after processing.

#### Case A: Knowledge Available

Response is shown as **structured plain text**, using headings or bold labels:

* **What might be happening**
* **How serious this is**
* **What you should do next**

Tone requirements:

* Calm
* Reassuring
* Non-technical

#### Case B: Knowledge Not Available

Show a clearly styled message:

> “I don’t have enough reliable information to help with this issue.”

Followed by:

> “It’s best to get the vehicle checked by a professional service center.”

No speculation allowed.

---

### 5.6 Error States (UI)

| Scenario            | UI Behavior                                     |
| ------------------- | ----------------------------------------------- |
| Backend unavailable | “Something went wrong. Please try again later.” |
| Empty input         | Prompt user to describe the issue               |
| STT failure         | Ask user to type instead                        |

---

## 6. Interaction Rules & Constraints

### 6.1 Language & Tone Rules

The system must:

* Avoid automotive jargon
* Avoid certainty (“definitely”, “guaranteed”)
* Use cautious language (“may”, “often”, “usually”)

---

### 6.2 Safety Constraints (MVP)

The system must **not**:

* Provide step-by-step repair instructions
* Encourage DIY mechanical work
* Claim the vehicle is safe/unsafe to drive with certainty
* Guess when knowledge is insufficient

---

## 7. Backend Behavior (Frontend-Relevant)

### 7.1 Input → Output Contract

**Frontend sends:**

* Plain text problem description

**Frontend receives:**

```json
{
  "status": "success" | "no_knowledge" | "error",
  "response_text": "string"
}
```

---

### 7.2 Knowledge Threshold Logic (Abstracted)

* Frontend does **not** decide confidence
* Backend returns either:

  * `success`
  * `no_knowledge`

Frontend simply renders accordingly.

---

## 8. Non-Goals (Explicitly Out of Scope)

* Multi-turn conversations
* Vehicle profile storage
* Image uploads
* OBD integration
* User accounts or history
* Infotainment system integration

---

## 9. MVP Definition of Done

The MVP is considered complete when:

* User can access the app via browser
* User can submit a problem via voice or text
* System either:

  * Returns a grounded, simple explanation
  * Or clearly says it cannot help
* UI handles loading and error states cleanly
* No unsafe or hallucinated advice is displayed

---

## 10. Future Enhancements (Not Required for MVP)

* Follow-up diagnostic questions
* Text-to-speech responses
* Vehicle context capture
* Mobile app
* Infotainment system deployment

---

**End of PRD**
