# ARTIFICAL INTELLIGENCE

## Index

- **Fundamentals**
    - [AI Model](#ai-model)
    - [What does the openai client do?](#what-does-the-openai-client-do?)
    - [Roles in AI Conversations](#roles-in-ai-conversations)
    - [Limiting tokens](#limiting-tokens)
    - [What Language Models Do](#what-language-models-do)
    - [Messages array](#messages-array)
    - [Streaming AI response](#streaming-ai-response)
    - [Respones API](#respones-api)
    - [Backend Migration Steps](#backend-migration-steps)

## AI Model

- greeat at recognizing and generating patterns
- can read and generate text
- also called an LLM
- Names using an ID format: **organization/model**
- hosted on a server by a model provider
- these model providers provides an API for you to sen AI reqs
- 3 important questions for your AI reqs:
    1. Where am I sending my requeset? WHich provider will I use?
    2. Which model do I want?
    3. How much will it cost?
- structure of the code won't change moving from provider to  provider
- reqs needs (store them in environment vars):
    - destination : AI_URL
    - model : AI_MODEL (has deprication date!!)
    - identity and permissions : AI_KEY
- **openai JS library**:
    1. not locked to OpenAI platform
    2. easily integrate AI functionality into apps
    3. works with any provider that hosts an OpenAI-compatible API
    (analogy: many different pizzeria and they all know Margherita pizza. )

## What does the openai client do?

- formats requests correctly
- atteches your API key
- handles responses and errors
- saves us from writing raw HTTP calls
- never put on the frontend your env vars!!

## Roles in AI Conversations

- `user` : who is talking to an AI model
- `assistant` : AI model
- user sending reqs in objects `{}`
- AI model response:
    - introduced in 2023
    - **chat completions API** (standard API for any kind of text generation)
    - referring to v1 chat completion API is not outdated, just the first API to be stable and widly adopted
- `system`:
    - separating model's rules and user's request
    - make it context aware
    - **few-shot** (or multi-shot) prompting
        - *shot*: example
        - zero-shot: no examples
        - few-shot: a small number of examples
        - **note!** : cost trade
    - control **temperature**:
        - *how bold the model is* about choosing next tokens
        - every time a model generates text, it's choosing between many possible tokens
        - you can think of temperature as how risky the model is willing to be when choosing its next word
        - higher temperature: model takss creaative risks
        - lower temperature: model plays it safer
    -control **top_p**:
        - *how selective the model* is about the possible next words
        -top_p: 1.0 -> default (considers every possibility)
        -top_p: 0.9 -> ignores least likely options
        -top_p: 0.5 -> very conservative
    - *guidlines*: rarely touch values of temperature or top_p once things are stable, tune early, never extreme, mostly tune only temperature

## Limiting tokens
- token: a chunk of text the model processes
- **limit max_tokens** / **max_completion_tokens**: warning!! incomplete answers
- refining prompt : be explicit with style, limits, structure.
- token costs and context windows:
    - models process tokens
    - tokens cost time and money
    - models have hard limits on how many tokens they can process at once
    - analogy: the screen acts as window that reveals only a portion of the full conversation, as you scroll, new messages enter the window and older ones move out of the view -> chat is the full context, and the visible area of the screen is the **context window** 
    - the danger of exceeding the context window: 
    > if the total number of tokens you send in your input messages array exceeds the context window size, than the request doesn't degrade gracefully, it just fails.

## What Language Models Do
- process tokens
- have no nuilt-in converstaion history
- only see the context you send
- analogy : calculator can use the previous result, but when push AC button it rests

## Messages array

- contains the context you send to the model
- the model evaulates as : Given this entire messages array, what comes next?
- model not remembers, each API call starts fresh
-conversaations work if you include prior messages.

**architecture:**
    1. turn : *input user msg* and then **output text response**
    2. turn : *input prev user msg, prev text response, new user msg* then **output text response**
    3. turn : *input old user msg, old text response,prev user msg, prev user msg* then **output text response**
    and so on.

## Streaming AI response

- improves UX 
- why stream? :
    1. UI feels frozen while the model works
    2. Since models generate text token by token, streaing fits perfectly
    3. Streamed responses reduce a user's perceived waiting time since they get immidiate visual feedback
    4. *without stream* : one response - full assistant message content available, easy to render, but feels slow
    5. `stream : true`
    6. *with stream* : **chunks**, the API sends chunks of text as the model generates them, the full message only exists if you build it from chunks. (we get a `choices[0].delta.content`)
    7. chunk is **asynchronous iterable**: `for await ... of` loop (sequence of values that arrives overtime)
    8. **note!** edge case: last chunk can be `undefiend`/`null`, we need to handle it.

## Respones API
~ chat completions API
- can perform complex web seraching and synthesize those results into your response with just one line.
- change in syntax : `openai.responses.create({})`, `input: []`, `response.output_text`
- easier access, cleaner code
- structured outputs with schemas
- have access  to **web search tool** - live information from the web and can control it, not training data
- **agentic function calling**

## Backend Migration Steps

1. Frontend makes request to the server with your user's prompt
2. Server handles the request and prepares a messages array
3. Server  makes a chat completions request to the Model Provider
4. Server extracts the AI Model's response and sends it to the Frontend
5. Frontend converts the Markdown string to sanitized