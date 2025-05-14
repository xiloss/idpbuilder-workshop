# 🏗️ Analogy: Opening a New Restaurant in a Smart City

Imagine you're in a futuristic smart city, and you want to open a new restaurant (let’s say it's called “Nginx Diner”).

Normally, opening a restaurant would take weeks — you'd need to register the business, hire staff, build the place, set up the kitchen, register it in delivery apps, and let the city know it exists.

But now, thanks to automation, you just press one button, and this happens:

You’re already using a **great restaurant analogy**.
Let’s smoothly integrate the **“how to get secrets”** part into the same storyline.

Here’s a **suggested extension** for your analogy, fully aligned with your current style:

---

## 🔑 Getting the Keys to the Restaurant (Secrets Retrieval)

Even in a smart city, a restaurant can’t open its doors without the keys!
The “keys” represent sensitive information like passwords, API tokens, or certificates — what we call **secrets**.

To prevent random people from duplicating keys or entering private areas (databases, APIs), the city provides a **secure vault** service.

In our scenario, the city vault is integrated into the restaurant automation system.
You (the owner) can simply run:

```
./idpbuilder get secrets
```

This command securely hands you the **master keys** (such as `USER_PASSWORD` for Backstage or `KEYCLOAK_ADMIN_PASSWORD` for Keycloak).

Think of it like a smart key dispenser: it checks your identity and then gives you a set of tightly controlled keys for:

* 🔐 **Admin access to Keycloak** (`KEYCLOAK_ADMIN_PASSWORD`)
* 👥 **User passwords for Backstage login** (`USER_PASSWORD`)

With these, you can:

1. Open the restaurant’s admin office (login as `cnoe-admin` in Keycloak).
2. Invite new staff or revoke access to anyone (create/delete users).
3. Let `user1` (admin) and `user2` (regular staff) enter and work securely.

The city also ensures **automatic key rotation** and **access logs**, so you always know who requested or used a key — just like a responsible business tracks who opens the safe.


## 🔐 ID Checkpoint Installed (Keycloak)
Before customers or staff can enter “Nginx Diner,” the city requires every business to set up a digital ID checkpoint. This is where Keycloak comes in — it acts as the smart city's centralized identity provider.

When someone tries to enter the restaurant (access the app), the system checks:

- Who they are (authentication)

- What they can do inside (authorization)

Keycloak is integrated with the restaurant's systems, so only the right people — chefs, waiters, managers (developers, services) — can enter specific areas (dashboards, APIs, admin panels).

## 🍽️ Restaurant Blueprint (Backstage Template)
You start with a digital form that describes what kind of restaurant you want — name, location, cuisine. This is your Backstage software template.

## 🏗️ Construction Company Assigned (Gitea Repo)
Immediately, the system creates a construction site and assigns a builder — this is the new Git repository in Gitea that will contain the code and infrastructure for your restaurant.

## 🚀 Automated Launch (ArgoCD + GitOps)
The city’s automation system (ArgoCD) notices the new blueprint and starts deploying the restaurant infrastructure in Kubernetes — plumbing, electricity, kitchen... everything is set up using GitOps, just by syncing the Git repo.

## 🗺️ Put on the Map (Backstage Component)
Once everything is running, the restaurant is registered on the city map (Backstage catalog). Now everyone in the city (your developers) can see it, access it, and even request updates or improvements.

# Basic Deployment

Let's start by deploying a simple application to the cluster through Backstage.

Click on the `Create...` button on the left, then select the `Create a Basic Deployment` template.

![img.png](images/backstage-templates.png)


In the next screen, type `demo` for the name field, then click Review, then Create. 
Once steps run, click the Open In Catalog button to go to the entity page. 

![img.png](images/basic-template-flow.png)

In the demo entity page, you will notice a ArgoCD overview card associated with this entity. 
You can click on the ArgoCD Application name to see more details.

![img.png](images/demo-entity.png)
