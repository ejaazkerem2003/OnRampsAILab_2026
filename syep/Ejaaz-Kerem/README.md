# CSA Call Copilot: A Scoring Agent for Live Architecture Requirements

## The Problem

I've spent most of my time so far in embedded systems and sensor fusion, where the core skill is noticing when something doesn't quite add up between what a system is doing and what it should be doing. Coming into this internship, I found myself looking for that same kind of gap, just in cloud instead of hardware.

I found it in how Cloud Solution Architects run customer calls. A CSA joins a live conversation to gather requirements for a proposed Azure architecture: workload type, scale, compliance needs, budget, existing stack. Customers correct themselves mid-call, add details later that contradict something said earlier, or mention constraints in passing that never get revisited. By the time the architect sits down to actually design the solution, some of that gets lost or misremembered, not because the architect isn't paying attention, but because tracking a live, shifting conversation and building a mental model of the requirements at the same time is hard to do perfectly every time.

There's a second piece to this that looks separate at first but comes from the same root cause. Once a CSA has a proposed architecture, they're expected to evaluate it against the Well-Architected Framework's five pillars. Doing that well and consistently is not something most technical people can do out of the box. It comes from having sat through hundreds of calls and designs before, building an intuition for what a strong architecture looks like versus a weak one. A newer CSA hasn't built that intuition yet, so both what gets captured during the call and how the resulting design gets judged can vary a lot depending on experience.

Both problems come down to the same thing: a newer architect doesn't yet have the pattern recognition that a senior one has built up over time.

## Target User

The primary user is a Cloud Solution Architect, especially someone early in their career who is still building the judgment to catch every important detail live and to score a design against WAF from memory. They need a way to work from something closer to a senior architect's lens while they're still developing that judgment on their own.

The secondary beneficiary is the customer. When less gets lost between the call and the design, they end up with a more accurate first-draft architecture.

## Solution Concept

The idea is an agent that sits alongside a CSA's customer call and turns the conversation directly into a scored first-draft architecture.

It takes a Teams call transcript as input, either uploaded or pasted in, with manual text entry as a fallback for calls that aren't transcribed. From there it extracts structured requirements from the conversation, things like workload type, scale, compliance needs, budget sensitivity, and existing stack, while handling corrections made mid-call and flagging anything that was never actually discussed instead of guessing at it.

Using those extracted requirements, the agent drafts a first-pass architecture grounded in Azure documentation through RAG, so the proposal is tied to real guidance rather than a generic template. It then scores that proposed architecture against the five Well-Architected Framework pillars, giving a short justification for each score.

The end result is a dashboard-style scorecard summary along with a downloadable file, giving the CSA a grounded starting point that reflects both what was actually said on the call and how well the proposed design holds up against WAF.
