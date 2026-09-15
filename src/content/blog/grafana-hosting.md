---
title: "What Is Grafana & How to Self-Host Your Dashboards the Easy Way"
slug: "grafana-hosting"
description: "Learn what Grafana is, why self-hosting it beats Grafana Cloud for control, and how to deploy managed Grafana hosting on xCloud in minutes."
focusKeyword: "Grafana hosting"
category: "Guide"
pubDate: 2026-09-15
draft: false
ogImage: "/_landing/blog/grafana-hosting-cover.png"
author: "xCloud Team"
tags: ["Grafana hosting", "self-hosted Grafana", "observability", "Docker hosting", "server management"]
faq:
  - question: "Is Grafana free to self-host?"
    answer: "Yes. Grafana OSS is free and open source under the AGPLv3 license. Grafana Cloud and Grafana Enterprise are separate, paid products from Grafana Labs with different feature sets; nothing exclusive to those tiers is part of a self-hosted OSS deployment."
  - question: "Does Grafana store my metrics?"
    answer: "No. Grafana queries data where it already lives: Prometheus, Loki, InfluxDB, Elasticsearch, PostgreSQL, MySQL, and dozens more, rather than storing metrics itself. What it does store, in its own database, are your dashboards, users, and alert rules."
  - question: "What database does Grafana use?"
    answer: "SQLite by default, with MySQL and PostgreSQL supported for teams that need more concurrency or a shared production database."
  - question: "Is xCloud affiliated with Grafana Labs?"
    answer: "No. The hosting on this page is xCloud's own managed infrastructure; xCloud is not affiliated with Grafana Labs, and Grafana is a trademark of Grafana Labs."
  - question: "How long does it take to deploy Grafana on xCloud?"
    answer: "Most teams go from choosing a server to a live instance with their first data source connected in a few minutes, using One Click Apps, with no manual Docker setup or SSL configuration required."
  - question: "Can I cancel my Grafana hosting plan anytime?"
    answer: "Yes. All xCloud Cloud VPS plans for Grafana hosting can be canceled anytime, with no long-term contract."
  - question: "Which plan should I start with?"
    answer: "The Cloud VPS 6GB ($24.99/mo) comfortably runs Grafana plus a modest data source for a small team. If you're running heavier sources like Prometheus or Loki alongside it, or scaling to more users, the 16GB plan is the popular choice for production setups."
  - question: "Do I need to know Docker to deploy Grafana on xCloud?"
    answer: "No. xCloud provisions the Docker server and the Grafana container for you through One Click Apps: you choose the server, select the application, and configure your domain. The underlying container setup is handled automatically."
---

# What Is Grafana & How to Self-Host Your Dashboards the Easy Way

You spin up a new service, wire it into Prometheus, and now you're staring at raw metrics in a browser tab with no context. Or worse, you're paying for a hosted dashboard tool that meters your users, caps your data retention, and quietly ships your infrastructure metrics through a third party's pipeline. Neither feels great once you've been burned by it.

**Grafana** solves the visualization half of that problem. It's the open-source layer that sits on top of the data you're already collecting and turns it into dashboards, ad-hoc queries, and alerts. What it doesn't solve on its own is where that instance lives, who patches it, and what happens to your dashboards if a vendor changes its pricing tier overnight. For teams who've been through that once, the answer is usually the same: put Grafana on a server you actually control.

This guide covers what Grafana is, what it's genuinely good at, the real trade-offs of self-hosting it versus using Grafana Cloud, and how **[managed Grafana hosting](https://xcloud.host/grafana-hosting/)** on xCloud gets a production instance live in minutes instead of an afternoon.

![What Is Grafana & How to Self-Host Your Dashboards the Easy Way](/_landing/blog/grafana-hosting-cover.png)

## TL;DR (Too Long, Didn't Read?)

Short on time? Here's the whole guide in a few lines:

- **Grafana is a query-and-visualize layer, not a database.** It connects to Prometheus, Loki, InfluxDB, Elasticsearch, PostgreSQL, MySQL, and dozens of other sources; it doesn't store your metrics itself.
- **Grafana OSS is free and AGPLv3-licensed.** Grafana Cloud and Grafana Enterprise are separate, paid products from Grafana Labs with different pricing and feature sets.
- **Self-hosting means your dashboards, users, and alert rules live in a database on your server** (SQLite by default, or MySQL/PostgreSQL for production setups) instead of on someone else's infrastructure.
- **The trade-off is maintenance**, not capability: Docker, updates, backups, SSL, and firewall rules are on you unless something manages them for you.
- **xCloud's managed Grafana hosting** removes that maintenance layer: one-click deployment, a persistent data volume, free SSL, and a control panel, on a Cloud VPS starting at **$24.99/mo**.
- Most teams are live with their first data source connected within minutes of deployment, not a weekend project.

## What Is Grafana?

**Grafana is an open-source observability and dashboard platform written in Go.** It queries the data you already collect and turns it into dashboards, ad-hoc exploration, and alert rules: it does not collect or store your metrics itself, so you bring the data sources to it.

