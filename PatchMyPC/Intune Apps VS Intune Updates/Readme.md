# TeamViewer Deployment with Patch My PC and Microsoft Intune

## Overview

A common question when using Patch My PC with Microsoft Intune is whether to publish an application as an **Intune App**, an **Intune Update**, or both.

This article uses **TeamViewer** as a practical example and explains the two most common deployment scenarios.

---
![image alt](https://github.com/pratham-swaroop/work-vault-master/blob/b61d7ca3efa3d700949a795803b56e57f2d8867b/MediaFiles/PMPC_IntuneApps-VS-IntuneUpdates.jpg)


## Scenario 1: Required App

### Goal
Ensure TeamViewer is installed and maintained on all targeted devices.

### Configuration
- Patch My PC → Intune Apps
- Product: TeamViewer
- Assignment: Required

### Device Behavior

| TeamViewer State | Result |
|------------------|----------|
| Not Installed | TeamViewer is installed |
| Installed but outdated | TeamViewer is upgraded |
| Installed and current | No action |

### When to Use
Use this model when TeamViewer is a mandatory corporate application and every managed device should have it installed.

---

## Scenario 2: Available App + Required Update

### Goal
Allow users to install TeamViewer from Company Portal while IT remains responsible for updates.

### Configuration

#### Intune App
- Product: TeamViewer
- Assignment: Available

#### Intune Update
- Product: TeamViewer
- Assignment: Required

## Scenario as Flowchart for TeamViewer Deployment Decision

```text
                    PATCH MY PC + INTUNE
                 TeamViewer Deployment Decision

                               START
                                  |
                                  ▼

        Do all corporate devices need TeamViewer installed?
                                  |
                    ┌─────────────┴─────────────┐
                    │                           │
                  YES                          NO
                    │                           │
                    ▼                           ▼

      Create TeamViewer as             Should users be able to
         INTUNE APP                    install TeamViewer from
         Assignment:                   Company Portal?
         REQUIRED
                    │                               │
                    │                   ┌───────────┴───────────┐
                    │                   │                       │
                    │                  YES                      NO
                    │                   │                       │
                    ▼                   ▼                       ▼

      Result: TeamViewer             Create TeamViewer      No deployment
      installs automatically         Intune App            required
      on every device & Patch        Assignment:
      Regularly                      AVAILABLE
                                         │
                                         ▼

                         If a user installs TeamViewer,
                         should IT automatically keep it updated?
                                         │
                          ┌──────────────┴──────────────┐
                          │                             │
                         YES                            NO
                          │                             │
                          ▼                             ▼

              Create TeamViewer              User installs app
              INTUNE UPDATE                  but updates are not
              Assignment: REQUIRED           managed by IT
                          │
                          ▼

                Device Evaluation During Check-In

                 TeamViewer Installed?
                           │
                  ┌────────┴────────┐
                  │                 │
                 NO                YES
                  │                 │
                  ▼                 ▼

             Do Nothing      Update to latest
                             TeamViewer version
```

### Device Behavior

| TeamViewer State | Result |
|------------------|----------|
| Not Installed | Update deployment does nothing |
| User installs TeamViewer | Application becomes eligible for updates |
| Installed and outdated | TeamViewer is upgraded automatically |
| Installed and current | No action |

### When to Use
Use this model for optional software where users decide whether they need the application, but IT wants to enforce security and compliance updates.

---

## Quick Decision Matrix

| Requirement | Configuration |
|------------|--------------|
| Install and update on every device | Intune App → Required |
| User chooses installation | Intune App → Available |
| Automatically update optional apps | Intune Update → Required |
| Optional install + automatic updates | Available App + Required Update |

---

## Key Takeaway

**Required App** = Install + Update

**Available App + Required Update** = User Chooses Installation + IT Controls Updates

This deployment pattern helps organizations balance user flexibility with application lifecycle management and security compliance.
