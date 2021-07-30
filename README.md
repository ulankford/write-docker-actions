
### Getting started

In this repository we will be diving into the world of writing GitHub Actions! I will guide you through the process of writing a custom Docker based GitHub Actions.

You may be asking yourself, "is Docker the only way to create custom GitHub Actions?"

Currently, there are **two** supported ways to create your own GitHub Actions:

- [Docker container actions](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/about-actions#docker-container-actions)
- [JavaScript actions](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/about-actions#javascript-actions)

As you can see we aren't necessarily limited to Docker even though it is the focal point for this course.

---

### Creating vs consuming actions

Although we can focus on creating and consuming a custom action, we will also be consuming some actions that have been made public to us. Because your workflows will most likely do the same.

After all, for each time we need to reinvent the wheel for our specific use-case there are a handful of times when we are better off using a wheel that's already made!

- The [GitHub Actions Marketplace](https://github.com/marketplace?type=actions) is the primary place to find open-source actions that the community has written and released. Your action, should you choose to release it, could also reside here one day, ready to be consumed by the world!
- The [GitHub Actions Repository](https://github.com/actions) is where you can find actions that are written by GitHub. We will leverage an action named `[checkout](https://github.com/actions/checkout)` from this repository as we go through this course. I'll explain more about what it does when we use it!
- Your repositories may also contain **private actions** and they will most likely be located in the `.github/actions` directory in the root of your repository. **This is the convention we will be using as we learn how to create our own action.**

---

