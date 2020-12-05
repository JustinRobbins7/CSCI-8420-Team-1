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
  7. CWE-312: Cleartext Storage of Sensitive Information:
  8. CWE-319: Cleartext Transmission of Sensitive information
  9. CWE-350: Reliance on Reverse DNS Resolution for a Security-Critical Action
  10. CWE-602: Client-Side Enforcement of Server-Side Security
  11. CWE-614: Sensitive Cookie in HTTPS Session Without 'Secure' Attribute
  12. CWE-770: Allocation of Resources Without Limits or Throttling

The checklist was produced from previously identified candidate vulnerabilities. Weaknesses 1, 2, and 3 were generated from the thought that Liberapay may not be properly handling user input, leading to possible vulnerabilities via code injection or similar XSS attacks. Items 7, 8, and 11 were generated from the risk the Liberapay may not be properly encrypting or otherwise protecting user communications and session data. Risks related to possible spoofing attacks matched with the CWE weaknesses 4, 9, and 10. Finally, various weaknesses that reduce the system's effectiveness against brute force attacks led to the inclusion of items 5, 6, and 12 on the checklist. These weaknesses formed the foundation of the manual code review performed on Liberapay. 

#### 1.2.2. Manual Code Review Findings


7. __CWE-312: Cleartext Storage of Sensitive Information:__

An analysis has been conducted on the parts of the Liberapay code that deal with database storage to determine whether they're storing any sensitive information without encrypting it. The result of the manual code review concluded that Liberapay utilizes: __pbkdf2_hmac__ encryption which is a type of encryption available from the hashlib python library. Sensitive data gets encrypted with it before it's inserted into the database. We've researched this encryption algorithm to determine it's encryption capabilities and we found the following description of it in the [python hashlib documentation](https://docs.python.org/3/library/hashlib.html) "The function provides PKCS#5 password-based key derivation function 2. It uses HMAC as pseudorandom function." Hence we've concluded that CWE-312 is addressed adequately. 


__8. CWE-319: Cleartext Transmission of Sensitive information:__

Liberapay communicates with two external systems with sensitive information. Those two external systems are Paypal and Stripe. An analysis on the code that communicates with Paypal and Stripe has been conducted to determine the mechanism of communciation and whether it poses any security concerns. Liberapay leverages makes requests to restful APIs exposed by Paypal and Stripe. The API's are of type "POST, and Liberapay passes in the request body some data about the order. As defined in the Paypal/Stripes API specs. The notable information being passed include the donator's user name, transaction amount, and the team/user name he donated to. This information does not include any monetary concerns or sensitive information per say such as a password. The actual transaction is conducted on Paypal/Stripe and Liberapay simply acts as a delegator. However one could argue that the donation that is happening could be a piece of information that a user wishes to keep private hence it's important that the request body is encrypted in some fashion to protect it from potential attackers. Going back to the fact that the communication is conducted via RESTful APIs. Those APIs use HTTP and support Transport Layer Security (TLS) encryption. TLS is a standard that keeps an internet connection private and checks that the data sent between two systems (a server and a server, or a server and a client) is encrypted and unmodified. Since Liberapay is using restful services to communicate with Paypal and stripe. We deem that the communication is safe and Liberapay has sufficient protections in place.

### 1.3. Findings from Automatic Tools
Our team utilized two distinct tools to try and locate common security issues/vulnerabilities in LiberaPay.

**SonarQube**:

We've setup a SonarQube server along with a Python plugin. We used a Python  plugin because the Liberpay code base is comprised predominantely of Python. And consequently all of the logic that deals with security related concerns is written in Python. We also installed a SonarQube Python-Based scanner to conduct a scan on the Liberapay code base. The scanner generated a report that we accessed through the SonarQube server. Over 16000 lines of code were scanned by the Python scanner. The findings of the scanner did not indicate any vulnerabilities in the code. Attached below is a screenshot of the result. However it did detect three bugs in the codebase, we've decided to manually validate the bugs to ensure that they did not pose any security concerns. But we deemed that those bugs are just related to code maintainability and do not pose any security threats.

![SonarQube Scan Results](/Images/sonarqube-scan-result.png)


**Bandit**

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
