# Learning GitHub Actions
Welcome to this repository for learning GitHub Actions!


This repository contains a Function App project along with a unit test project. 
For this workshop, your task is to build GitHub Actions to automate developer tasks for the repository. 

## Pre requisites

1. A personal GitHub account. 
If you dont have one, [a GitHub account can be created for free](https://github.com/signup?ref_cta=Sign+up&ref_loc=header+logged+out&ref_page=%2F&source=header-home).

2. [.NET 6 SDK](https://dotnet.microsoft.com/download/dotnet/6.0)

## Task 1 - Clone the repository to your own GitHub account
In a tool of your choice clone the github

## Task 2 - Automating building and running tests on project

Getting started with GitHub actions is really easy. 
One of the reasons for this is that there are many workflows 
readily available for you, and GitHub even recommends suitable 
workflows based on the files in your repository.

## Task 2.1 - Set up a workflow from template

1. Go to the `Actions` tab for your repository.

![Marked Actions tab on page](imgs/find-actions-in-tab.PNG)


2. Find the `.NET` workflow and select `Set up this workflow`.

![The .NET workflow illustration](imgs/dotnet-workflow.PNG)

A yml file containing the workflow is automatically generated for you. 
Notice the path to the file `.github/workflows`. 
All workflows must be places in this directory to be picked up by GitHub.

3. Change line 19 from

```yml
dotnet-version: 5.0.x
```

to 

```yml
dotnet-version: 6.0.x
```

4. Study the workflow file. Can you identify the key components? 

  - Which events does the workflow listen after? 
  - The runner defined, is it a self-hosted agent or a GitHub runner?
  - Could you think of another runner that could have been used? [Available runners](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners#supported-runners-and-hardware-resources)


5. Commit the file to the repository 
![Commit details](imgs/start-commit.PNG)

6. Follow the workflow run from the `Actions` tab.

## Task 2.2 - Customize the template workflow 

1. Re-name the created yml file to `build-and-test.yml`. 
2. 



https://github.com/marketplace/actions/send-email