# Generative AI and Where It Fits in the Microsoft AI Landscape

## What Generative AI Is

Generative AI is a category of AI models that create new content from a natural language prompt, rather than just analyzing or predicting from existing data the way more traditional AI approaches do. A generative model instead produces new content, whether that is text, an image, or code, based on what you ask it for.

At its core, a generative model takes your input and turns it into a matching output. Under the hood, the text you type gets broken into smaller pieces called tokens, which the model converts into numbers it can actually work with. From there, the model predicts the next token based on probability, one piece at a time, until it has built a full response. This is also why the same prompt can give you a slightly different answer each time. The model is not just picking the most likely next word, there is a degree of randomness built in so the output feels less mechanical and more like a first attempt you can build on rather than a fixed answer.

Generative AI also continues to improve as people use it. Each interaction can help refine the quality and relevance of what it produces, so the more it is used, the more it adapts to what users are actually asking for. Large Language Models, or LLMs, are what make all of this possible. They are trained on massive amounts of text so they can understand language and generate responses that read naturally, and they are the backbone behind nearly every generative AI tool in use today.

## How It Works

Large Language Models are what actually power generative AI, and there is more than one type. Models can differ based on what they generate, some handle text, others handle images, audio, or code, and businesses pick a model based on the task, the data available, and cost. Alongside LLMs, embedding models play a supporting role. They convert text into a numerical representation, which makes it possible for a system to compare pieces of text based on meaning rather than just matching keywords. This numerical form is what makes retrieval possible.

There are three main ways an LLM can get the knowledge it uses to answer a question. The first is pretrained knowledge, meaning whatever the model already learned during training. The problem is that pretrained knowledge is frozen at whatever point the model stopped training, so it has no way of knowing about anything more recent or anything private, like internal company data. The second is Retrieval-Augmented Generation, or RAG, where the model pulls in outside knowledge at the time you ask a question instead of relying only on what it already knows. The third is Retrieval-Centric Generation.

RAG is worth walking through in more detail since it is the approach most commonly used to ground a model in real, current data. The flow works like this: a user asks a question, which goes to an orchestrator. The orchestrator queries a data or search index to find relevant information, and those results get added into the prompt. That combined prompt, now carrying both the original question and the retrieved context, is sent to the LLM, which generates a response based on both. Behind the scenes, this depends on a vector database, which stores the embeddings mentioned earlier so the system can quickly find the chunks of information most related to the question being asked.

RAG and RCG matter because they solve a real limitation of LLMs. Without them, a model can only answer using what it learned during training, which means it cannot speak to anything current or specific to an organization. Retrieval lets a model stay useful without needing to be retrained every time new information shows up, and it is a big part of why generative AI tools can be adapted to a specific business or specific data rather than staying generic.

## Agentic AI

Agentic AI is the new frontier of generative AI, and it works by specializing an LLM to carry out a specific task rather than just respond to a prompt. At a basic level, an agent is an LLM that has been given instructions and access to tools, so instead of only generating text, it can actually go do something with it.

What makes an agent function is a loop. The agent takes in an input, reasons through what needs to happen, acts on that reasoning, often by calling a tool, and then produces an output based on the result. This loop repeats as needed, with the agent drawing on its knowledge, the actions available to it, and memory of what has already happened in the conversation, so it can keep building toward completing the task rather than stopping after a single response.

This is also the main thing that separates an agent from a regular chatbot. A chatbot responds with text based on a prompt and that is where its job ends. An agent can take that same reasoning and turn it into action, whether that means querying a database, calling an API, or using another tool entirely, and then factor the result of that action back into what it does next. That ability to act, not just respond, is what makes agentic AI a meaningfully different category of generative AI.

## How This Fits in Microsoft

Microsoft has a lineup of AI products that put these concepts into practice, including GitHub Copilot, Copilot Studio, Visual Studio Copilot, Microsoft Foundry, and Microsoft Fabric, and each one maps back to a piece of what generative and agentic AI actually do.

Microsoft Foundry is where the model side of things lives. It lets teams explore, customize, and manage AI models and agents at scale. Microsoft Agent Framework is the newer open-source development kit for building AI agents and multi-agent workflows in .NET and Python, a more hands-on, developer-first path to building an agent. Copilot Studio takes a different approach to that same problem, letting someone build and extend AI-driven copilots without writing as much code, making it easier to add a working chat experience to a website or app. Azure AI for Developers rounds this out by giving developers a way to build generative AI apps directly on Azure using OpenAI services.

These products are not separate tools so much as different entry points into the same set of AI capabilities. Someone building at the model and infrastructure level would work in Microsoft Foundry, someone building a custom agent from scratch would reach for the Agent Framework, and someone who wants to build or extend a copilot with less code would use Copilot Studio. GitHub Copilot and Microsoft 365 Copilot then sit on top of all of this as the everyday, already-built products most people actually interact with, turning generation, retrieval, and agentic behavior into real tools people use at Microsoft and beyond.

## Closing

Generative AI comes down to a few connected pieces: how LLMs generate a response, how retrieval fills in what a model does not already know, and how an agent takes that same reasoning and turns it into action. Together, these pieces explain why generative AI has become such a big part of how Microsoft builds products.

I plan to carry this understanding into the rest of my internship project, where I will be building an agent using Copilot Studio. Having a working sense of how models, retrieval, and agent behavior fit together should make it easier to understand the choices behind that tool rather than just using it.

## Sources

- Cameron Jackson, Generative AI and Agentic AI session notes (Week 1)
- microsoft/generative-ai-for-beginners, Parts 1, 2, 15, & 17
- Microsoft AI Learning Hub (learn.microsoft.com/en-us/ai)
