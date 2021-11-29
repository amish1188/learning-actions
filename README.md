# Learning GitHub Actions
Welcome to this repository for learning GitHub Actions!


This repository contains a Function App project along with a unit test project. 
For this workshop, your task is to build GitHub Actions to automate developer tasks for the repository. 

## Pre requisites

1. A personal GitHub account. 
If you dont have one, [a GitHub account can be created for free](https://github.com/signup?ref_cta=Sign+up&ref_loc=header+logged+out&ref_page=%2F&source=header-home).

2. [.NET 6 SDK](https://dotnet.microsoft.com/download/dotnet/6.0)


## Task 1 - Fork the repository to your own GitHub account

In a tool of your choice clone the GitHub repository.

## Task 2 - Automating building and running tests on project

Getting started with GitHub actions is really easy. 
One of the reasons for this is that there are many workflows 
readily available for you, and GitHub even recommends suitable 
workflows based on the files in your repository.


**Lets try setting up a build and test workflow from a template!**
## :pencil2: Task 2.1 - up a workflow from template

A given workflow for a repository woul
### Step 1 - Go to the `Actions` tab for your repository.

  ![Marked Actions tab on page](imgs/find-actions-in-tab.PNG)


### Step 2 - Find the `.NET` workflow and select `Set up this workflow`.

  ![The .NET workflow illustration](imgs/dotnet-workflow.PNG)

A yml file containing the workflow is automatically generated for you. 

> :information_source: **Notice the path to the file `.github/workflows`**:All workflows must be placed in this directory to be picked up by GitHub.

### Step 3 - Set the correct .NET version

Change line 19 from
```yml
dotnet-version: 5.0.x
```
to 
```yml
dotnet-version: 6.0.x
```

### Step 4 - Reflection

 Study the workflow file. Can you identify the key components? 

  - Which events does the workflow listen after? 
  - The runner defined, is it a self-hosted agent or a GitHub runner?
  - Could you think of another runner that could have been used? [Available runners](https://docs.github.com/en/actions/using-github-hosted-runnersabout-github-hosted-runners#supported-runners-and-hardware-resources)


### Step 5 - Save the workflow

Commit the file as shown in the picture bewlow. Once completed, follow the workflow run from the `Actions` tab in GitHub.

  ![Commit details](imgs/start-commit.PNG)

## :pencil2: Task 2.2 - Customize the template workflow 

It is seldom the case that you can create a workflow based on a template 
and have it suit your needs out of the box. 

Let's go ahead and make some customizations for the workflow!

### Step 1 - Clone the repository to your local machine

Using your preferred tool, clone the forked repository to your local machine.
The following steps should be executed in a code editor, e.g. Visual Studio Code.

### Step 2 - Re-name the created yml file and workflow. 

When setting up workflows within larger projects, following a naming standard, allows the developers to quickly identify what a workflow is related to.

The template we have used has set the name of the workflow to `.NET` 
which isn't nessesarily very descriptive. 

The name of the workflow is declared in the first line of `dotnet.yml`.

**Give the workflow a reasonable name and rename the file.**

### Step 3 - Remove the PR event trigger for the workflow

As this is a workshop for you to learn GitHub Actions,
we do not expect any pull requests for this repository.

**Simplify your workflow file by removing the pull request event, but leaving the push event for main.** 

### Step 4 - Add a scheduled event trigger for the workflow

GitHub Workflows supports scheduled events, meaning that you 
can trigger your workflow based on a schedule. This could be useful if there are weekly or daily tasks such as patching your code or closing stale issues.

Read up on [how to set up a scheduled event trigger](https://docs.github.com/en/actions/learn-github-actions/events-that-trigger-workflows#scheduled-events). 

**Can you schedule this workflow to trigger at the top of every hour, Monday - Friday?**


## Task 3 - Automating developer workflows

A great benefit to GitHub Actions is that it does not only support 
automation of CI/CD workflows. Workflows in GitHub allow us to automate
time consuming and error prone developer workflows, such as

- issue and pull request labeling
- assignment of issues and/or pull request to developers
- creation of release notes
- patching of dependencies

In task 3 we will explore some of the developer workflows that can be 
automated.


## :pencil2: Task 3.1 

In projects with large code bases labels are often used for both issues and pull requests to identify which part of the solution they relate to. 

Below is an example of some issues from [Azure's repository azure-sdk-for-net](https://github.com/Azure/azure-sdk-for-net/issues). Notice the different labels assigned to each issue.

  ![Example of issue labeling in repository](imgs/labels-azure.png)


We will be setting up a workflow to automatically label pull requests based on the files that the PR modifies using an action from the GitHub Marketplace.


### Step 1 - Define the labels for the workflow to use

A prerequisite for the action we are using is a file that defines the labels and the files in the repository that each label applies to.

Create an empty file called `labeler.yml` in the `.github` folder
and copy the content below.

```yaml
area/test:
  - 'SimpleFunctionApp.Test/*'

area/automation:
  - '.github/**/*'

area/development:
  - 'SimpleFunctionApp/*'
```

The file defines three labels for the repository. Below you find a description of 
each label.

|Label            |Description                  |
|-----------------|-----------------------------|
|area/test        | Covers all changes under the `SimpleFunctionApp.Test` directory |
|area/automation  | Covers all changes under the `.github` directory  |
|area/development | Covers all changes under the `SimpleFunctionApp` directory  |
--------

**Can you think of another abel to add to the repository? **

### Step 2 - Set up a workflow for automatic labeling on PR

In the `.github/workflows` folder create a new file and name it ``pr-labeler.yaml`.
Copy the code below into the file.

```yaml
name: Pull Request Labeler
on: [pull_request_target]

jobs:
  label:

    runs-on: ubuntu-latest

    steps:
    - name: Labeler
      uses: [insert correct action]
      with:
        repo-token: "${{ secrets.GITHUB_TOKEN }}"
```

This defines an action that triggers on the event of a pull requests. 
The key word `pull_request_target` differs from `pull_request` in that
the workflow will run in the context of the base of the pull request, rather than the merge commit. 

A single job `label` running on an `ubuntu-latest` runner defines a single step `Labeler` using a not yet defined action. The action is given access to a secret from the repository.


### Step 3 - Complete the workflow file

The workflow copied in step 2 is not valid. The action to use is missing.

Find the correct action to insert from [Github Marketplace for Actions](https://github.com/marketplace?type=actions).

>:balloon: Hint: The action is published by `actions` and we are working on adding a `Label` to a pull request.





### Step 4 - Create a PR to test the new workflow

In GitHub, navigate to `SampleFunctionApp.Test\FunctionsTest.cs`.

Enable editing of the file by clicking the pencil icon

  ![Example of issue labeling in repository](imgs/edit-file-github.png)

Copy the code block below into the file.
```cs
[Fact]
 public async void HttpTriggerWithParams2()
 {
     var request = TestFactory.CreateHttpRequest("name", "Nancy");
     var response = (OkObjectResult)await Function1.Run(request, logger);
     Assert.Equal("Hello Nancy! Welcome to Azure Functions!", response.Value);
 }
```

Your file should look like this after adding the new test.

```cs
// ommited using statements

namespace SampleFunctionApp.Test
{
    public class Function1Test
    {
        private readonly ILogger logger = NullLoggerFactory.Instance.CreateLogger("Null Logger");

        [Fact]
        public async void HttpTriggerWithParams()
        {
            var request = TestFactory.CreateHttpRequest("name", "Bill");
            var response = (OkObjectResult)await Function1.Run(request, logger);
            Assert.Equal("Hello Bill! Welcome to Azure Functions!", response.Value);
        }
           
        [Fact]
         public async void HttpTriggerWithParams2()
         {
             var request = TestFactory.CreateHttpRequest("name", "Nancy");
             var response = (OkObjectResult)await Function1.Run(request, logger);
             Assert.Equal("Hello Nancy! Welcome to Azure Functions!", response.Value);
         }
        
        [Fact]
        public async void HttpTriggerWithoutParams()
        {
            var request = TestFactory.CreateHttpRequest("", "");
            var response = (OkObjectResult)await Function1.Run(request, logger);
            Assert.Equal("Hello there! Welcome to Azure Functions!", response.Value);
        }
    }
}

```

Make sure to give the change a descriptive name, select the `create a new branch`
option and complete the pull request creation.

  ![Create PR in GitHub](imgs/create-pr.PNG)


Useful links

https://github.com/marketplace/actions/send-email

https://docs.github.com/en/actions/learn-github-actions/events-that-trigger-workflows