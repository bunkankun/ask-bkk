# How this catalog was made

This catalog was compiled using **Claude Code**, a coding agent by the AI company **Anthropic**, between April 23 and May 19, 2026.  The bulk of the work was done with the model *Opus 4.7*, with 'xhigh' effort.  Towards the end of the period, a switch to Sonnet 4.6 with 'medium' effort was made.  Running out of tokens, the last 74 texts in KR4k and the overview of section KR4k was made using the same harness, but with the model *Deepseek V4 Pro* instead.  The Anthropic sourced interactions were covered by a Max 20x plan for 1 month, at the cost of `US$ 220`, while the Deepsek part run to roughly `US$ 10`, bringing the total cost to `US$ 230`.  

The process was set up as an unsupervised autonomous process, where the agent was instructed to go over all texts in Kanripo one after another and carry out a certain number of steps to produce the desired result.  Setting up the system in a satisfying way was key to the successful completion. 

## Setting up the environment

When starting this project, I had no specific experience with AI agents, except a few days of playing around with it for light coding tasks.  The first few days were spent setting up the environment and establish a viable path, after which the process went ahead without much oversight.   I will try to describe the lessons learned during this startup phase.

Key to interacting with AI models is to be as explicit as possible about the expected response. In interactive chat sessions, this is usually evolving in a question-and-answer form of interaction, where the AI responds directly to a user request.  The main difference between chat sessions and agent use is the so-called *harness*: A local program that receives the input and decides what to do with it, for example calling tools or other agents, or sending it directly to the model for response.  The response will also be received by the harness, and acted upon, until at some point a final action brings this loop to an end: a message to the user, or  a newly written catalog entry in our case. 

For agentic use, written instructions also form part of the harness.  Some of these are very basic instructions that set up the agent for a specific task, others are more fleeting and depend on the context.  In the case of Claude Code, this fundamental file is called `CLAUDE.md`.  The first part of the bootstrapping process was therefore to set up this file.  I did this in an interactive manner, trying some requests and refining these according to the result, then updated the agent file with the result.   In addition to the task instruction, I drafted a  basic template for the catalog entry, called `template.md`   Eventually, I asked the 

### Data format
Since I had used Obsidian in other projects and found it suitable for the purpose at hand, I designed 