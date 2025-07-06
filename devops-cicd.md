# Complete DevOps CI/CD Pipeline Guide with AWS

## Welcome to 7 part DevOps build series! 

Over the next 7 days, you'll build a complete CI/CD pipeline that takes code from commit to production, gaining practical DevOps skills that are in high demand across the tech and cloud industry.

### By the end of this challenge, you'll have:

- ✅ Built a complete CI/CD pipeline using AWS tools
- ✅ Automated the testing and deployment of a real application
- ✅ Created 7 pieces of portfolio-worthy documentation that showcase your skills

## What to Expect

This is a 100% hands-on challenge. You'll be working in the AWS console from day one.

Don't worry if you're new to AWS or DevOps—we've designed this challenge to be beginner-friendly:

- 🤝 Step-by-step instructions guide you through each project
- 🎥 Live demos of every project on YouTube
- 📖 Clear explanations of DevOps concepts as we go
- 📝 Line-by-line explanations of any code (no coding experience required)
- 🔍 Troubleshooting tips to help you solve common errors


## How to Start

You'll build your CI/CD pipeline project over 7 parts. Set up an free AWS account before you start.

When you're ready, start this series by selecting the first part of the project!

1. **Set Up a Web App in the Cloud**
2. **Connect a GitHub Repo with AWS**
3. **Store Dependencies in CodeArtifact**
4. **Package an App with CodeBuild**
5. **Deploy an App with CodeDeploy**
6. **⭐️ Optional: Automate Your Infrastructure with CloudFormation**
7. **CI/CD with CodePipeline**

## What do DevOps and CI/CD mean, anyway?

Goooood question! To say thanks for making it to the end of this challenge intro, you've unlocked a bonus section (and bonus documentation) on what is CI/CD.

### What is DevOps?

DevOps is all about getting teams to build and deploy code faster without sacrificing reliability. Think of it as a blend of automating tasks, making sure apps are running consistently everywhere, and smart monitoring to catch issues early.

Before DevOps, development and operations teams worked separately with minimal communication. Developers would build software in their own environment, then hand it off to operations to deploy it to users.

But operations couldn't immediately deploy the code. They needed to carefully test it first, make sure it wouldn't break existing systems, and prepare the infrastructure - all without knowing how the code was written.

This created delays, misunderstandings, and frustration on both sides: developers wanted to ship features quickly, while operations wanted to make sure the code is stable before it's deployed.

To address this conflict, DevOps is a set of practices that combines development (writing code) and operations (deploying and maintaining code) to shorten development cycles and deliver reliable software.

#### 💡 If DevOps is a set of practices, what is a DevOps engineer?

DevOps engineers implement DevOps practices in their organization. They...

- Build automated pipelines (e.g. using AWS CodePipeline, Jenkins, GitHub Actions) that test, package, and deploy software, so releases become faster and more reliable
- Containerize applications using tools like Docker and Kubernetes, which helps applications scale
- Turn cloud infrastructure into code (e.g. AWS CloudFormation, Terraform), so teams can create and manage resources more efficiently than manually configuring them

### What is CI/CD?

Let's understand CI/CD by starting with something familiar - cooking!

In traditional cooking, there are chefs (developers) that come up with new recipes, and restaurant staff (operations) that follow the recipes and serve the dishes to customers (users).

Things can go wrong when the chef releases a new recipe (i.e. new code):

- 😦 The restaurant's pantry doesn't have the ingredients for the new recipe
- 😨 Some restaurant staff are still cooking with the old recipe
- 😰 The new recipe comes with special serving instructions that servers don't know about, so customers aren't getting the full experience

#### If there was CI/CD for cooking...

Now, let's imagine if there was a way to apply some automations to this process:
- The pantry automatically stocks all the required ingredients for the latest recipes
- Cooking equipment automatically cooks identical, consistent dishes based on the latest recipe
- Servers automatically know about the new serving instructions, and serve dishes to customers without any delays
- A restaurant manager automatically checks that everyone is following the instructions for the latest recipe of every dish

These automations make the flow from the chef's latest recipe to the customer's plate much more efficient and reliable!

#### In software development, CI/CD works similarly

The goal of CI/CD is to make the flow from code changes to a live application as efficient and reliable as possible. To do this, automations are set up across the software development and deployment process.

- **In project 1 - part 1**, you'll start by setting up a Development environment i.e. the test kitchen where developers write code
- **In project 1 - part 2**, you'll connect your development environment to GitHub i.e. the recipe book storing different versions of the code
- **In project 1 -  part 3**, you'll store dependencies in CodeArtifact i.e. the pantry for pre-built code your app needs to function
- **In project 1 - part 4**, you'll build your code with CodeBuild i.e. the cooking process that prepares code for deployment

💡 **CI (continuous integration)** means the build process starts automatically - whenever developers update the code in GitHub (i.e. a new recipe is added), CodeBuild will automatically build the updated code.

- **In project 1 - part 5**, you'll use CodeDeploy to deploy your code to a Web server/Live application i.e. servers that deliver code to users

💡 **CD (continuous delivery)** means deployment happens automatically whenever a new build is successful.

- **In project 1 - part 6**, we'll do a bonus project to learn how to use CloudFormation to turn your infrastructure into code
- **Finally, in project 1 - part 7**, you'll set up the CI/CD pipeline with CodePipeline i.e. the restaurant manager that makes sure code is automatically flowing from one stage to the next

## Overview

This guide will walk you through building a complete CI/CD (Continuous Integration/Continuous Deployment) pipeline using AWS tools that takes code from commit to production. You'll automate the testing and deployment of a real application while learning essential DevOps practices.

## Prerequisites

Before starting this guide, ensure you have:

- AWS account with appropriate permissions
- Basic knowledge of:
  - Git version control
  - Java/Maven (for the sample application)
  - Linux command line
  - Docker basics
- Code editor (VS Code, IntelliJ, etc.)

## Project Structure

```
devops-cicd-pipeline/
├── src/
│   ├── main/
│   │   ├── java/com/devopsdemo/
│   │   │   └── HelloServlet.java
│   │   └── webapp/
│   │       ├── index.jsp
│   │       └── WEB-INF/
│   │           └── web.xml
│   └── test/
│       └── java/com/devopsdemo/
│           └── HelloServletTest.java
├── pom.xml
├── deploy.sh
├── setup-ec2.sh
└── README.md
```

## CI/CD Pipeline Architecture

### Pipeline Flow Diagram

```
[Developer] → [Git Commit] → [CodePush] → [AWS CodeCommit]
                                    ↓
[CodeBuild] → [Unit Tests] → [Integration Tests] → [Build Artifact]
                                    ↓
[CodeDeploy] → [Deploy to Staging] → [Automated Testing] → [Deploy to Production]
                                    ↓
[CloudWatch] → [Monitoring & Logging] → [Alerting]
```

### Components Overview

1. **Source Control**: AWS CodeCommit
2. **Build Service**: AWS CodeBuild
3. **Deployment**: AWS CodeDeploy
4. **Infrastructure**: AWS EC2, VPC, Security Groups
5. **Monitoring**: AWS CloudWatch
6. **Container Registry**: AWS ECR (optional)
