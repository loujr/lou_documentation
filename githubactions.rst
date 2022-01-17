=================================================
Improving Your CI/CD Workflow with GitHub Actions
=================================================

Introduction:
-------------

This article outlines the use of GitHub Actions to improve CI/CD workflow. Further, this article is intended for System Administrators who are looking to improve and create their CI/CD pipeline or developers who might be new to GitHub Actions.

What are GitHub Actions?
------------------------

At its core, GitHub Actions is a series of automation tools that are integrated within GitHub to provide ready-to-use automation for your git repository. Actions work by listening to events, changes made within your repository, and triggering a workflow. This workflow is written in YAML and allows you to automate your CI/CD pipeline because steps such as building, testing, merging, and deployment can be validated automatically once changes have been made. 

The figure below illustrates a basic CI/CD Pipeline. Within this development workflow, automation through GitHub Actions can reduce the need for human intervention in the management of a CI/CD Pipeline and allow faster deployment into production.

.. image:: https://user-images.githubusercontent.com/61295275/146704016-48045b4e-1bdd-4aed-8756-c816a4113af2.jpeg


Advantages of Using GitHub Actions
----------------------------------

GitHub Actions utilize Virtual Machine runners (Windows, Mac, and Ubuntu) hosted within GitHub. This means that there is no need to maintain infrastructure for testing. Actions also integrate completely within the GitHub ecosystem and can be utilized within your current GitHub repository. Actions are also backed by GitHub which has an amazing `Support Community <https://github.community/c/code-to-cloud/github-actions>`_ as well as great `Documentation <https://docs.github.com/en/actions>`_. GitHub Actions can help you leverage automation to increase the effectiveness of your project and improve your CI/CD pipeline. 

From GitHub, community Actions have also made a positive impact on many individuals. In his article `Overcoming human error with code automation and testing <https://github.com/readme/guides/code-automation-and-testing>`_, Developer Colby Fayock had the following to say regarding GitHub Actions:

 "Actions give us the flexibility to take control of our automated workflows without much hassle, making it a great place for both beginners to automation and seasoned developers alike."

How To Get Started With GitHub Actions
--------------------------------------

To get started with GitHub Actions, check out the `Quickstart for GitHub Actions <https://docs.github.com/en/actions/quickstart>`_. This guide provides a five-minute tutorial to allow you to get started with the GitHub Actions workflow.

For community support, visit the `Support Community for GitHub Actions <https://github.community/c/code-to-cloud/github-actions>`_. 

Regarding the GitHub-hosted runners mentioned above, more information regarding the virtual machines that run workflows is found at `About GitHub-hosted Runners <https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners>`_.
