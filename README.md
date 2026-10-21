# SWYNEX – Monitoring and Cost Awareness
### Task 3 — SWYNEX Cloud Internship

**Deployment being monitored:** Static portfolio site from Task 2 (`SWYNEX-Basic-Cloud-Deployment`)
**Host:** GitHub Pages (static-site PaaS)
**Live URL:** https://kshirsagar-mahesh.github.io/SWYNEX-Basic-Cloud-Deployment/

---

## 1. Overview

This document adds basic monitoring, a simple alert idea, and a cost and access-control checklist for the Task 2 deployment. Because the site is static and hosted on GitHub Pages, there are no servers or application logs to manage, so monitoring focuses on **availability** (is the site up?) and **deployment health** (did the last publish succeed?).

---

## 2. Logs and Status Checks

| What to check | Where | What it tells me |
|---|---|---|
| Deployment history | Repo → **Actions** tab → "pages build and deployment" runs | Whether each push built and published successfully, with a log for every run |
| Pages status | Repo → **Settings → Pages** | Current live URL and the last successful deployment |
| Availability check | Scheduled GitHub Actions workflow (`uptime-check.yml` in this repo) | Requests the live URL every 6 hours and fails if the HTTP status is not 200 |
| External uptime probe (optional) | Free uptime-monitoring service (e.g. UptimeRobot) | Independent check from outside GitHub, with response-time history |
| Traffic (optional) | Add a privacy-friendly analytics script, or a free counter | Page views over time |

**How the uptime workflow works:** it runs on a schedule, sends an HTTP request to the site, and exits with an error if the response is anything other than HTTP 200. A failed run shows as a red cross in the Actions tab, which is the status-check log.

---

## 3. Simple Alert Idea

| Trigger | Alert | Action |
|---|---|---|
| Uptime workflow fails (site down or returns an error) | GitHub emails the repository owner about the failed workflow run | Open the Actions log, check Pages settings, redeploy if needed |
| Deployment build fails after a push | Same GitHub failed-run email notification | Read the build log, fix the file, push again |
| Uptime service detects downtime (optional) | Email alert from the uptime service | Verify the site, then check GitHub status page |

**Notification setup:** GitHub → Settings → Notifications → enable email for **Actions** (failed workflows only), so alerts are not noisy.

---

## 4. Cost Awareness Checklist

- [x] Hosting is on **GitHub Pages**, which is free for public repositories — current cost is **$0**
- [x] No databases, servers, or paid services attached to this deployment
- [x] Know the free-tier limits: Pages sites are intended to stay under about 1 GB and roughly 100 GB of bandwidth per month (soft limits)
- [ ] If this moves to a paid cloud (AWS / Azure / GCP): create a **budget alert** (e.g. AWS Budgets) at a small threshold before deploying anything
- [ ] Tag resources by project so costs can be traced
- [ ] Delete unused resources and test deployments after the internship tasks end
- [ ] Prefer static hosting or serverless for low-traffic projects to avoid idle-server cost

---

## 5. Security and Access-Control Checklist

- [x] **Two-factor authentication** enabled on the GitHub account
- [x] **HTTPS enforced** (Settings → Pages → "Enforce HTTPS")
- [x] Only the repository owner has write access (no unnecessary collaborators)
- [x] No passwords, API keys, or tokens committed to the repository
- [ ] Enable **secret scanning** and **Dependabot alerts** (Settings → Code security)
- [ ] Use **fine-grained tokens** with minimum permissions if any token is ever needed
- [ ] Protect the `main` branch (require pull requests) if more contributors are added
- [ ] Review account sessions and authorised apps periodically

---

## 6. What I Learned

- Even a static site benefits from simple monitoring: availability checks and deployment logs catch problems early.
- Alerts should be few and actionable, not noisy.
- Cost control starts before deployment: know the free-tier limits and set budget alerts when using paid services.
- Least privilege and 2FA are the cheapest and most effective access-control measures.