Think of Grafana less like a database and more like a universal lens: point it at Prometheus, Loki, InfluxDB, Elasticsearch, or a plain PostgreSQL table, and it renders the same query language of graphs, gauges, and tables on top, regardless of where the numbers actually live.

**A few things worth knowing:**

- Grafana OSS is **free and open source under the AGPLv3 license**.
- It stores its own dashboards, users, and alert rules separately from your metrics, in **SQLite by default**, with **MySQL and PostgreSQL supported** for larger teams.
- **Grafana Cloud** and **Grafana Enterprise** are distinct, paid products from Grafana Labs. Features exclusive to those tiers aren't part of a self-hosted OSS deployment.
- xCloud's hosting is xCloud's own managed infrastructure offering; **xCloud is not affiliated with Grafana Labs**, and Grafana is a trademark of Grafana Labs.

| Layer | What it does | Example |
|---|---|---|
| **Data source** | Collects and stores raw metrics/logs | Prometheus, Loki, InfluxDB, Elasticsearch |
| **Grafana** | Queries, visualizes, alerts | Dashboards, Explore mode, alert rules |
| **Grafana's own DB** | Stores dashboards, users, alert config | SQLite, MySQL, or PostgreSQL |
| **Contact points** | Where alerts get routed | Email, Slack, chat webhooks |

If you only remember one line: **Grafana is the display and the doorbell, not the warehouse.**

## Why Self-Host Instead of Using a Hosted Dashboard Vendor?

This is the honest reframe most product pages skip. A fully hosted dashboard tool is genuinely less setup work on day one. The catch nobody talks about is what happens after day one: per-seat pricing that scales against your headcount, data retention limits tied to a tier, and metrics that transit through infrastructure you don't control before you ever see them.

Self-hosting flips that trade. **You take on the operational side** (deployment, updates, backups, uptime) **in exchange for dashboards, users, and alert rules that live in a database on a server you chose.** That's not automatically "more private" by default; you still have to configure retention, disable usage reporting if you want it off, and pick where that server physically sits. But the option is yours, not a vendor's.

For platform teams and agencies who've watched a SaaS dashboard bill creep up with every new engineer added, that trade is usually worth making.

## Who Actually Needs a Self-Hosted Grafana Instance?

These aren't hard boundaries, they're starting points. If you already care where your metrics live, your dashboards belong on a server you control.

