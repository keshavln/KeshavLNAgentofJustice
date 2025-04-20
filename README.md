# KeshavLNAgentofJustice

This repository contains my take on the Cynaptics Phase 2 Task, which is to simulate the proceedings of a courtroom using LLM agents to represent different parties present during trial. A text describing the case and establishing basic facts is provided as input.

My approach is based off the provided starter code, but has several modifications. 

Changes made to the initial code:

-  Huggingface has been replaced with Groq's API. The LLM used is Mistral Saba 24b, which has 24 billion trainable parameters.
-  The function _format_prompt, which brings a user prompt into a form that is syntactically valid for processing by the LLM, originally had a loop which converted messages (a list of dicts) into a string. This was removed as the new model already accepts prompts in the original form.
- The self.history attribute of the class was removed and replaced with a global history variable. This is so that every agent has access to what every other agent has said during the proceedings, instead of having access to only its own history.

The proceedings are governed by six agents: the judge, defence, prosecution, defendant, plaintiff and witnesses. The defence and prosecution are allowed to call witnesses at any given point of time, should they feel it necessary. Only when a witness is summoned is a witness agent created and made to respond to the questions raised by the defence/prosecution. A new witness agent is created every time a new witness is summoned.

The proceedings start off with opening statements, where each party involved provides a brief introduction as to their stance. No witnesses are called during this stage. After the opening statements, the rounds begin. At the end of every round, the judge evaluates all arguments presented. If the judge agent is able to reach a verdict, it declares the defendant guilty or not guilty and closes the case. If it feels that more elaboration is required, it may call for another round. The maximum number of allowed rounds is 4. If the judge is unable to make a verdict by the end of the fourth round, it postpones the remainder of the proceedings. In order to minimise repetition of points, the plaintiff and defendant have been prompted to keep their responses terse.

Improvements:
- Initially, the responses generated were occassionally incomplete. To fix this issue, a prompt was included to keep the word limit of responses within a certain range and to prioritise finishing sentences.
- Each agent has been prompted to always add its own name at the beginning of every response. This is so that other agents can understand which parties spoke particular dialogues when the agent's response is eventually added to the global history. The agents' ability to recognise and refute the other side's arguments increased significantly after this implementation.
