# Revolutionizing Version Control: An AI-Powered Approach to Simplifying Git

**Author**: Georges Arthur Engono | [georges.arthur.engono.essame@umontreal.ca](mailto:georges.arthur.engono.essame@umontreal.ca)
**Supervisor**: Eugène Syriani |  [syriani(@)iro.umontreal.ca](mailto:syriani(@)iro.umontreal.ca)

---

### Introduction

In the world of software development, version control systems are indispensable for managing changes to source code over time. Git, created by Linus Torvalds in 2005, has emerged as the predominant version control system, lauded for its flexibility and distributed nature. However, its widespread adoption has not been without challenges. The steep learning curve, complex command syntax, and inefficient workflows have been significant barriers for many developers, particularly those new to version control or those working in fast-paced, collaborative environments.

### Problem Statement 

Git's robust functionality is overshadowed by its user-unfriendly interface and complex workflows. Major challenges include:

- **Non-Intuitive Design**: While powerful, Git's user interface is complicated, often requiring deep understanding or memorization of commands.
- **Confusing Command Syntax**: Git commands can be perplexing, with multiple functionalities often packed into a single command, leading to user confusion.
- **Merge Conflicts**: A significant amount of time is spent avoiding or resolving merge conflicts, particularly in teams working on different features simultaneously.
- **Steep Learning Curve**: Git's learning curve is daunting, necessitating the learning of a domain-specific language, which can lead to critical errors.
- **Increased Workflow Steps**: Tasks that are straightforward in other systems, like TFS, are more cumbersome in Git, requiring additional steps and time.

These challenges can hinder productivity, increase frustration, and elevate the risk of errors, particularly for new contributors and smaller project teams.

### Proposed Solution: AI-Powered Chatbot for Git in CLI

To revolutionize the way developers interact with Git, we propose the development of an AI-powered chatbot, integrated within the Command Line Interface (CLI). This solution is designed to simplify Git commands and workflows, making them more accessible and efficient. The chatbot will offer the following features:

- **Natural Language Interaction**: Users can communicate with Git through natural language queries and instructions. Instead of typing traditional Git commands, users can ask questions like, "How do I commit my changes?" or state commands such as, "Update my branch with the latest changes from the main branch."

- **Project Structure Awareness**: The chatbot will have a deep understanding of the current project's structure. It will be aware of critical details like previous commits, the number of branches, active contributors, and recent changes. This knowledge allows the chatbot to provide contextually relevant advice and actions, tailored to the specific state and needs of the project.

- **Personalized Assistance and Error Prevention**: By understanding the project's context and user's intent, the chatbot can offer personalized guidance. It can suggest the most appropriate Git actions, help avoid common mistakes, and alert users to potential conflicts or issues based on the project's history and current status.

- **Facilitating Complex Operations**: The chatbot can assist with more complex Git operations that typically require multiple steps and a deep understanding of Git's intricacies. For example, it can guide users through resolving merge conflicts, rebasing branches, or managing pull requests.

- **Seamless CLI Integration**: This chatbot will be a part of the CLI environment, ensuring that developers can access its capabilities without leaving their terminal or interrupting their workflow. This integration provides a smooth, cohesive experience, blending the power of AI with the familiarity of the CLI.

  

### High-Level Architecture



![](/Users/garthur007/Desktop/Université/Session VI/IFT 3150/bronco_doc/media/High Level Architecture.png)



This diagram illustrates a high-level architecture for the AI-powered system designed to process Git commands using an LLM (OpenAI) model. Here's a summary of the workflow:

1. **Git Commands**: This is the input to the system, where Git commands are issued by the user.

2. **Text Splitter**: The input Git commands are split into smaller chunks of text. This step is necessary for parsing and make the input more manageable for processing.

3. **Embedding API**: The chunks from the text splitter are passed to an embedding API, which converts the text into numerical vector representations (embeddings). These embeddings capture the semantic meaning of the chunks in a format suitable for machine processing.

4. **Vector Database**: The embeddings are stored or matched against a vector database, such as Pinecone. This database can be used for searching and retrieving relevant documents based on similarity in the vector space.

5. **Query**: This represents the user's query or command that needs to be processed entered from the user's CLI.

6. **Git Project Data**: This includes structured data about the Git project in a JSON format containing metadata, branch information, commit history, etc.

7. **Git Model Generator**: This component uses the Git API to generate or update the Git project data. It interacts with the Git repository and the local project to fetch the latest data.

8. **Git API**: This is the interface through which the Git Model Generator communicates with the Git repositories, allowing it to execute Git operations and retrieve data.

9. **OpenAI Model**: This is the AI model (like GPT-4) which processes the user's query, the Git project data, and the context (possibly including the relevant documents retrieved from the vector database). It generates an answer or output based on this information.

10. **Answer**: This is the final output from the OpenAI model, which is the system's response to the user's query. It could be a direct answer, a Git command execution result, or further guidance on Git operations.

This architecture uses natural language processing and semantic search with a vector database to interpret Git commands, fetch relevant project data, and provide intelligent responses and actions based on the context of the Git project.



### Git Project Model

