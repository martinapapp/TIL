# ARTIFICAL INTELLIGENCE

##Index

##AI Model:
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

## Roles in AI Conversations:
- `user` : who is talking to an AI model
- `assistant` : AI model
- user sending reqs in objects `{}`
- AI model response:
    - introduced in 2023
    - **chat completions API** (standard API for any kind of text generation)
    - referring to v1 chat completion API is not outdated, just the first API to be stable and widly adopted
- `system`:
    - separating model's rules and user's request


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

## What Language Models Do:
- process tokens
- have no nuilt-in converstaion history
- only see the context you send
- analogy : calculator can use the previous result, but when push AC button it rests

## Messages array: 
- contains the context you send to the model
- the model evaulates as : Given this entire messages array, what comes next?
- model not remembers, each API call starts fresh
-conversaations work if you include prior messages.

**architecture:**
    1. turn : *input user msg* and then **output text response**
    2. turn : *input prev user msg, prev text response, new user msg* then **output text response**
    3. turn : *input old user msg, old text response,prev user msg, prev user msg* then **output text response**
    and so on.



