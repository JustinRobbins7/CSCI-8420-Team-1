# Team 1 Project Proposal: Liberapay

Written by Team 1:
Joel Allou, Mohammad H. Al Huneidi, Miti Mareddy, Justin Robbins, and Nicholas Sabata

Link to Liberapay Github: [Liberapay Project](https://github.com/liberapay/liberapay.com)

## 1. Operational Environment
### *1.1. Systems Engineering View*
![Systems Engineering View](/Images/SA.png)

#### _SYSTEM-OF-INTEREST View_
![System Engineering View Zoomed](/Images/SA_Zoom.PNG)

#### _PDF Version_
![Systems Engineering View PDF](/Images/SA.pdf)

---

### *1.2. Possible Threats to Operational Environment*
Due to the nature of Liberapay there are possible threats within its operational environment. Those threats exist because of the vulnerabilities (discussed in the Vulnerabilities section below) that Liberapay has. The two main possible threats that we have identified when it comes to Liberapay pertain to:
1. Interception of payment information (Monetary Concerns): Since Liberapay deals with donations/crowdfunding, an opportunity could exist for attackers to intercept such transactions and gain access to financial information of users.
2. Unauthorized access to private information (Privacy Concerns): Users of Liberapay have to register accounts to use it. Those accounts contain private information pertaining to the user, such as statements, names, goals etc... An opportunity could exist for attackers to get unauthorized access to an account and obtain a victim’s private information.

### *1.3. Software Security Features*
Based on the purpose of Liberapay, the software security features should revolve around three main categories: Payment Process, User Data Management, and Miscellaneous.

<ins>Payment Process</ins>
* SSL Protocol
* PCI Compliance
* Tokenization 
* Monitor transactions for possible fraud, money laundering, and terrorism financing

<ins>User Data Management</ins>
* Authentication/Authorization
* Access Control
* Cookies Control
* Backups/Recovery
* Encryption
* Data Masking
* Deletions/Erasure 
* X-XSS Protection
* Cross-Site Request Forgery (CSRF) Protection
* Content Security Policy Settings
* Validate Redirects and Forwards
* X-Frame Options (Clickjacking protection)

<ins>Miscellaneous</ins>
* Compliance with GDPR guidelines of Europe

## 2. Motivation
Our team inspected a handful of open source projects, and we ended up deciding to work with Liberapay because it met our goals. We wanted a project that was interesting, active, popular, and comprised of components that we can build security requirements around. Liberapay currently has about 477 contributors, which indicated to us that it was both active and popular. As addressed in some of the other sections in this project proposal, Liberapay is a platform that facilitates donations to organizations. Liberapay consists of components that are exposed in some fashion to the internet, network, or other systems providing a place to look for security flaws. The nature of this project will allow us to build some interesting security requirements and perhaps find some security concerns as well.

## 3. Project Description
Liberapay is a non-profit organization that enables individuals to support the people who build free software and spread free knowledge by donating toward their work. As an organization, Liberapay provides an accessible web platform where people can donate money recurrently to various teams, organizations, and individuals and crowdfund workable incomes for these creators to accomplish their work. Liberapay’s users are composed of both donors and creators, and all processing payments are currently supported through Stripe and PayPal.

As an open-source software project, Liberapay has a well-established codebase that is currently maintained by a large development team of 477 contributors with 17,280 commits made to the project. The source code for this project is primarily written in Python, but the codebase also uses other programming languages such as SQL, HTML, JavaScript, Shell Script, Make, XML, and CSS. Since Liberapay was launched 5 years ago, this project has generated a high amount of user activity and reported the following statistics as of September 9th, 2020:
- 32,988 Open individual and organization accounts currently exist on Liberapay
- 5,144 Active users gave/received money within Liberapay for the latest week
- The sum of active donations for the latest week was $6,941
- The sum of donor charges processed for the latest week was $6,930

## 4. License and Contribution Policies
The Liberapay project has very lax licensing and contribution policies. The project is listed with the CC0 Creative Commons License, which allows the code to be used wherever and has little to no protections for the project. This will allow us to examine and contribute to the project freely. The license has allowed the project to be extremely free with regards to the contributors that help develop the project, with over 1000 issues and over 900 pull requests. The project has no official contributor agreement or processes. The primary way that individuals contribute to the project is by raising issues and submitting pull requests that are approved by a smaller core of developers. Other than developer oversight, no restrictions are made to submit issues and pull requests.

## 5. Summary of Software Security History
In this section, we highlight the software security history of Liberapay. The history can be divided into three main categories: Vulnerabilities, Security-Related Engineering Decisions, and Added/Removed Changes.

<ins>Vulnerabilities</ins>

Liberapay has a bug bounty program on the platform, HackerOne, that allows users to report potential issues. Along with the issues tab on their GitHub repository, some of the security issues were also reported from the bug bounty. Here are a few of them:

* [Missing back-end user input validation can lead to DOS flaw](https://hackerone.com/reports/361337)
* [Broken Authentication and session management OWASP A2](https://hackerone.com/reports/449671)
* [OAuth credentials are stored in the DB instead of the secrets vault](https://github.com/liberapay/liberapay.com/issues/1673)
* [Storing redirect URLs in the path and querystring is a security risk](https://github.com/liberapay/liberapay.com/issues/496)
* [Usage of 'pickle' is potentially dangerous](https://github.com/liberapay/liberapay.com/issues/1132)
* [SQL injection vulnerability in liberapay.com](https://github.com/liberapay/liberapay.com/issues/559)
* [Email address is exposed in verification URL](https://github.com/liberapay/liberapay.com/issues/492)
* [Paying failures aren't handled properly](https://github.com/liberapay/liberapay.com/issues/249)

<ins>Security-Related Engineering Decisions</ins>

Liberapay has set up some security-related engineering to assure security as best as possible. Here are a few of them:

* All network connections are encrypted, except for some communications between machines located in the same datacenter.
* As a precaution against identity theft in case of data leak, the identity information of Liberapay account owners is stored encrypted in their database.
* When a user inputs a password, it gets cross-checked against a "pwned passwords" to assess compromisation of password.
* A generic rate limit has been implemented for "unsafe" HTTP requests, among other components.
* Content Security Policy (CSP) was implemented to prevent cross-site scripting, prevent mixed content issues, as well as report violations for investigation.
* Liberapay does not tell creators who their patrons are to avoid personal information leaks.
* Liberapay allows users to link other social media to their accounts, but only store public information. No private data is stored.
* Liberapay restricts cookies to same-site requests as an additional protection against CSRF and enforcing privacy by preventing tracking of users through those widgets.

<ins>Added/Removed Changes</ins>

Along with fixing issues, Liberapay has also taken some measures to improve security. Here are a few of them.

* [Moved from python2 to python 3](https://github.com/liberapay/liberapay.com/pull/1399)
* [Replaced 'pickle' library in python to CBOR to avoid Remote Code Execution vulnerability through code injection](https://github.com/liberapay/liberapay.com/pull/1455)

## 6. Project GitHub Links
Here are links to our Github Pages: \
Master branch: [Home Page](https://github.com/JustinRobbins7/CSCI-8420-Team-1) \
Issues Page: [Issues Page](https://github.com/JustinRobbins7/CSCI-8420-Team-1/issues) \
Project Page: [Project Board](https://github.com/JustinRobbins7/CSCI-8420-Team-1/projects/1) 

Contribution Records: \
Github Pulse: [Pulse Page](https://github.com/JustinRobbins7/CSCI-8420-Team-1/pulse) \
Github Contributors: [Contributors Page](https://github.com/JustinRobbins7/CSCI-8420-Team-1/graphs/contributors) 

## 7. Teamwork Reflection
While determining which open-source software project to investigate for the course, each team member was tasked with researching three different software project ideas and reporting them back to the team for discussion. During the team meeting, everyone presented their software project ideas so that our team could collectively evaluate the pros and cons for each project and then agree upon which selection to choose from. Based upon our brief analysis of the different project selections, our team decided to use Liberapay as the open-source software project for further investigation in this course. While no major issues occurred during the project proposal phase, our team plans to continue making important decisions collaboratively as a group. This includes gathering together at least once per week on a Zoom call in order to report our progress with each other, and to keep everyone informed about the latest tasks that need to be completed.