Creating a model for representing Git projects enhances the bot's understanding of the project's intricate details and current status. This structured approach allows for more accurate, context-aware responses, tailored to the unique characteristics and needs of each project. It significantly improves the bot's ability to handle complex queries, offer predictive suggestions, and prevent potential errors by understanding the project's dynamics.

**Project Metadata**

- `project_name`: String
- `repository_url`: String
- `creation_date`: DateTime
- `last_updated`: DateTime
- `primary_language`: String
- `description`: String

**Branch Information**

- branches: List of Branches
  - Branch:
    - `name`: String
    - `last_commit`: String (Commit Hash)
    - `created_date`: DateTime
    - `last_updated`: DateTime
    - `is_default`: Boolean

**Commit History**

- commits: List of Commits
  - Commit:
    - `hash`: String
    - `author`: String
    - `date`: DateTime
    - `message`: String
    - `branch_name`: String

**File Status**

- files: List of Files
  - File:
    - `file_name`: String
    - `status`: Enum (Modified, Untracked, Deleted)
    - `last_modified`: DateTime

**Issues and Pull Requests**

- issues: List of Issues
  - Issue:
    - `id`: Integer
    - `title`: String
    - `status`: Enum (Open, Closed)
    - `created_date`: DateTime
    - `closed_date`: DateTime (optional)
- pull_requests: List of Pull Requests
  - Pull Request:
    - `id`: Integer
    - `title`: String
    - `status`: Enum (Open, Closed, Merged)
    - `created_date`: DateTime
    - `closed_date`: DateTime (optional)
    - `branch_source`: String
    - `branch_target`: String

**Collaborators**

- collaborators: List of Collaborators
  - Collaborator:
    - `username`: String
    - `role`: Enum (Contributor, Maintainer, Owner)

**Configuration and Settings**

- configurations: Dictionary
  - Key-value pairs for various Git settings (e.g., `merge.conflictstyle`, `core.autocrlf`).

Example

```json
{
  "project_metadata": {
    "project_name": "ExampleProject",
    "repository_url": "https://example.com/repo.git",
    "creation_date": "2024-01-01T12:00:00Z",
    "last_updated": "2024-01-15T08:00:00Z",
    "primary_language": "Python",
    "description": "This is an example project."
  },
  "branches": [
    {
      "name": "main",
      "last_commit": "a1b2c3d4",
      "created_date": "2024-01-01T12:00:00Z",
      "last_updated": "2024-01-15T08:00:00Z",
      "is_default": true
    }
  ],
  "commits": [
    {
      "hash": "a1b2c3d4",
      "author": "johndoe",
      "date": "2024-01-15T08:00:00Z",
      "message": "Initial commit",
      "branch_name": "main"
    }
  ],
  "files": [
    {
      "file_name": "README.md",
      "status": "Modified",
      "last_modified": "2024-01-15T08:00:00Z"
    }
  ],
  "issues": [
    {
      "id": 1,
      "title": "Fix README formatting",
      "status": "Open",
      "created_date": "2024-01-15T08:00:00Z",
      "closed_date": null
    }
  ],
  "pull_requests": [
    {
      "id": 1,
      "title": "Add new feature",
      "status": "Open",
      "created_date": "2024-01-15T08:00:00Z",
      "closed_date": null,
      "branch_source": "feature/new-feature",
      "branch_target": "main"
    }
  ],
  "collaborators": [
    {
      "username": "johndoe",
      "role": "Owner"
    }
  ],
  "configurations": {
    "merge.conflictstyle": "diff3",
    "core.autocrlf": "true"
  }
}

```



### Components

##### Frontend - CLI

...

##### Git commands - Vector Database

...

##### Git API - Git Model

...

##### Text Splitter

...

##### Embedding Generator

...

##### Context Generator

...

##### OpenAI Chatbot

...

### Roadmap

![](/Users/garthur007/Desktop/Université/Session VI/IFT 3150/bronco_doc/media/Roadmap.png)



### Weekly Update

#### Week 1 & 2

Drafted the initial design document and the template for my final report.

Wrote the design document and researched different components of the project. I set up a website for this class where I will be posting my weekly updates.

#### Week 3 & 4

I created the NPM package which will contain the source code of this project.

- #### [This is where it all begins...](https://github.com/georgesengono/git-ai/commit/415cddccf0ae3634e8c6981ec5422a582bcfca42)

I created the text splitter that will be used to split the large pdf file that will serve as a knowledge base for the LLM AI.

- #### [Add function to get token count of text](https://github.com/georgesengono/git-ai/commit/88b78beb838388af81eef8b2eacaf30c879d4f6c)

- #### [Add sentence splitter](https://github.com/georgesengono/git-ai/commit/af2f77747286229e8b108c95248fb71846b7c5f0)

- #### [Add function to group sentences with overlap](https://github.com/georgesengono/git-ai/commit/c569bffe1591fb1f6e0489e99a750f8885617b07)

- #### [Add text splitter function](https://github.com/georgesengono/git-ai/commit/70f8fa13405f0e00365551fa453b82baf838127a)

I created the OpenAI wrapper class which will be used to interact with the OpenAI api and I implemented the GenerateEmbedding function to generate the embeddings of a given input text.

- #### [Add embedding generator](https://github.com/georgesengono/git-ai/commit/929bb1dab494028355405e3c9f598d026c49f046)