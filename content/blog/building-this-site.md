+++
date = 2024-12-09T21:53:49-05:00
draft = false
title = 'Building This Site'
type = 'blog'
+++
# Hosting This Site: Technology Stack and Workflow

This site is hosted using a variety of technologies, each playing a specific role in creating a functional and efficient environment:

- **TrueNAS Scale** – *My primary NAS and hypervisor, utilizing ZFS for redundancy.*
- **Ansible** – *Defines the state of the server, providing a pseudo-CI/CD pipeline for this site.*
- **Hugo** – *Handles static site generation.*
- **Docker** – *Hosts the Nginx and Cloudflare Tunnel containers.*
- **Cloudflare** – *Provides DNS and HTTPS tunneling for ease and security.*
- **GitHub** – *All configurations are version-controlled in a private repository.*

---

## Why Use These Methods

There are easier ways to achieve what I’ve done here, but I intentionally chose a more complex tech stack to learn and practice new technologies. This setup doesn’t necessarily reflect enterprise practices—it’s tailored for personal growth and experimentation.

I wanted a basic site to showcase my résumé and personal projects, but I didn’t want to use traditional site builders like Wix or Squarespace. Those platforms are great for simplicity, but you don’t learn anything from them. This project was a chance to challenge myself.

---

## Evolution of the Setup

### Starting with a Homelab
I originally began with a homelab built on a series of laptops configured in a Proxmox cluster. However, I soon outgrew that setup due to storage limitations—my Google Photos space was running out, and I wasn’t interested in paying for more storage. This led me to build my own NAS, which gave me hands-on experience with the **ZFS filesystem** (vastly superior to traditional RAID) and migrating services to a new hypervisor.

### Moving to TrueNAS Scale
TrueNAS Scale became the backbone of my infrastructure, replacing the Proxmox cluster and simplifying my setup. While TrueNAS supports hosting VMs, one limitation I encountered was the lack of easy deployment options via Terraform or other Infrastructure-as-Code (IaC) tools. Instead, I manually built the VM for this site—just a few clicks to get an Ubuntu base VM up and running.

---

## Automating with Ansible

Once the VM was running, I leveraged **Ansible playbooks** to automate the initial setup:
- Creating the Ansible user.
- Updating the host.
- Defining folder structures and permissions for hosting the site’s files.
- Installing Docker and configuring Nginx and Cloudflare containers through Docker Compose.

I do not have a separate Ansible server for running playbooks, instead I was introduced to a way of containerizing Ansible. This approach ensures I can quickly rebuild my environment if necessary, even after a catastrophic event. The Ansible container, running on my Windows workstation via WSL, can be deployed to any system that is capable of running Docker and Make. 

---

## Building the Site with Hugo

The site itself is generated using **Hugo**, a static site generator. Hugo runs locally (though it can also be containerized, which wasn’t necessary for my use case). I utilize themes from GitHub—designed by professionals with far better artistic skills than me—to create the content and pages. 

The workflow allows me to:
1. Develop the site locally.
2. Version control changes in my homelab repository.
3. Deploy the updated site with a single button press.

---

## The Magic of Cloudflare

The final step to make the site accessible is Cloudflare. It serves as my DNS provider and leverages its **Tunnels** feature to:
- Manage site certificates.
- Provide additional security features.
- Offer the convenience of toggling settings via a mobile app.

These tunnels, managed through Ansible and containerized via Docker, add a layer of simplicity and peace of mind to the deployment process.

---

## Final Thoughts

This project is a testament to my desire to learn and grow as a technologist. While this stack may seem overly complicated for a simple personal site, it has given me valuable hands-on experience with:
- Hosting and hypervisor management.
- Automation with Ansible.
- Modern web deployment workflows.

If you’d like to dive deeper into this setup, feel free to ask me in our interview — I’d love to discuss it further!

 