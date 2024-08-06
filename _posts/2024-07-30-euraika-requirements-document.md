---
layout: post
title:  "eurAIka Product Requirements Document"
date:   2024-07-30 15:06:00 -0500
categories: eurAIka update
---

## About eurAIka

**eurAIka** is an open-source AI-powered research platform designed to accelerate every phase of the scientific discovery process. It augments a scientist's abilities with tools for outlining concepts, literature review, writing, programming, and remote collaboration. With **eurAIka**, the scientist can focus on the problem space rather than learning new software skills each time.

The three main components of [eurAIka](https://euraika-sciences.github.io/) are

- [The Whiteboard](https://euraika-sciences.github.io/the-whiteboard/) for outlining concepts, working through ideas visually, writing documents and managing project workflows. 
- [The Librarian](https://euraika-sciences.github.io/the-librarian/) for literature searches, intelligently finding relevant journal articles, managing bibliographies and discussing papers.
- [The Coder](https://euraika-sciences.github.io/the-coder/) to implement coding problems in any software tool.

The [goal of eurAIka](https://euraika-sciences.github.io/2024-02-04-euraika-goals/) is to assist the scientist with all phases of discovery. The platform should include useful features while being as friction-free as possible. It should have a learning curve barely above working on a whiteboard. The platform should be multimodal to be able to interact with the user in written and spoken form, and to be able to understand and discuss plots. eurAIka has similar goals to [Project Kepler](https://kepler-project.org/index.html) so it may be beneficial to merge the two projects, or to mimic some of the ideas in Kepler.

The layout needs to be very flexible so that the working document and a journal article might be open side-by-side, or the current document and a finished document may be open together, or The Whiteboard and The Coder may be open simultaneously. 

**eurAIka** should manage projects seamlessly and transparently to the user to control environments containing working documents, the associated bibliography, literature search results and generated code. Saving to repositories such as Github should be as simple as a button click, or even a natural language command. 

### Benefits 

- Accelerate literature review with automatic summaries
- Reduce programming time through natural language coding
- Focus on advancing science instead of logistics
- Increase exposure by publishing in top journals
- Access cutting edge AI to enhance documentation, coding, and discovery
- Switch any focus pane to full screen to reduce distractions
- Connect to electronic whiteboards with handwriting recognition
- With speech capabilities and voice recognition **eurAIka** becomes a true partner
- Cross-component search using a powerful search function that can find information across all components (documents, literature, and code).
- Project management features to help users track progress and meet deadlines.
- Integration with external tools to connect eurAIka with popular scientific tools and services (e.g., electronic lab notebooks, data repositories, computation services).
- Users may create and save custom workflows that integrate features from all three components.

**eurAIka** is multimodal so it can understand graphs and discuss them with you,  and it can produce interactive charts and plots that help convey your  story.

Let's consider feature requirements for each main function.

## The Whiteboard

**The Whiteboard** is a platform for outlining concepts,  working through ideas visually, and managing project workflow like a  traditional whiteboard. It understands mathematical equations and can  insert figures and citations.

- An easy to use authoring system using [Milkdown](https://milkdown.dev/), or equivalent.
- Automatic document saves, automatic equation and figure numbering and referencing.
- Drag and drop bibliography references and maintenance. 
- Built-in editorial capabilities. This should be similar to the Wild Peaches editorial assistant which considers problem definition, technical accuracy, clarity and structure, solution relevance, visual aids, examples and applications, methodology, readability, future directions, and references. Documents are passed to an LLM with instructions to act as an expert editor and provide feedback.
- Convert from Markdown to any other format with [Pandoc](https://pandoc.org/) and be able to modify the finished document to meet journal template requirements.
- Reformat plots and graphs to publication quality. Suggest image captions.
- Perform calculations, including symbolic manipulations.
- Leverage AI to act as a subject matter expert and aid discovery.
- Collaborate with remote teams in real-time with integrated video chat.
- Publish to your GitHub account.
- Visually map out concepts and project workflow like a traditional whiteboard.
- Robust version control system allows users to track changes, revert to previous versions, and compare different versions of their documents.
- Real-time collaborative editing features allows multiple users to work on the same document simultaneously.
- Mind mapping functionality helps users visually organize and connect ideas.
- Contains a library of customizable templates for common scientific document types (e.g., research papers, grant proposals, lab reports).
- Voice recognition for hands-free note-taking and brainstorming sessions.

## The Librarian

**The Librarian** helps with literature searches,  intelligently finding relevant journal articles for the problem. It  maintains the bibliography and citations, and visualizes relationships  between papers through citation links or by topic. It can suggest  highlights and notes, or generate summaries of key ideas.

- Intelligently discover [relevant papers](https://typeset.io/) and maintain bibliographies.
- [Discuss papers](https://www.chatpdf.com/) with AI to clarify difficult passages and concepts.
- Visualize paper relationships through [citation links](https://www.citationtree.org/) and [topics](https://citationgecko.azurewebsites.net/).
- Generate [summaries](https://sassbook.com/ai-summarizer) of key ideas and highlight critical information.
- Obtain [journal templates](https://www.overleaf.com/latex/templates/tagged/academic-journal) and prepare documents in correct formats.
- Link to a bibliography manager like Zotero, use hotkey to save current document to the bibliography and insert into text.
- Maintain literature search history as part of the project.
- Automated meta-analysis to help researchers conduct systematic reviews and meta-analyses of the literature.
- Plagiarism checking tool to ensure the originality of the user's work.
- Citation style manager allows users to easily switch between different citation styles and automatically reformat their bibliography.
- Conference and journal finder suggests relevant conferences and journals based on the user's research topic and paper content.
- Support for searching and managing preprints from various sources (e.g., arXiv, bioRxiv).

## The Coder

**The Coder** acts as an expert software engineer to  abstract programming away from current languages. Natural language  coding is enhanced by searching for existing tools, reading  documentation, writing and documenting code, and creating unit tests. The Coder has four main components - a calculator mode, a Jupyterlab interface, an IDE for more in-depth programming, and a browser to access online software tools.

- Abstract programming through [natural language coding](https://jupyter-ai.readthedocs.io/en/latest/)
- Convert concepts into working code, run and debug it
- Maintains a database of any language commands and features for [retrieval augmented generation](https://python.langchain.com/expression_language/cookbook/retrieval)
- Support for Julia, Python, R, MATLAB and other languages with [Jupyter kernels](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels)
- Enhanced data analysis, modeling, visualization, and lab equipment control
- [Adaptive analysis](https://www.fast.ai/) uses machine learning to find optimal data solutions
- Provide insight and visualization of datasets
- Symbolic programming understands LaTeX for equations
- Conversational [theorem proving](https://morph.so/blog/the-personal-ai-proof-engineer/)
- Fully customizable and interactive coding environment
- Read and understand plots, extract data points and perform [curve-fitting](https://labplot.kde.org/)
- Publication ready graphs, consistent formatting between sessions (line colors and widths, font styles and sizes, etc.)
- Solve problems directly (without coding) when asked, and provide step-by-step explanations of the solution. The Coder should have the capability of checking its own answers.
- When solving a problem, The Coder should first look to available tools rather than relying on LLM generated answers. The available tools should be everything from a simple calculator to a computer algebra system such as Wolfram Language or even a theorem prover like Lean.
- Be able to open external online programs such as Geogebra.
- The Coder should be a full featured IDE similar in style to RStudio, but capable of handling any language. A minimal set of features for the IDE would be:
  - A text editor for writing code with automatic saves
  - A console to execute code snippets, or run programs
  - Have a fully functional debugger to step through programs and show current states. 
  - Display code outlines on demand
  - Have a separate plot window
  - Maintain a command history
  - For each language maintain a complete help system
  - Interact with the user in natural language to help improve code, obtain help about the language, outline code architecture, and set desired plot parameters
- Code review assistant that suggests improvements and identifies potential issues.
- Reproducibility tools such as automatic environment tracking and packaging of code and data.
- Interactive data exploration allows users to quickly gain insights from their datasets.
- Automatically generate and update code documentation based on the codebase.
- Performance profiling to help users identify performance bottlenecks in their code and suggest optimizations.
- Allow users to easily submit jobs to HPC clusters or cloud computing services for resource-intensive tasks.

## Key points for eurAIka development

1. Integration:
   - Fully integrate the Whiteboard, Librarian, and Coder components.
   - Allow users to seamlessly switch between components or display multiple components simultaneously.
   - Implement a flexible, customizable interface that supports various layouts and workflows.

2. AI Model Integration:
   - Design a flexible AI integration system that supports both powerful online models and local models.
   - Implement a model selection interface for users to choose and configure their preferred AI model.
   - Ensure the platform can adapt to different AI models without significant reconfiguration.

3. Project Management:
   - Develop a transparent project management system that automatically creates and manages environments for each new project.
   - Implement automatic document, code, and data management within each project environment.
   - Create an intuitive project initiation process that requires minimal user input.

4. Data Privacy:
   - Build robust data privacy capabilities into the platform.
   - Allow users to configure privacy settings according to their needs.
   - Implement encryption for data at rest and in transit.
   - Provide clear documentation on data handling and privacy features.

5. Scalability:
   - Optimize the platform for individual researchers and small teams.
   - Ensure efficient resource usage to handle complex scientific workflows.
   - Implement performance monitoring and optimization features.

6. Online/Offline Functionality:
   - Develop core functionality to work offline by default.
   - Implement cloud-based features as optional components that can be enabled on demand.
   - Create a seamless transition between offline and online modes.

7. Desktop Focus:
   - Optimize the user interface for desktop and laptop computers.
   - Ensure compatibility with various screen sizes and resolutions.

8. Extensibility:
   - Design a robust plugin architecture to support custom extensions.
   - Develop clear documentation and APIs for third-party developers.
   - Implement a user-friendly extension management system.

9. Deployment:
   - Create a self-hosted solution optimized for individual installation.
   - Develop a streamlined installation process with minimal technical requirements.
   - Provide comprehensive, user-friendly documentation for installation and setup.
   - Implement an automatic update system to ensure users can easily keep the platform current.

   

