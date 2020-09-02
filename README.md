# awsworkshop

## Introduction

This is the AWS Workshops template that has been used for APAC based workshops across the Security, ML/AI and Public Sector events. Instrucitons below on how to use this to quickly deploy your content. 

The template has categories for:
- On Demand Videos
- Labs
- FAQ

# Deploy notes for Hugo based site

1) Fork/Duplicate the repo 
2) Log in to the AWS Amplify Console and choose Get Started under Deploy.
![](/images/amplify-gettingstarted.png)
3) Connect a branch from your GitHub, Bitbucket, GitLab, or AWS CodeCommit repository. Connecting your repository allows Amplify to deploy updates on every code commit to a branch.
![](/images/amplify-connect-repo.gif)
4) Accept the default build settings. The Amplify Console automatically detects your Hugo build settings and output directory.
![](/images/amplify-build-settings.png)

{{% notice note %}}
if Amplify does not automatically detect Hugo or you do not see `hugo` under `commands`, then add `hugo` in and also set the `baseDirectory:` to `public`
{{% /notice %}}

5) Review your changes and then choose Save and deploy. The Amplify Console will pull code from your repository, build changes to the backend and frontend, and deploy your build artifacts at https://master.unique-id.amplifyapp.com. 


# How to run locally
### On a Mac:
1. brew install hugo
1. in the repo's main directory, run: `hugo serve -D`
1. You should get a localhost:port to connect and see changes live.
