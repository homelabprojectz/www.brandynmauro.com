+++
date = '2025-01-07T22:49:51-05:00'
draft = false
title = 'Implementing Github Actions'
+++
# Implementing GitHub Actions for Faster Deployments

One of the key challenges I faced during this deployment was the slow file transfer process via Ansible. Previously, I used Ansible to copy all the HTML, CSS, and other assets to the server's Nginx hosting directory. This method proved to be extremely slow and inefficient.

To address this, I transitioned to using a **GitHub self-hosted runner**. This approach deploys site updates automatically whenever there is a push to the `main` branch.

![Workflow Success](images/blog/cicd/workflowsuccess.png)

## Simplifying the Process

Initially, I overcomplicated the deployment process in my mind, resulting in **29 failed workflows** before achieving success. I was overthinking the necessary steps, which led to unnecessary complexity. Once I simplified my approach, everything fell into place.

The self-hosted runner is installed on the same host as my Docker containers, reducing resource overhead. With this setup, the workflow now involves a straightforward series of actions:
1. Check out the repository to the local host.
2. Copy the site's directory to the directory used by my Nginx container.

![Action Code](images/blog/cicd/actioncode.png)

## Overcoming Deployment Issues

During initial testing after trimming down 90% of the actions code, my deployments would fail to complete. The root cause turned out to be my old, slow Ansible method, which introduced file permission issues.

Ansible was running as `root` to copy the site files, setting the file owner to `root`. Consequently, when GitHub Actions attempted to run, they encountered permission errors and failed.

![Permission Denied](images/blog/cicd/permissiondeny.png)

After adjusting the permissions on the target directory, GitHub Actions now executes as a user with the appropriate permissions to modify the directory. With this fix in place, the workflow runs smoothly and efficiently.

---

This experience taught me the value of keeping things simple and ensuring that all components of the deployment pipeline are configured correctly. If you’re facing similar challenges, consider switching to GitHub Actions with a self-hosted runner—it’s been a game changer for me.
