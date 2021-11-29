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
## Task 2.1 - up a workflow from template

### Step 1 - Go to the `Actions` tab for your repository.

    ![Marked Actions tab on page](imgs/find-actions-in-tab.PNG)


### Step 2 - Find the `.NET` workflow and select `Set up this workflow`.

    ![The .NET workflow illustration](imgs/dotnet-workflow.PNG)

    A yml file containing the workflow is automatically generated for you. 

    > :information_source: **Notice the path to the file `.github/workflows`**: All workflows must be placed in this directory to be picked up by GitHub.

### Step 3 - Change line 19 from

    ```yml
    dotnet-version: 5.0.x
    ```

    to 

    ```yml
    dotnet-version: 6.0.x
    ```

### Reflection - Study the workflow file. Can you identify the key components? 

    - Which events does the workflow listen after? 
    - The runner defined, is it a self-hosted agent or a GitHub runner?
    - Could you think of another runner that could have been used? [Available runners](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners#supported-runners-and-hardware-resources)


### Step 4 - Commit the file to the repository 

    ![Commit details](imgs/start-commit.PNG)

### Step 5 - Follow the workflow run from the `Actions` tab.

## Task 2.2 - Customize the template workflow 

It is seldom the case that you can create a workflow based on a template 
and have it suit your needs out of the box. 

Let's go ahead and make some customizations for the workflow!

### Step 1 - Clone the repository to your local machine

Using your preferred tool, clone the forked repository to your local machine.
The following steps should be executed in a code editor, e.g. Visual Studio Code.

### Step 2 - Re-name the created yml file. 

When working with workflows in larger projects, following a naming standard to quickly identify what a workflow is related to could be useful.

Give your existing workflow a reasonable name.

### Step 3 - Remove the PR event trigger for the workflow

As this is a workshop for you to learn GitHub Actions,
we do not expect any pull requests for this repository.

### Step 4 - Add a scheduled event trigger for the workflow

GitHub Workflows supports scheduled events. 

[Here you find documentation](https://docs.github.com/en/actions/learn-github-actions/events-that-trigger-workflows#scheduled-events) on the syntax of setting up a scheduled event.

Can you schedule this workflow to trigger at the top of every hour, Monday - Friday? 





Useful links

https://github.com/marketplace/actions/send-email

https://docs.github.com/en/actions/learn-github-actions/events-that-trigger-workflows