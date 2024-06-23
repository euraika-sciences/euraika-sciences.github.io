---
layout: post
title:  "AI Generates AI"
date:   2024-03-05
categories: eurAIka update
permalink: /2024-03-05-ai-generates-ai/
---



## Coding for non-coders

Quite a bit of the eurAIka website was built with AI assistance. I used Anthropic's Claude to work through details. I wrote up a description of the project and asked Claude to recommend rewrites. After a few iterations, the result is the complete website, [*eurAIka - The Scientist's Assistant*](https://euraika-sciences.github.io/).

The video required a bit more fine-tuning to reduce the length to 30 seconds, and the narration was done with [NaturalReader](https://www.naturalreaders.com/index.html) using the voice "Ava" (no longer available). Some of the images were generated with [DreamStudio](https://dreamstudio.ai/generate), as was the logo using the prompt, "A tesseract with metallic green spheres at the nodes and silver metallic rods for the connections." DreamStudio doesn't seem to understand the definition of *tesseract*, but I liked the image it created, so I kept it. 

I asked Claude to write a program in [Octave](https://octave.org/) to convert a sequence of latitude and longitude points into a *.kml* file to be imported into Google Earth similar to the Matlab [*kmlwrite*](https://www.mathworks.com/help/map/ref/kmlwrite.html) function. The function Claude produced worked, generating lines connecting each point. I then asked for the option to plot points, or points connected by lines which Claude successfully produced. This is the response I'm hoping to achieve with The Coder.



## The setup

[SuperAGI](https://superagi.com/) is an "[open source](https://github.com/TransformerOptimus/SuperAGI) autonomous AI agent framework enabling developers to build, manage & run useful autonomous agents." 

![superagi-architecture](C:\Users\johnx\Documents\GitHub\euraika-sciences.github.io\assets\img\2024-03-05-ai-generates-ai\superagi-architecture.png)

Other similar projects are [crewAI](https://www.crewai.com/), [AgentGPT](https://agentgpt.reworkd.ai/), [babyAGI](https://github.com/yoheinakajima/babyagi) and others. 

