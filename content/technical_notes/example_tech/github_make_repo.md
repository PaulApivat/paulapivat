---
date: "2021-07-21T00:00:00+01:00"
draft: false
linktitle: Creating a Github Repo
menu:
  example_tech:
    parent: Git & GitHub
    weight: 1
title: Creating a Github Repo
toc: true
type: docs
weight: 1
---

## Guide for Creating a GitHub Repo and Cloning Locally

This guide can help you setup a fresh new GitHub Repo, then clone it locally to begin working in your local environment, then pushing changes up to your GitHub Repo. 

This guide assumes you have a Text Editor like VSCode installed and that you can use the **Terminal** to navigate through various folders. 

## Creating Repo

1. Sign-up for a GitHub Account
2. Navigate to Your Profile
3. Click on Repositories 
4. Click on green button "New" (for New Repo)
5. In the Repository Name field, give your new repo a name*

Names should have no spaces, use underscore _ to separate words (ie., special_project)

6. Description is optional
7. Default is Public (a Private Repo requires a monthly fee)
8. Tick Add a README and Choose license (if applicable, if not just go with a README)
9. A .gitignore file is recommended if your project has sensitive info associated with it that you'd like to keep OUT of GitHub

10. Click "Create Repository"


## Cloning Locally

11. Navigate to your newly created repository (still on GitHub)
12. Click on the Green Button "Code"
13. Select the HTTPS (default) option and **copy** the https://github.com/YourName/special_project

Once you've copied the `https` address, you'll use the **terminal** in your text editor (i.e., VSCode) to navigate to a folder on your local machine (desktop or laptop) where you've *already* created a folder just for coding projects (i.e., coding_projects)

14. Open the text editor (i.e., VS Code), click on Terminal in the menu bar, "New Terminal", this will open your terminal to have you start at a level *above* the Desktop. Suppose you have a folder *on* your Desktop that says "coding_projects" you'll want to **clone** your newly-created github repo *in* this folder. But you have to navigate to *coding_projects* folder first. Here's the command to navigate in Terminal:

`cd Desktop/coding_projects`   

15. `cd` means change directory; here, you'll change directory into your Desktop, then into coding_projects folder (two steps) to **clone** your github repository (special_project)

16. (in Terminal), you're now inside the `coding_projects` folder, you'll run this command, pasting the `https` address that you copied from your GitHub Repo:

`git clone https://github.com/YourName/special_project`

This will initialize your repo locally. From here, try making a minor change to the README file. 

17. Make a small change to your README file on your Text Editor, then save the changes. 

18. Back in Terminal, you'll add the changes, then commit, then push back up to GitHub. You'll run these commands:

`git status`   (this should indicate in red that you have new changes)
`git add .`    (you'll add those changes)
`git push`     (you're pushing those changes to your GitHub Repo)

**NOTE**: you might see a prompt to do `git push --upstream origin`, which happens if this is the first push you're making to the repo. 

Now you've cloned your repo locally, made some changes, then pushed it back up to GitHub. Its important to **note** that the process described here is to allow you to track changes for your personal coding projects, **NOT** any code for production or development environment. 

In those cases, the team may have it's own protocol for version control and you'll want to get familiar and follow those protocols.  







