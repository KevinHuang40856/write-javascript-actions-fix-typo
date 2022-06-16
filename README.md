<!--
  <<< Author notes: Header of the course >>>
  Include a 1280×640 image, course name in sentence case, and a concise description in emphasis.
  In your repository settings: enable template repository, add your 1280×640 social image, auto delete head branches.
  Next to "About", add description & tags; disable releases, packages, & environments.
  Add your open source license, GitHub uses Creative Commons Attribution 4.0 International.
-->

<img src="https://repository-images.githubusercontent.com/225716723/812b4e80-586d-11ea-88cb-74a437c5dc3b" width=300 align=right>

# GitHub Actions: Writing JavaScript Actions

_Learn how to write your own GitHub JavaScript Action!  This course will empower you to begin automating customized tasks unique to your workflow._

<!--
  <<< Author notes: Start of the course >>>
  Include start button, a note about Actions minutes,
  and tell the learner why they should take the course.
  Each step should be wrapped in <details>/<summary>, with an `id` set.
  The start <details> should have `open` as well.
  Do not use quotes on the <details> tag attributes.
-->

<details id=0 open>
<summary><strong>:golf: Start</strong></summary>

**To start this course: [<img width="150" alt="Use this template" src="https://user-images.githubusercontent.com/1221423/148581131-555c0fb8-5361-4450-a760-75fa6219a2fc.png">](https://github.com/InfomagnusOrg/github-actions-writing-javascript-actions/generate)**

> We recommend creating a public repository, as private repositories will [use Actions minutes](https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions).<br>
> After you make your own repository, wait about 20 seconds and refresh. I will go to the next step.

Over the duration of this course you will learn the skills needed to begin using and customizing GitHub Actions to fit your unique workflow scenarios.

- **Who is this for**: Developers, GitHub users, users new to Git, students, managers, teams.
- **What you'll learn**: 
  - Consume actions within a workflow file
  - Create custom JavaScript based actions
  - Publish your newly created action to the marketplace
  
- **Prerequisites**: Before you take this course, you may want to go through the following courses:
  taking the following courses:
- [Hello, GitHub Actions!](https://lab.github.com/github/hello-github-actions!) to learn the basics of how GitHub Actions work
- [GitHub Actions: Continuous Integration](https://lab.github.com/githubtraining/github-actions:-continuous-integration) to dive deeper into a workflow file
  
## Projects used

This makes use of the following open source projects. Consider exploring these repos and maybe even making contributions!

- [GitHub Actions Toolkit](https://github.com/actions/toolkit), a multipurpose JavaScript library for writing actions

</details>

<!--
  <<< Author notes: Step 1 >>>
  Choose 3-5 steps for your course.
  The first step is always the hardest, so pick something easy!
  Link to docs.github.com for further explanations.
  Encourage users to open new tabs for steps!
  TBD-step-1-notes.
-->
### Welcome to this Learning Lab course about Actions where you will build the following:

- ![screenshot of a pull request in the course with instructions on how to fetch a joke from the API, a second screenshot of a workflow running and outputting the joke: "Guy told me today he did not know what cloning is. I told him, that makes 2 of us."](https://user-images.githubusercontent.com/16547949/76105870-cce3a380-5fa3-11ea-8882-7138319b4100.png)

  - In this course you will build three actions that each accomplish different tasks designed to demonstrate the flexibility of creating and consuming JavaScript Based Actions.

  - First, you will start with the traditional "Hello World!" program which will teach you where to find the output of a workflow run. You will then modify this "Hello World!" action to accept `input` parameters which allow the action to be more dynamic. 

  - Second, you will write an action that call upon an external API to retrieve a fact about cats and prints it to the workflows output. You will then modify this cat fact action to set the retrieved data as `output` for another action in the workflow to consume.

  - Lastly, you will write a third action that will open an issue in your repository making the cat fact available to everyone. You will learn how to use the `output` of previous actions as `input` for current actions in this step.

### Configuring a workflow

Actions are enabled on your repository by default, but we still have to tell our repository to use them. We do this by creating a workflow file in our repository.

A **workflow** file can be thought of as the recipe for automating a task. They house the start to finish instructions, in the form of `jobs` and `steps`, for what should happen based on specific triggers.

Your repository can contain multiple **workflow** files that carry out a wide variety of tasks. It is important to consider this when deciding on a name for your **workflow**. The name you choose should reflect the tasks being performed.

_In our case, we will use this one **workflow** file for many things, which leads us to break this convention for teaching purposes._

📖Read more about [workflows](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/configuring-a-workflow#choosing-the-type-of-actions-for-your-workflow)

<details id=1 closed>
<summary><strong>:zap: Step 1: Initialize a new JavaScript project</strong></summary>

## On to your development environment

Our JavaScript actions are going to leverage the [GitHub ToolKit](https://github.com/actions/toolkit) for developing GitHub Actions.

This is an external library that we will install using `npm` which means that you will need [Node.js](https://nodejs.org/) installed.

I find writing actions to be easier from a local environment vs trying to do everything right here in the repository. Doing these steps locally allows you to use the editor of your choice so that you have all the extensions and snippets you are used to when writing code.

If you do not have a preferred environment then I suggest following along with me exactly as you see on the screen, which means you'll need to install [Visual Studio Code](https://code.visualstudio.com/).

## Don't forget to set up your workstation 😉

Most of your work going forward will take place away from your Learning Lab repository, so before continuing with the course ensure you have the following installed on your **local machine**.

1. [ ] [Node.js](https://nodejs.org)
2. [ ] [Visual Studio Code](https://code.visualstudio.com/) or your editor of choice
3. [ ] [Git](https://git-scm.com/)
  
### :keyboard: Activity: Initialize a new JavaScript project

Once you have the necessary tools installed locally, follow these steps to begin creating your first action.

1. Open the **Terminal** (Mac and Linux) or **Command Prompt** (Windows) on your local machine
2. Clone your Learning Lab repo to your local machine:
   ```shell
   git clone <this repository URL>.git
   ```
3. Navigate to the folder you just cloned:
   ```shell
   cd <local folder with cloned repo>
   ```
4. We are using branch called `main`. 
   ```shell
   git switch main
   ```
5. Create a new folder for our actions files:
   ```shell
   mkdir -p .github/actions/joke-action
   ```
6. Navigate to the `joke-action` folder you just created:
   ```shell
   cd .github/actions/joke-action
   ```
7. Initialize a new project:
   ```shell
   npm init -y
   ```
8. Install the **request**, **request-promise** and **@actions/core** dependencies using `npm` from the [GitHub ToolKit (https://github.com/actions/toolkit):
   ```shell
   npm install --save request request-promise @actions/core
   ```
9. Commit those newly added files,we will remove the need to upload **node_modules** in a later step:
   ```shell
   git add .
   git commit -m 'add project dependencies'
   ```
10. Push your changes to your repository:
    ```shell
    git push
    ```
11. Wait about 20 seconds then refresh this page for the next step.

</details>
  
<details id=2 closed>
<summary><strong>:zap: Step 2: Configure Your Action</strong></summary>

### Excellent!
  
Now that we have the custom action pre-requisites, let is create **joke-action** action.

### :keyboard: Activity: Configure Your Action

💡All of the following steps take place inside of the `.github/actions/joke-action` directory.

We will start with using the parameters that are **required** and later implement some optional parameters as our action evolves.

1. Create a new file in: `.github/actions/joke-action/action.yml`
2. Add the following contents to the `.github/actions/joke-action/action.yml` file:
   ```yaml
   name: "my joke action"

   description: "use an external API to retrieve and display a joke"

   runs:
     using: "node12"
     main: "main.js"
   ```
3. Save the `action.yml` file
4. Commit the changes and push them to the `main` branch:
   ```shell
   git add action.yml
   git commit -m 'create action.yml'
   git push
   ```
5. Wait about 20 seconds then refresh this page for the next step.

</details>
  
<details id=3 closed>
<summary><strong>:zap: Step 3: Create the metadata file</strong></summary>

## Action metadata

Every GitHub Action that we write needs to be accompanied by a metadata file. This file has a few rules to it, lets outline those now:

- Filename **must** be `action.yml`
- Required for both Docker container and JavaScript actions
- Written in YAML syntax

This file defines the following information about your action:

| Parameter   | Description                                                                                                                                            |      Required      |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ | :----------------: |
| Name        | The name of your action. Helps visually identify the actions in a job.                                                                                 | :white_check_mark: |
| Description | A summary of what your action does.                                                                                                                    | :white_check_mark: |
| Inputs      | Input parameters allow you to specify data that the action expects to use during runtime. These parameters become environment variables in the runner. |         ❌         |
| Outputs     | Specifies the data that subsequent actions can use later in the workflow after the action that defines these outputs has run.                          |         ❌         |
| Runs        | The command to run when the action executes.                                                                                                           | :white_check_mark: |
| Branding    | You can use a color and Feather icon to create a badge to personalize and distinguish your action in GitHub Marketplace.                               |         ❌         |

---

📖Read more about [Action metadata](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/metadata-syntax-for-github-actions)

### :keyboard: Activity: Create the metadata file
  
💡All of the following steps take place inside of the `.github/actions/joke-action` directory.

Our action does not require much metadata for it to run correctly. We will not be accepting any inputs, we will however be setting a single output this time.

1. Update the action metadata file `.github/actions/joke-action/action.yml` with the following content:
   ```yaml
   name: "my joke action"

   description: "use an external API to retrieve and display a joke"

   outputs:
     joke-output:
       description: The resulting joke from the icanhazdadjokes API

   runs:
     using: "node12"
     main: "main.js"
   ```
2. Save the `action.yml` file
3. Commit the changes and push them to GitHub:
   ```shell
   git add action.yml
   git commit -m 'add metadata for the joke action'
   git push
   ```
4. Wait about 20 seconds then refresh this page for the next step.

</details>
  
<details id=4 closed>
<summary><strong>:zap: Step 4: Create the JavaScript files for your action</strong></summary>

## Files? 🤔

Yes... files... plural. As you probably know, in JavaScript and other programming languages it is common to break your code into modules so that it is easier to read and maintain going forward. Since JavaScript actions are just programs written in JavaScript that run based on a specific trigger we are able to make our action code modular as well.

To do so we will create two files. One of them will contain the logic to reach out to an external API and retrieve a joke for us, the other will call that module and print the joke to the actions console for us. We will be extending this functionality in our third and final action.

### Fetching a joke

**Joke API**

The first file will be `joke.js` and it will fetch our joke for us. We will be using the [icanhazdadjoke API](https://icanhazdadjoke.com/api) for our action. This API does not require any authentication, but it does however that we set a few parameters in the [HTTP headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers). I'll point out what those are when we get to the code, however it is outside of the scope of this course to cover HTTP in any depth.

When we make our request to this API we will get back a JSON Object in the response. That Object looks like this:

```
{
  id: '0LuXvkq4Muc',
  joke: "I knew I shouldn't steal a mixer from work, but it was a whisk I was willing to take.",
  status: 200
}
```

It contains 3 key:value pairs of data that we can use in our own program or service. In our case, we are only interested in the `joke` field.

**Joke Module**

We will create a file named `joke.js` and it will reside in the `.github/action/joke-action` directory.

The joke module will look like this:

```javascript
const request = require("request-promise");

const options = {
  method: "GET",
  uri: "https://icanhazdadjoke.com/",
  headers: {
    Accept: "application/json",
    "User-Agent":
      "Writing JavaScript action GitHub Learning Lab course.  Visit lab.github.com or to contact us."
  },
  json: true
};

async function getJoke() {
  const res = await request(options);
  return res.joke;
}

module.exports = getJoke;
```

<details><summary>Need an advanced description of the <code>joke.js</code> source code?</summary>
We first bring in the `request-promise` library that we installed earlier using `npm`.

Next we define a set of `options` that the `request-promise` library will use when it makes the request.

📖Read more about [request-promise](https://github.com/request/request-promise/)

Inside of the `options` block we add a key named `headers`. This defines the HTTP headers that the **icanhazdadjoke** API expects in each request that comes it's way.

**icanhazdadjoke** cares the most about the keys, **Accept** and **User-Agent**, so we need to make sure we fill them in.

Next we define an **asynchronous JavaScript function** to make the request for us, storing the JSON Object that is returned in a variable named `res`.

Lastly, we `return` the `res.joke` which is only the value associated with the `joke` key of the JSON Object. This value will be random every time our action runs because of how we are interacting with the **icanhazdadjoke** API.

This file finishes up by exporting the newly created function so that we can use it in our `main.js` file.
  
</details>

### Creating the main entry point for your action

**Main Module**

We will also create a file named `main.js` that resides inside of the `.github/actions/joke-action` directory.

That file will look like this:

```javascript
const getJoke = require("./joke");
const core = require("@actions/core");

async function run() {
  const joke = await getJoke();
  console.log(joke);
  core.setOutput("joke-output", joke);
}

run();
```

<details><summary>Need an advanced description of the <code>main.js</code> source code?</summary>
Like we did in the `joke.js` file, we are first going to bring in our dependencies. Only this time, our dependencies include something we wrote! To do that we simply use `require()` to point to the location of the file we wish to bring in.

We also bring in `@actions/core` so that we can set the output of our action.

Next we write another **asynchronous JavaScript function** that stores the return value of `getJoke()` in a variable called **joke**.

Then we log the joke to the console.

Finally we finish the function with by setting the contents of the joke as the value of the `joke-output` output parameter. We will use this output later in the course.
_Don't forget to call the `run()` function._

</details>
  
### :keyboard: Activity: Creating the JavaScript files for your new action.

1. Create and add the following contents to the `.github/actions/joke-action/joke.js` file:

   ```javascript
   const request = require("request-promise");

   const options = {
     method: "GET",
     uri: "https://icanhazdadjoke.com/",
     headers: {
       Accept: "application/json",
       "User-Agent":
         "Writing JavaScript action GitHub Learning Lab course.  Visit lab.github.com or to contact us."
     },
     json: true
   };

   async function getJoke() {
     const res = await request(options);
     return res.joke;
   }

   module.exports = getJoke;
   ```

2. Save the `joke.js` file.
3. Create and add the following contents to the `.github/actions/joke-action/main.js` file:

   ```javascript
   const getJoke = require("./joke");
   const core = require("@actions/core");

   async function run() {
     const joke = await getJoke();
     console.log(joke);
     core.setOutput("joke-output", joke);
   }

   run();
   ```

4. Save the `main.js` file.
5. Commit the changes to this branch and push them to GitHub:
   ```shell
   git add joke.js main.js
   git commit -m 'creating joke.js and main.js'
   git push
   ```

</details>
  
<details id=5 closed>
<summary><strong>:zap: Step 5: Add your action to the workflow file</strong></summary>

### Great job!
  
💡All of the following steps will add the action to the workflow file that’s already in the repo [`my-workflow.yml` file](/.github/workflows/my-workflow.yml)
  
### :keyboard: Activity: Edit the custom action at the bottom of the workflow file.
  
Activity: Have the leaner add the following to the bottom of their workflow file:
```yaml
   - name: ha-ha
     uses: ./.github/actions/joke-action
```
Here is what the full file should look like (we’re using issues instead of the pull request event  and removing the reference to the hello world action. 
```yaml
- name: JS Actions

on:
  issues:
    types: [labeled]

jobs:
  action:
     runs-on: ubuntu-latest

     steps:
       - uses: actions/checkout@v3

     	 - name: ha-ha
         uses: ./.github/actions/joke-action
```
</details>
  
<details id=6 closed>
<summary><strong>:zap: Step 6: Trigger the joke action</strong></summary>

### Great job! 
Everything is all set up and now we are ready to start laughing 🤣. You will find you have some joke related labels available to you in this repository. You don't have to use them, any label will trigger our workflow, but it might be easier to follow along with me if you use the labels I suggest.

### :keyboard: Trigger a joke

1. Create a new issue in "Issues tab" and save it
2. Apply the `first-joke` label to the issue
3. Wait a few seconds and then apply the `second-joke` label to the issue
4. Check the workflow results on the "Actions tab"
 
</details>

<details id=7 closed>
<summary><strong>:checkered_flag: Finish</strong></summary>

### Congratulations friend, you've completed this course! :tada:

In this course, you've learned a lot about developing custom actions using JavaScript and Actions Toolki.

## Publishing your actions

Publishing your actions is a great way to help others in your team and across the GitHub community. Although actions do not need to be published to be consumed by adding them to the marketplace you make them easier to find.

Some notable actions you will find on the marketplace are:

- [Actions for Discord](https://github.com/marketplace/actions/actions-for-discord)
- [GitHub Action for Slack](https://github.com/marketplace/actions/github-action-for-slack)
- [Jekyll action](https://github.com/marketplace/actions/jekyll-action)
- [Run Jest](https://github.com/marketplace/actions/run-jest)

And that just scratches the surface of the 1600+ and counting actions you will find on the marketplace 😄

📖Follow [this guide](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/publishing-actions-in-github-marketplace#publishing-an-action) to learn how to publish your actions to the GitHub Marketplace
  
### What's next?

- We'd love to hear what you thought of this course [in our community forum](https://github.community/c/education/github-learning-lab/34).
- [Take another GitHub Learn course](https://github.com/githublearn).
- [Read the GitHub Getting Started docs](https://docs.github.com/en/get-started).
- To find projects to contribute to, check out [GitHub Explore](https://github.com/explore).

</details>

---

Get help: [Post in our community forum](https://github.community/c/education/github-learning-lab/34) &bull; [Review the GitHub status page](https://www.githubstatus.com/)

&copy; 2022 GitHub &bull; [Code of Conduct](https://www.contributor-covenant.org/version/2/1/code_of_conduct/code_of_conduct.md) &bull; [CC-BY-4.0 License](https://creativecommons.org/licenses/by/4.0/legalcode)
