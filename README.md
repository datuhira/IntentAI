<p align="center">
  <img src="assets/intent-logo.svg" width="520" alt="IN/TENT">
</p>

<p align="center"><strong>An adaptive prompt builder that turns rough ideas into task-specific instructions.</strong></p>

<p align="center">
  <a href="https://intentaiprompt.netlify.app/workspace.html"><strong>Open the working demo</strong></a>
</p>

# IN/TENT: Product Development Log

**Current stage:** Working front-end prototype  
**Source code:** Private  
**Role:** Product design, interaction design, front-end development, prompt-system design, and testing

IN/TENT asks a user what they want to accomplish, identifies the kind of work involved, and builds a detailed prompt for the AI tool they plan to use. I am documenting the project here to show how the product changes as I test it, find weak points, and rebuild them.

## Demo note

The production backend and abuse protections are not built yet.

Please use **one prompt generation** when testing. One run shows the full experience. If the demo receives abusive traffic, I will remove public access while I finish server-side limits.

The demo stores prototype account and usage data in the browser. Do not enter private, sensitive, or confidential information.

## The problem I started with

My first version asked a few questions and returned a prompt, but the result was too generic. It collected information without understanding which details mattered for the task. A request for an essay, a medical terminology breakdown, and a one-page website could still produce prompts with nearly the same structure.

That failure changed the direction of the project. Collecting more text was not enough. The questions and final prompt had to adapt to the user's intent.

## How the adaptive flow works

The current prototype follows this sequence:

1. The user describes what they want in normal language.
2. A lightweight classifier routes the request into a working category: academic or medical, software, creative, business, career, data, planning, or general.
3. The next question changes based on that category. A website request asks about the platform and existing code. Academic work asks about the class level, rubric, sources, and citation requirements.
4. The user chooses the destination model: Claude, Codex, GPT, or Cursor.
5. The user selects how many reference images they will provide. The upload step appears only when the count is above zero.
6. A task-specific prompt profile assembles the role, desired outcome, audience, requirements, boundaries, deliverables, quality checks, clarification rules, and target environment.
7. The completed prompt is saved to history and can be copied into the selected AI.

This is still a browser prototype. The adaptive behavior currently uses explicit routing rules and task profiles so I can test the product logic before connecting a production model and backend.

## Problems I ran into and what I changed

### 1. The generated prompts sounded generic

The earliest output repeated the user's words inside a broad template. It looked detailed, but it did not tell the destination AI how to handle that specific kind of work.

I replaced the single template with task-specific prompt profiles. Medical terminology requests now ask for pronunciation, word-part analysis, plain-language meaning, clinical context, examples, related terms, and safety boundaries. Software requests produce product intent, information architecture, visual direction, responsive behavior, components, interaction states, implementation constraints, and acceptance checks.

The result now changes with the job instead of changing only the title.

### 2. A fixed questionnaire asked the wrong questions

The first flow treated every request the same. Users had to answer questions that were useful for one task and irrelevant to another.

I added category detection and adaptive follow-up questions. The flow keeps a small shared foundation, then asks for the details that affect the selected type of work. This reduced unnecessary questions without removing the context needed for a strong prompt.

### 3. Image attachments were referenced but not available to the destination AI

During testing, an uploaded image appeared in the IN/TENT interface, but the copied prompt carried only its filename. The destination AI correctly reported that it could not see the file.

I changed the result screen to keep the image references visible and tell the user to attach the same files with the copied prompt. The production version will need image storage and a vision-capable API request so the handoff happens automatically.

### 4. Users could not correct an earlier answer

The guided flow originally moved only forward. A typo or wrong selection meant starting over and losing the rest of the work.

I added Back controls and answer restoration. Text responses, model choices, image counts, and selected image previews remain available when the user returns to an earlier step.

### 5. Copy confirmation was easy to miss

The first copy interaction depended on a small label and the browser Clipboard API. In some preview environments the API could fail silently, and the visual response was too subtle.

I added a fallback copy method and changed the button itself from orange to green with a checkmark after a successful click. The confirmation now happens at the exact place where the user acted.

### 6. The workspace needed history without becoming cluttered

Saved prompts made the sidebar useful, but a flat list became difficult to scan. I added deletion, persistent pinning, and separate Pinned and Recent groups. The user can reopen a completed prompt without spending another generation.

### 7. Prototype limits are not real security

The demo uses local storage for accounts, plans, history, and generation counts. This works for interface testing, but anyone can reset or modify browser data. Front-end code also cannot protect an admin password or enforce a subscription.

A production release needs server-side authentication, database records, usage enforcement, billing webhooks, and role checks. I have kept those limitations visible rather than presenting the demo as production-ready.

## What works in the current demo

- Account creation and sign-in screens
- Two prototype generations for a regular free account
- Admin access for reviewing all screens without consuming credits
- Adaptive questions based on the user's request
- Model selection for Claude, Codex, GPT, and Cursor
- A 0–8 image reference selector with conditional upload
- Back navigation with restored answers
- Animated progress and generation states
- Task-specific prompt construction
- Saved prompt history with pin, reopen, and delete controls
- Copy-to-clipboard feedback and fallback behavior
- Pricing and student-plan interface concepts
- Responsive desktop and mobile layouts

Payment, `.edu` verification, generation limits, and account permissions are interface prototypes. They are not connected to production services.

## Technical approach

The current demo is a static HTML, CSS, and JavaScript application hosted on Netlify. I used browser storage to test account states, plans, prompt history, and credits before committing to a backend architecture. The prompt builder uses explicit JavaScript routing and structured profiles so each rule can be inspected and revised during testing.

I also adapted interaction ideas such as spring transitions, rolling counters, split selectors, progress indicators, magnetic buttons, and animated status changes without adding a React runtime to the static prototype.

## Product direction

The current product concept includes:

- Free: 2 prompt generations
- Student: 60 monthly generations for verified `.edu` accounts at $4.99 per month
- Pro: 120 monthly generations at $9.99 per month
- A student referral concept with a 30-day holding period and monthly payout cap

These plans are part of product testing. Pricing, limits, and referral terms may change before launch.

## Next engineering steps

1. Move authentication and sessions to a secure backend.
2. Store accounts, plans, prompt history, and usage in a database.
3. Add server-side generation limits and admin authorization.
4. Connect a production prompt-generation model.
5. Send uploaded images through a vision-capable request instead of filename handoff.
6. Add Stripe checkout, billing webhooks, cancellation handling, and account management.
7. Verify `.edu` eligibility on the server.
8. Add automated tests for routing, credit deductions, navigation state, and prompt output.
9. Test prompt quality across more subjects and compare generated results against defined evaluation cases.

## Why the source is private

IN/TENT is planned as a paid product, so this repository contains the development log, decisions, and selected visuals rather than the application source code. The public demo exists for evaluation and feedback.

## Ownership

Copyright 2026 Jon Esteves. All rights reserved.

IN/TENT, its source code, product logic, interface, visual identity, documentation, and related materials are proprietary. No license is granted to copy, modify, redistribute, reverse engineer, sell, or create derivative works from the product or its materials.

Third-party product names belong to their respective owners. IN/TENT is not affiliated with or endorsed by those companies.
