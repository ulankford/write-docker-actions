# Welcome to your course 🎉

### Getting started

In this repository we will be diving into the world of writing GitHub Actions! I will guide you through the process of writing a custom Docker based GitHub Actions.

You may be asking yourself, "is Docker the only way to create custom GitHub Actions?"

Currently, there are **two** supported ways to create your own GitHub Actions:

- [Docker container actions](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/about-actions#docker-container-actions)
- [JavaScript actions](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/about-actions#javascript-actions)

As you can see we aren't necessarily limited to Docker even though it is the focal point for this course.

---

### Creating vs consuming actions

Although we are going to focus on creating and consuming a custom action, in this course we will also be consuming some actions that have been made public to us. Because your workflows will most likely do the same, I found it important to show you where to look for actions that already exist.

After all, for each time we need to reinvent the wheel for our specific use-case there are a handful of times when we are better off using a wheel that's already made!

- The [GitHub Actions Marketplace](https://github.com/marketplace?type=actions) is the primary place to find open-source actions that the community has written and released. Your action, should you choose to release it, could also reside here one day, ready to be consumed by the world!
- The [GitHub Actions Repository](https://github.com/actions) is where you can find actions that are written by GitHub. We will leverage an action named `[checkout](https://github.com/actions/checkout)` from this repository as we go through this course. I'll explain more about what it does when we use it!
- Your repositories may also contain **private actions** and they will most likely be located in the `.github/actions` directory in the root of your repository. **This is the convention we will be using as we learn how to create our own action.**

---

### Using actions and Learning Lab

In other courses, you may have noticed that some behaviors take me longer to respond to than others. In this course, many of the behaviors we'll see demonstrated will be related to our GitHub Actions workflow. Those workflows sometimes take longer to complete, up to several minutes. Don't be concerned if I take a few minutes to respond, or if I respond too quickly. Sometimes, I'll let you know what the workflow will say before it finishes! Please wait for the workflows to finish before moving on to your next step.

If you aren't already familiar, it may be a good idea to go through the [Introduction to GitHub Learning Lab](https://lab.github.com/githubtraining/introduction-to-github).

---

Why use Docker when writing GitHub Actions?
That's a super great question to ask. Before we talk about the components that make up Docker based actions, we should understand the good and the bad of Docker based actions.

Advantages	Disadvantages
Docker actions package the source code that will be executed right alongside any dependencies that source code has.	Slower execution time because the image containing your source code and dependencies gets built with every workflow run.
Docker containers package the environment with the GitHub Actions code. This creates a more consistent and reliable unit of work because the consumer of the action does not need to worry about the tools or dependencies.	Docker container actions can only execute in the GitHub-hosted Linux environment.
Ideal for running in environments with very specific configurations and tools.	Debugging can be difficult with containers.
Actions can be written in any language you choose.	More files to maintain when changes to the action occur.

an action is the unit of work that a workflow file executes when it reaches that task. They are called by referencing them as the value to the uses: key in a workflow step.

What makes up an action?
Docker actions consist of three key components:

Component	Description
Action source code files	These files contain the logic of your action. This includes any dependencies or custom modules that your main logic may need.
Action metadata file	This file contains information that the actions source code can use. An example of this is allowing a developer to specify an API key as an input variable for your action to consume. This file MUST be named action.yml
Dockerfile	This file defines the environment and dependencies for the action. It is best practice that this file be named Dockerfile

Let's take a look at how those components fit together.

**The workflow**, usually workflow.yml, is the user facing component. It must be housed in .github/workflows/ and doesn't need to be called workflow.yml, but its essence is a workflow. This is what we interact with directly when we want to supply input, or consume the output of a given action. You can think of workflow.yml as a user-interface for the action.yml file.

**The action's metadata, called** action.yml, is the interface between the user and the source code of the action. It is housed in a directory just for the action. This file may contain default values for inputs and outputs that the source code relies on. Those values can be overwritten by the workflow.yml file, but their default values live here and allow the action to execute properly if no values are supplied. This file also defines a high-level environment for the source code, such as whether to use Docker or Node.js and the execution entry point.

**The action's source code** files define the logic of an action. It is housed in a directory just for the action. The user doesn't interact with these files directly. There could be as little as one source code file, but there is no hard rule on how modular an action can be!

**Dockerfile** is only needed when creating Docker based actions and it is responsible for packaging up the environment as well as the source code for the given action. It is housed in a directory just for the action. This file is read and a Docker image is created from it. Once the image is created a container is created from the image and the action is executed.
