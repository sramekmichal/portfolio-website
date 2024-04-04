---
title: "GitHub: Personal Access Tokens"
date: 2023-10-19
draft: false
author: "Michal Šrámek"
# devto: "https://dev.to/sramek5/activity-monitoring-and-audit-in-aws-1hd3"
tags:
  - GitHub
  - Security
  - Audit
image: /images/blogs/github-security.jpg
description: "GitHub: Personal Access Tokens"
toc: true
---

There are currently 2 types of personal access tokens (PATs) available on GitHub. **Fine-grained PATs** (beta) and **Classic PATs**. Both are used for authentication and authorization, but have different properties and uses. There is no Token like Token.

|   | Fine-grained PATs (beta) | Classic PATs |
| --- | --- | --- |
| Purpose | Token for GitHub API for scripting or testing. | OAuth personal access tokens **OR** Instead of a Git password via HTTPS **OR** For API authentication via Basic Authentication. |
| Expiration | Required (up to 1 year) | Can be used without expiration. |
| Owner | User and also Organization | User |
| Access | Either repository permissions (public, all, selected) and may (may not) access resources under the personal account; **OR** repository permissions (public, all, selected) and may (may not) access resources under the organization | Determined by [Scopes](https://docs.github.com/en/apps/oauth-apps/building-oauth-apps/scopes-for-oauth-apps). These can be for anything the user is entitled to (even within the Organization). |
| Overview | They are visible in the Organization in the "Active tokens" overview with a visible expiration date. Once clicked, the accesses (what permissions the token has in which repository) are clearly visible. | There is no overview in the Organization. It is possible to retrieve token activity via the API, but we cannot distinguish user activity from machine activity. Their activity is only recorded in GitHub Audit Logs. |
| Can PAT be disabled? | Yes | Yes |
| Is approval required? | Yes and No | No |

## Active use of Fine-grained PATs

If the GitHub Organization Member (not the Owner) chooses to use Fine-grained PATs and mandatory approval of Fine-grained PATs is enabled, the Member will receive a "not found" message and the request will be in pending status. It will also appear in the "Pending Requests" report in the Organization:

![Fine-Grained PAT](/images/blogs/fine-grained-pat.png)

The GitHub Organization Owner can judge whether the access is justified or not and approve/deny the request (or instruct the Member to restrict the rights of his PAT):

![Repository Access](/images/blogs/repository-access.png)

This way **least privileges** can be demanded and access is under control. Member will receive the approval/denial result via email.

With disabled approval, the PAT is visible in the overview of active PATs, but there is no Organization Owner control over accesses.

If a Member delete their PAT, no Owner approval is required, the PAT simply disappears from the list of active PATs.

If a Member changes the rights of an already approved PAT, it needs to be re-approved (**correct and expected approach**).

With Classic PATs there is no overview of who and how many are actually using them. Through the API it is possible to get some information, but it is no longer possible to distinguish between a machine and a Member (all activities are pretending to be Member activities).

## The risks of using PATs

The use of PATs within the GitHub Organization exposes the Organization to some potential risks:

1. **Broad permissions:** If the PAT grants wide permissions (e.g. administrative rights to everything in the Organization), a Member or application with this token can perform a wide range of actions, including those that may harm the Organization.
2. **Loss of token:** If the PAT is in the wrong hands, it can be misused. Depending on the permissions of the token, this can mean loss of data, modification of repositories, etc.
3. **Expiration:** Classic PATs on GitHub do not need to have any expiration (see table). This means that if a PAT is compromised and no one notices, it can be misused for a long time.
4. **Audit:** Although actions performed using PAT can be recorded, it can be harder to distinguish who actually performed the action if multiple people or systems share the token.
5. **Management:** With multiple PATs created it can be harder to keep track of who created what PAT, why and where it is used -> especially in the case of Classic PATs that are not visible in the active PAT overview (see table). This is a significant complication for security management.
6. **Function of systems**: A Member who builds a functional system/application on his/her personal PAT within his/her team and changes employment in the future, the rest of the team (or possibly the Organization) will no longer be able to use the automation/system/application. Upon leaving GitHub, the Organization will lose its PAT rights.

## What GitHub Support says

- Fine-grained PATs have similar access restrictions as GitHub Apps.
- Classic PAT has the most capabilities of all the verification methods.

Adding [repository to the GitHup App](https://docs.github.com/en/rest/apps/installations?apiVersion=2022-11-28#add-a-repository-to-an-app-installation) would not help, because it has unsupported [endpoint for Fine-grained PAT](https://docs.github.com/en/rest/overview/endpoints-available-for-fine-grained-personal-access-tokens?apiVersion=2022-11-28#apps). Managing GitHub Apps has always been seen as something that requires a human to be involved because of the potential for improper access. GitHub is also getting feedback on Fine-grained PATs (they are in beta) in [discussion](https://github.com/orgs/community/discussions/36441). Anyone interested in using Fine-grained PATs for GitHub Apps and their repository management can have their voice heard here.

## Replacement for PATs

Unless a specific case requires it, the use of PATs within the GitHub Organization should be avoided at all costs (especially classic ones), see Risks above. There are different authentication and authorization solutions for different tasks and integrations:

1. **OAuth App -** Allows you to grant a specific application access to specific resources with specified permissions. In addition, access can be revoked at any time.
2. **GitHub Apps -** the most preferred way to integrate with GitHub. They can only access the information they need (**least privileges**) which increases security.
3. **Deploy Keys -** only for specific repositories, for authentication. These are SSH keys that bind to exactly one repository and allow reading or writing. 
4. **Webhooks -** for notification of certain events occurring in the Repository or the Organization. Not functional with applications running on-premise.

## Apparent Solution
PAT's are unfortunately another thing which is being left very often in an uncontrolled situation due to nobody really is responsible for it. There is no definite "best practise" approach. One thing is recommended - only the newer fine-grained PATs with enforced approval should be allowed to GitHub Organization Members. This way, the Organization Owner can control the accesses and ensure that the principle of <u>least privileges</u> is respected. The use of Classic PATs should be avoided as much as possible and left as a privilege only for the Organization Owners and their GitHub management. 
