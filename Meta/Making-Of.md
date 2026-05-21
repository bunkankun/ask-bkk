# How this catalog was made

This catalog was compiled using **Claude Code**, a coding agent by the AI company **Anthropic**, between April 23 and May 19, 2026.  The bulk of the work was done with the model *Opus 4.7*, with 'xhigh' effort.  Towards the end of the period, a switch to Sonnet 4.6 with 'medium' effort was made.  Running out of tokens, the last 74 texts in KR4k and the overview of section KR4k was made using the same harness, but with the model *Deepseek V4 Pro* instead.  The Anthropic sourced interactions were covered by a Max 20x plan for 1 month, at the cost of `US$ 220`, while the Deepsek part run to roughly `US$ 10`, bringing the total cost to `US$ 230`.  

The process was set up as an unsupervised autonomous process, where the agent was instructed to go over all texts in Kanripo one after another and carry out a certain number of steps to produce the desired result.  Setting up the system in a satisfying way was key to the successful completion. 

## Setting up the environment

When starting this project, I had no specific experience with AI agents, except a few days of playing around with it for light coding tasks.  The first few days were spent setting up the environment and establish a viable path, after which the process went ahead without much oversight.   I will try to describe the lessons learned during this startup phase.

Key to interacting with AI models is to be as explicit as possible about the expected response. In interactive Chat sessions, this is usually evolving in a question-and-answer form of interaction, where the AI responds directly to a user request. For agentic interactions, some of these instructions 

### Data format
Since I had used Obsidian in other projects and found it suitable for the purpose at hand, I designed 