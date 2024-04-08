---
title: "Fortress GitHub: Building a Secure Organization"
date: 2024-02-14
draft: false
author: "Michal Šrámek"
devto: "https://dev.to/sramek5/fortress-github-building-a-secure-organization-4ke8"
tags:
  - GitHub
  - Security
  - Audit
image: /images/blogs/github-authentication.jpg
description: "Fortress GitHub: Building a Secure Organization"
toc: true
---

Authentication security on the GitHub platform is a key aspect of protecting corporate data and securing user accounts. Usually, we rely not only on standard methodologies (passwords, 2FA) but also on the significant benefits of centralized access control and easy integration with an external identity service. In this way, we could contribute to an overall increase in the level of security. SAML (Security Assertion Markup Language) is a standard for exchanging authentication and authorization data between security domains, often used for Single Sign-On (SSO) across web applications. In the context of GitHub, it's important as it allows centralized access control and easy integration with external identity services, enhancing overall security.

Before implementing SAML protocol on the GitHub platform, several key requirements should be met.

## Regions Separated by Root Teams
In an international organizations integrated inside one big GitHub Enteprise, it is important to separate the different regions (resources) with a specific team that will have a root responsibility.

- Each unique root team should have the opportunity to participate in decision-making (global PR related to the region) concerning the specifics of the region (technology, business, etc.).
- Other development teams corresponding to their region should be always nested within a root team and should not exist outside their team (including users).

![GitHub Root Teams](/images/blogs/github-root.png)

- External partners (helping with migrations, quality assesment etc.) should be in the namesake group and their access should be explicit, limited, and valid only for the repository invitation (see below).

![GitHub Externals Partners](/images/blogs/external-partners.png)

## Disabling Outside Collaborators

It is practically unacceptable for anyone with admin rights in a repository to have permission to add Outside Collaborators to that repository. Disabling these Outside Collaborators on an organization-wide level is an important step toward securing and protecting the organization's sensitive data and resources. This measure will minimize the risk of unauthorized access and misuse of information by those outside the organization.

  - There should be no Outside Collaborators at the organizational level. Any potential Outsiders must go through internal company HR system to gain access in the same way as internal members.
  
![Outside Collaborators](/images/blogs/outsiders.png)

  - Related: Outside Collaborators should not be able to request access to GitHub or OAuth applications to access any organization and its resources.

## Base Permission for Organization
At the GitHub organization level, it is important to correctly select the Base Permissions of members. The most commonly, the **Read** method is selected (users are allowed to clone and download repositories). The permissions in GitHub, like the RBAC policy of any other system, should be as strict as possible (especially if the company is in the Cloud, where is applied shared responsibility) and should only provide the permissions that are necessary for each specific user. An important step to strengthen security in a multi-region approach is to change the organizational base permissions from **Read** to **No permissions**.

This step eliminates the **implicit** access of all members to the organization and replaces it with **explicit** access at the repository level. Support for explicit access allows the organization to flexibly set and manage access rights in accordance with security policies. The organization gains greater control over who has access to individual resources and minimizes the risk of unauthorized access, data misuse, and reduces the risk of security incidents.

- Users are not be blocked from collaborating in this way, they will need just add/remove and fine-tune the correct access of other teams to their repositories (see below).
- The principle of <u>least privilege</u> is respected.

![Base Permissions](/images/blogs/base-permissions.png)

More information is provided by [this article](https://cycode.com/blog/github-permissions-for-maximum-security/).

## Collaboration at Team Level
Collaborating on GitHub in teams is an organized approach to working on projects. Teams allow for grouping members (using other teams), managing permissions, communicating, and sharing responsibilities. It also enhances transparency, as all team members have access to the same resources and information. The team structure should be designed to reflect the organization's structure and business processes.

- Teams are repository owners, not individuals (*common security goal*).
- Collaboration and approval of PR should be at the team level, not on individual level.

![Collaboration](/images/blogs/github-collaboration.png)

- The creation of Ad Hoc Teams should be prohibited at the organization-wide level. Every team that needs to exist in a GitHub organization should be first processed through internal company system (e.g. Active Directory). Such a change should be then reflected in GitHub.
- Once SAML is enforced, GitHub will automatically synchronize team members with the corresponding group structure according to used IdP.

## Repository Protection

### Repository Creation

The process of creating a repository is the first step to organizing and sharing code and projects with the team. It is important not only to set permissions correctly to ensure that only appropriate team members have access to the repository (see above), but also to implement a policy. There are currently 3 types of repositories that can be created in the enterprise version of GitHub:

1. <span style="color:green">**Private (visible to organization members with permission).**</span>
2. <span style="color:orange">**Internal (visible to all members of the enterprise organization = among other regions).**</span>
3. <span style="color:red">**Public (visible to anyone from the internet).**</span>

<span style="color:green">**Private**</span> repositories are a key part of maintaining the integrity of the internal development process and should be established as a **matter of priority**.

All members of the Enterprise organization have access basically to all repositories created as <span style="color:orange">**Internal**</span>. Permissions can be changed within the Enterprise, it is not easy to assess at Organisation level who all has such permissions. Therefore, it is worth considering whether to create Internal repositories and the responsibility should belong to each team.

<span style="color:red">**Public**</span> repositories should be gradually banned at the level of the GitHub Enterprise. A separate organization should be created within GitHub Enterprise, where a public license and policy should be properly created to manage such repositories. Thus, in exceptional and justified cases, a <span style="color:red">**Public**</span> repository may be created, but outside of the each regions in dedicated GitHub Organization.

![Repository Creation](/images/blogs/github-repository.png)

### Repository Default Branch
**Default Branch Protection Rules** are essential to ensure the integrity and security of code in various forms of shared repositories. The rules allow us to define levels of protection, including mandatory rules for change approval or protection against direct upload to a branch that is to be used exclusively for deployment ([Trunk Based Development](https://trunkbaseddevelopment.com/)) to the Cloud.

Because automation is a key part of the development process, it is important to ensure that the rules are set correctly and that the automation is properly configured. It is possible to use GitHub Actions to create PRs. However, this also has some risks and therefore in all repositorie should be required at least 1 person for approval of the PR → For this reason, CODEOWNERS should be properly defined in each repository to prevent automatic merges to the release branch without the repository owner's knowledge.

![Branch Protection](/images/blogs/branch-protection.png)

Code owners are always a team in the organization (see above), or even a team other than the one that owns the repository can be defined. However, such a team must then have WRITE rights to the repository.

- GitHub Actions may create PRs, but they may not automatically approve them.
- There should be a CODEOWNERS file in each repository.
- Gradually the protection of the default (release) branch should be enforced by introducing the Organizational **Ruleset** and monitored scraping the **Audit Logs**.

## Conclusion
The implementation of the SAML protocol on the GitHub platform is a key step in enhancing the security of the organization. However, it is important to ensure that the organization's structure and processes are properly designed and that the necessary security measures are in place. The organization should be structured in such a way that it reflects the business processes and that the team structure is designed to ensure that the organization's resources are properly managed and protected. The implementation of the SAML protocol should be accompanied by the implementation of the necessary security measures, such as the protection of the default branch, the protection of the repository, and the protection of the organization's resources. The implementation of these measures will help to ensure that the organization's resources are properly managed and protected, and that the organization's data is properly secured.