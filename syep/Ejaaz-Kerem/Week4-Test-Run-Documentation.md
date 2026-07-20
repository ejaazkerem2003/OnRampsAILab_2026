# Testing My CSA Assistant — Week 4

I built the agent in Copilot Studio with three skills chained together: one that reads a customer call and pulls out the requirements, one that turns those requirements into a rough Azure architecture, and one that scores that architecture. Once all three were built, I wanted to actually test them with a realistic example instead of just trusting that the instructions I wrote would work the way I expected.

Below is the fake customer call I used to test it, what happened at each step, and what I found when I ran Copilot Studio's built-in testing tool afterward.

## The test call I used

I made up a transcript that looks like a real Teams meeting, with a few tricky things built into it on purpose: someone corrects a number partway through, someone offers a budget and then walks it back without giving a new number, there's a compliance requirement (PCI DSS, which is just the security standard you have to follow if you handle credit card payments), and nobody ever brings up anything about monitoring or day-to-day operations.

I wanted to see if the agent would actually catch all of that, or if it would just smooth over the messy parts like a person skimming a meeting recap might.

```
0:00:12 Sarah Chen: Thanks for hopping on. So as I mentioned in the invite, we're
looking to move our retail order platform off our on-prem servers.

0:00:24 Sarah Chen: Right now everything runs on a single SQL Server box with a
.NET web app in front of it. That part's not changing anytime soon, we just want
the customer-facing app in the cloud.

0:00:41 Mike Torres: Yeah and traffic-wise we're expecting around 10,000 daily
active users at launch, growing from there.

0:00:55 Priya Patel: Budget's pretty flexible for this project honestly, we've got
about $50k a month set aside for the infrastructure piece.

0:01:10 Sarah Chen: Oh, one more thing, since we process card payments directly
we'll need to be PCI DSS compliant. That's non-negotiable given what happened with
our last vendor audit.

0:01:28 Mike Torres: Actually, sorry, let me correct something I said earlier.
Traffic's going to be a lot higher than 10,000, marketing's planning big seasonal
sale events, so we should design for up to 100,000 daily users during peak
periods.

0:01:47 Priya Patel: And actually I talked to finance again this morning, they
want to walk back on that budget flexibility. They're pushing for a leaner spend
on this given some other projects competing for funds this quarter.

0:02:03 Sarah Chen: Yeah we don't have a new number yet, finance is still working
through it, but just know it's not going to be as open as Priya said before.

0:02:15 Mike Torres: Makes sense. Anything else we should flag before we wrap?

0:02:19 Sarah Chen: I think that covers it for now.
```

## Step 1: pulling out the requirements

The agent read through the whole thing and used 100,000 users as the real number, not 10,000, and it noted that the original number got corrected instead of just quietly using the new one with no explanation. That part matters because in a real call, whoever's taking notes might miss that a number changed if they're not paying close attention the whole time.

The part I was most curious about was the budget. Someone offered 50k a month, then someone else walked that back later without giving a new number. I was worried the agent might just default back to the $50k since that was the only real number it had. Instead, it said flat out that the budget was unresolved and gave a "to be determined" note instead of making up a number. That's the behavior I actually care about most, since a made-up budget could send someone down the wrong path entirely.

It also correctly figured out that the existing SQL Server database wasn't part of what needed to move to the cloud, since the customer said that piece wasn't changing. And it noticed, on its own, that the walked-back budget and the never-restated number were connected, and it asked me what I wanted to do about it instead of just picking an answer for me.

## Step 2: drafting a rough architecture

Once I told the agent how to handle the open budget question, it moved on to sketching out an architecture. It organized things into categories (compute, storage, networking, security, and monitoring/operations) and explained why it picked each service based on something specific from the call, not just generic best practices.

One thing I liked: since the customer said the database was staying on-prem, the agent didn't suggest an Azure database at all. It didn't just default to adding one because that's usually what you'd expect in a cloud migration.

Since the budget was still up in the air, it gave me two versions side by side, a cheaper option and a fully scaled-up option, instead of guessing which one the customer would actually want.

It also flagged some things I hadn't even thought to ask about, like the fact that the app would be running in the cloud but every request still has to reach an on-prem database, which could become a real problem if that connection ever goes down. That wasn't something the customer brought up, the agent noticed it while working through the design.

One small thing I want to clean up later: that on-prem database risk isn't really a conflict between two things the customer said, it's more like a risk the design itself revealed. Right now my instructions lump that in with actual conflicts (like budget vs. compliance cost), so I split those into two separate categories going forward, conflicts between what the customer said, and risks the agent notices on its own while designing.

## Step 3: scoring the architecture

The last skill scores the design against Microsoft's Well-Architected Framework, which is basically five categories (reliability, security, cost, day-to-day operations, and performance) that Microsoft uses to judge whether a cloud design is solid.

I used a simple four-level scale instead of a number score, since giving a precise number for a rough first draft felt like it was implying more confidence than the design actually had.

The agent lowered the score on cost, reliability, and security, and explained clearly that it wasn't lowering them because the design was bad, it was lowering them because we genuinely don't have enough information yet (no confirmed budget, no stated uptime requirement, unclear how much of the payment process actually touches Azure). That distinction mattered to me, I didn't want it dinging a design for being incomplete when the incompleteness was really coming from the customer call itself, not from a bad design choice.

It also lowered two more categories (operations and performance) based on gaps it spotted on its own, missing runbooks for handling incidents, and no real plan for how the database would perform once traffic got heavy. Those weren't things I had told it to check for specifically, it noticed them while going through the scoring. That's a good sign, but it also means my instructions need a small update so they explicitly allow that kind of independent judgment instead of only allowing it to flag things that were already written down somewhere earlier.

## Running the built-in test tool

Copilot Studio has a feature that auto-generates test questions and checks whether the agent's answers are relevant and complete. I ran it and got a 64% score, 7 out of 11 passed.

At first that looked bad. But when I actually opened up the failing ones, all four were cases where the test tool asked the agent to score or draft something without actually giving it anything to work with, like asking "score this architecture" with no architecture attached. In every one of those cases, the agent did exactly what I wanted, it said it didn't have enough to go on and asked for the missing piece instead of making something up.

Here's what happened in one of those "failed" cases, step by step: I asked it to score a draft with nothing attached, and it correctly asked me to provide one. I then gave it just a short list of service names, not a real detailed draft. Instead of treating that short list like a complete architecture, it said plainly that this was just a list of services, not a full design, and it lowered basically every score because of how little information it had to work with.

So the tool marked that as a failure, but reading through it myself, that's actually the agent behaving exactly the way I wanted. The automatic grading tool seems to reward answering immediately and confidently, and it doesn't seem to know how to judge an agent that's supposed to pause and ask questions instead. So I don't think the 64% is really telling me the agent has a problem, I think it's telling me the grading tool isn't built for an agent like this one.

## What I still want to do

- I already fixed the design tensions vs. risks-noticed mixup from step 2.
- I already updated the scoring instructions so it's allowed to lower a score for things it notices itself, not just things flagged earlier.
- I want to write a few of my own test questions with an answer key, instead of relying only on the auto-generated ones, so future test runs don't punish the agent for asking good questions.
- I haven't connected the agent to real Azure documentation yet (RAG grounding), that's still the next big thing to add.