### Platform & DevOps Teams
Infrastructure metrics on one screen, alert rules evaluated by your own instance (not a third party's), and native support for **Prometheus, Loki, and dozens of other sources** across every environment from staging to production.

### Developers
Dashboards deployed beside the app you ship, a documented HTTP API, dashboards provisioned as code, and **Explore mode** for the ad-hoc query you need at 11 PM without building a whole panel first.

### Small Teams
One instance covering the whole team, a folder per project, separate accounts, and **viewer, editor, and admin roles** so nobody's stepping on anybody else's dashboard.

### Agencies & Consultants
Replace per-seat dashboard pricing with a flat infrastructure cost, unlimited dashboards and panels, and the ability to move fast for a new client without waiting on a vendor's seat limit.

### IoT & Home Lab
Sensor data on your own hardware, a wall dashboard that just stays up, and time-series sources of your choosing, running beside the rest of your self-hosted stack.

### Privacy-First Teams
Data on your own server, in the region of your choosing, with dashboards stored on your own disk and no third-party analytics vendor sitting between you and your metrics.

## What Grafana Actually Does Once It's Running

### Dashboards & Panels
Time series, tables, gauges, logs, and geomaps, arranged on dashboards you can template, version, and share across a team.

### Explore & Query
Run ad-hoc queries against any connected source without building a full dashboard first: the tool you reach for when you just need an answer, not a permanent panel.

### Data Sources
**Prometheus, Loki, InfluxDB, Elasticsearch, PostgreSQL, MySQL**, and dozens more, each configured per instance to match whatever you're already running.

### Alerting
Alert rules are **evaluated by your own instance** and routed to the contact points you configure: email, a chat webhook, or whatever your team already uses for incidents.

### Users & Roles
Organizations, teams, and viewer, editor, or admin roles, so a single instance can cover an entire team without everyone sharing one login.

### Privacy Controls
Nothing leaves the server you deployed to, and Grafana's usage reporting can be switched off in the config. Worth repeating: self-hosting is not privacy on its own, you still have to configure it that way.

### APIs & Provisioning
A documented HTTP API, provisioning files, and AGPLv3-licensed source, so anything you want to automate or adapt, you can, without asking a vendor's roadmap to catch up first.

### Your Own Database
Dashboards, users, and alert rules live in a database on your server: **SQLite by default**, with **MySQL and PostgreSQL supported** for production scale. Your actual metrics stay in whatever sources you connect; Grafana never takes ownership of them.

## The Part Self-Hosting Guides Don't Mention: Maintenance Is the Real Cost

Deploying Grafana manually isn't hard on its own: it's a modest Go binary, and the [official Grafana documentation](https://grafana.com/docs/grafana/latest/) walks through installation cleanly. The part that eats a weekend is everything around it: setting up **Docker** correctly, provisioning a **persistent volume** so your data survives a container restart, issuing and renewing **SSL**, configuring a firewall, and then remembering to do all of that again for the next update.

That's the trade-off worth naming plainly: self-hosting gets you control, but maintenance is the line item people forget to budget for. This is exactly where managed hosting earns its keep, without taking the control away.

## How Managed Grafana Hosting on xCloud Works

xCloud handles the operational layer so the "self" in self-hosted still means **you own the server and the data**, without you personally patching containers at midnight.

### Step 1: Choose Your Server
Pick an **xCloud managed server** or connect your own provider, then set it up as a **Docker server** to deploy Grafana on top of.

### Step 2: Select Application
Open **One Click Apps** in your xCloud dashboard and choose **Grafana** from the available applications to begin the guided setup.

### Step 3: Configure Your Instance
Add your domain and deployment settings. xCloud provisions Grafana on your server with its **data directory on a persistent volume**, so container restarts don't wipe your dashboards.

### Step 4: Go Live Right Away
Confirm the setup, open your domain, sign in as the **admin user**, change the default password, and connect your first data source.

👉 For the full walkthrough, check the [xCloud docs](https://xcloud.host/docs/) for step-by-step guides on Docker deployment and server provisioning.

## Grafana Hosting Pricing on xCloud

Your dashboards pile up; your bill shouldn't. Grafana itself is a modest Go service, so the entry tier leaves real room for the data sources and everything else you're likely running alongside it.

| **Plan** | **Price** | **RAM** | **Storage** | **vCPU** | **Bandwidth** |
|---|---|---|---|---|---|
| Cloud VPS 6GB | **$24.99/mo** | 6 GB | 100 GB NVMe SSD | 4 cores | 30 TB |
| Cloud VPS 16GB (Popular) | **$59.99/mo** | 16 GB | 200 GB NVMe SSD | 6 cores | 30 TB |
| Cloud VPS 24GB | **$84.99/mo** | 24 GB | 300 GB NVMe SSD | 8 cores | 30 TB |

All plans renew at list price and can be canceled anytime, no long-term lock-in. See the full **[pricing page](https://xcloud.host/#pricing)** for current details, or browse **[all xCloud features](https://xcloud.host/features)**.

**Rule of thumb:** the 6GB tier comfortably runs Grafana plus a modest Prometheus or Loki instance for a small team. Once you're running Grafana alongside heavier data sources, or scaling to more dashboards and users, the 16GB tier is where most production setups land.

## What You Get Beyond the VPS Spec Sheet

- ✅ **Fast Provisioning**: deploy on an xCloud managed server built with optimized stacks and high-speed NVMe SSDs.
- ✅ **Enterprise-grade security**: a free SSL certificate included, with more advanced security features at no additional cost.
- ✅ **Intuitive Control Panel**: from server creation to application deployment, everything is managed from one dashboard.
- ✅ **No Lock-In**: cancel anytime, and your data stays on infrastructure you control the whole time.

## Self-Hosted Grafana vs. Grafana Cloud: The Honest Comparison

| **Factor** | **Self-Hosted (xCloud)** | **Grafana Cloud** |
|---|---|---|
| **Cost model** | Flat infrastructure cost | Per-seat / usage-tiered pricing |
| **Data location** | Your server, your region | Grafana Labs' infrastructure |
| **Retention limits** | Set by your own storage | Capped by plan tier |
| **Setup effort** | Minutes, via One Click Apps | Minutes, fully hosted |
| **Maintenance** | Handled by xCloud's managed layer | Handled entirely by Grafana Labs |
| **Enterprise features** | Not included (OSS only) | Available on paid tiers |
| **Best for** | Teams that want infrastructure ownership | Teams that want zero infrastructure at all |

Neither path is universally better. Grafana Cloud is a reasonable choice if you never want to think about a server again and per-seat pricing doesn't scare you. Self-hosting through **[managed Grafana hosting](https://xcloud.host/grafana-hosting/)** is the better call once you're past a couple of engineers, care about where the data physically sits, or you're already running other services on a VPS and don't want a second vendor relationship for dashboards alone.

## Deploy Your Grafana Instance Today

Grafana is the free, well-built half of the observability stack, the part that turns raw metrics into something a team can actually look at. The other half is deciding where it lives, and that decision compounds. A vendor's pricing tier or retention cap eventually becomes your problem too, right around the time you've built twenty dashboards on top of it.

Putting Grafana on a server you control sidesteps that entirely, and with a one-click deployment path, it no longer costs you the weekend it used to. Pick a **[Cloud VPS plan](https://xcloud.host/#pricing)**, deploy Grafana through **One Click Apps**, and connect your first data source before your coffee's cold.

**[Deploy managed Grafana hosting on xCloud →](https://xcloud.host/grafana-hosting/)**

If you have found this blog helpful, feel free to [**subscribe to our blogs**](https://xcloud.host/blog/) for valuable tutorials, guides, knowledge, and tips on web hosting and server management. You can also join our [**Facebook community**](https://www.facebook.com/groups/xcloud.community) to share insights and engage in discussions.
