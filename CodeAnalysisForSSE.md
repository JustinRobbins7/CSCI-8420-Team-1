# Team 1 Liberapay Code Analysis

Written by Team 1:
Joel Allou, Mohammad H. Al Huneidi, Miti Mareddy, Justin Robbins, and Nicholas Sabata

Link to Liberapay Github: [Liberapay Project](https://github.com/liberapay/liberapay.com)

## 1. Code Analysis

### 1.1. Summary of Code Review Strategy

Our code review strategy placed an emphasis on manual code review based on possible vulnerabilities found in previous work on the Liberapay system. Nevertheless, we still performed some automatic code review to see if the tools could find any relevant vulnerabilities that we had missesd. Liberapay utilizes Python in most of its code base, so we expected the response from most tools to be lightweight and less helpful due to the lack of restrictions imposed in Python code. As a result, we focused our efforts on manual code review to locate code relevant to our previously examined vulnerabilities.

The code review process could be described as follows:
  1. Examine previous artifacts for vulnerability archetypes. (XSS, improper authentication, etc.)
  2. Match these vulnerabilities with CWE weakness classes to create a checklist for manual review.
  3. Work through the checklist, examining the Liberapay code base for code artifacts either addressing the weakness or causing it.
  4. Record these sections of code for later modification and communication with the project managers.
  5. Run automatic scanning tools to search for any other security flaws missed in the manual code review.
  6. Collect and report the results of both the manual and automatic code reviews.

### 1.2. Findings from Manual Code Review

#### 1.2.1. CWE Weakness Checklist

Based on previous work, this weakness checklist lists the 12 CWE weaknesses we used to evaluate Liberapay's code base during our code review:
  1. CWE-20: Improper Input Validation
  2. CWE-79: Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')
  3. CWE-94: Improper Control of Code Generation (Code Injection)
  4. CWE-291: Reliance on IP Address for Authentication
  5. CWE-307: Improper Restriction of Excessive Authentication Attempts
  6. CWE-308: Use of Single-factor Authentication
  7. CWE-312: Cleartext Storage of Sensitive Information
  8. CWE-319: Cleartext Transmission of Sensitive information
  9. CWE-350: Reliance on Reverse DNS Resolution for a Security-Critical Action
  10. CWE-602: Client-Side Enforcement of Server-Side Security
  11. CWE-614: Sensitive Cookie in HTTPS Session Without 'Secure' Attribute
  12. CWE-770: Allocation of Resources Without Limits or Throttling

The checklist was produced from previously identified candidate vulnerabilities. Weaknesses 1, 2, and 3 were generated from the thought that Liberapay may not be properly handling user input, leading to possible vulnerabilities via code injection or similar XSS attacks. Items 7, 8, and 11 were generated from the risk the Liberapay may not be properly encrypting or otherwise protecting user communications and session data. Risks related to possible spoofing attacks matched with the CWE weaknesses 4, 9, and 10. Finally, various weaknesses that reduce the system's effectiveness against brute force attacks led to the inclusion of items 5, 6, and 12 on the checklist. These weaknesses formed the foundation of the manual code review performed on Liberapay. 

#### 1.2.2. Manual Code Review Findings



### 1.3. Findings from Automatic Tools

[Bandit Scan](/Images/Automated_Scan_Bandit.pdf)

Found new issue: 
  - CWE-916: Use of Password Hash with Insufficient Computational Effort

## 2. Key Findings and Contributions

### 2.1. Summary of Key Findings



### 2.2. Planned / Ongoing Contributions to Upstream Projects



## 3. GitHub Information

Here are links to our Github Pages: \
Master Branch: [Home Page](https://github.com/JustinRobbins7/CSCI-8420-Team-1) \
Issues Page: [Issues Page](https://github.com/JustinRobbins7/CSCI-8420-Team-1/issues) \
Project Board: [Project Board](https://github.com/JustinRobbins7/CSCI-8420-Team-1/projects/3)

Contribution Records: \
Github Pulse: [Pulse Page](https://github.com/JustinRobbins7/CSCI-8420-Team-1/pulse) \
Github Contributors: [Contributors Page](https://github.com/JustinRobbins7/CSCI-8420-Team-1/graphs/contributors) 

## 4. Teamwork Reflection
For the code analysis/review, team members coordinated roles and tasks beforehand and set out to tackle this deliverable in an organized way. After our team check-in we had a better understanding of tasks for the deliverable and goals to achieve. We met an additional time prior to the deadline to review team tasks and WIP left to complete. We also approached code analysis from a combined automated and manual perspective. After reviewing automated results, further tasks were delegated. The assignment progressed with no major conflict or impediments after this point.

For the next deliverable, we plan to continue our weekly team meetings and keep everyone up-to-date on both GitHub and other communication platforms. Completing this deliverable allowed the team to gain a better understanding of the overall project and have a unified frame of reference for the upcoming presentation.
