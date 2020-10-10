# Team 1 Project Proposal: Liberapay

Written by Team 1:
Joel Allou, Mohammad H. Al Huneidi, Miti Mareddy, Justin Robbins, and Nicholas Sabata

Link to Liberapay Github: [Liberapay Project](https://github.com/liberapay/liberapay.com)

## 1. Claims

### 1.1. Claim 1

### 1.2. Claim 2 - Liberapay minimizes unauthorized account access
![Account Access Diagram](/Images/Assurance_Case_Diagrams/Justin_Robbins-Assurance_CaseV2.jpeg)

### 1.3. Claim 3

### 1.4. Claim 4
- Miti
### 1.5. Claim 5


## 2. Evidence Alignment

### 2.1. Claim 1 Evidence Alignment

### 2.2. Claim 2 Evidence Alignment

#### *2.2.1 Available Evidence*
*E2* \
Liberapay leverages simplates to generate many different types of emails for different situations. These simplates act as standardization that can help reduce the chance that poorly constructed phishing emails will be able to spoof Liberapay's emails.
- Email Simplates: [https://github.com/liberapay/liberapay.com/tree/master/emails](https://github.com/liberapay/liberapay.com/tree/master/emails)

*E3* \
Liberapay implements login limits true rate limiting, restricting the number of times password login, email login, account verifications, and account creations that can be performed in an hour. This ensured that attackers cannot merely guess the login information of a user via brute force. Full details about the policy can be seen in the below issue documentation.
- Liberapay rate limiting: [https://github.com/liberapay/liberapay.com/pull/699](https://github.com/liberapay/liberapay.com/pull/699)

*E4, E6, and E9* \
Liberapay leverages https, ensuring that their communications are encrypted according to the TLS encryption algorithm. This also means that their session keys are generated from random data at the time of session initialization. These keys are also short-lived, ensuring that even if the attackers acquire a key through theft, they will not be able to use it for long.
- Liberapay website (notice the use of https): [https://liberapay.com/](https://liberapay.com/)

*E5* \ 
An argument for the security of Liberapay's sensitive can be found in sections 1.1. and 2.2. of this paper.



#### *2.2.2. Unavailable/Insufficient Evidence*
*E1 and E7* \
Liberapay is in the process of implementing a time-based one time password two factor authentication system. When it is complete, it will greatly improve the security of Liberapay's login services. Until it is, attackers with user login information are not prevented from logging in using stolen user credentials. This represents a significant threat to Liberapay, as many users can be prone to using the same password across multiple web sites, whose security cannot be guaranteed by Liberapay.
- Two-factor authentication issue on Liberapay's GitHub: [https://github.com/liberapay/liberapay.com/issues/926](https://github.com/liberapay/liberapay.com/issues/926)

*E8* \
Liberapay's implementation of logging in and email sending does actually send links that the user can use to log into the web site automatically. These links are primarily used for account recovery and verification. These emails are only sent on request, so users will not receive email with links constantly. However, they still represent a gap between the diagram and reality, as these links do require the user to insert login information afterward. This represents a possible vector for spoofing a recovery or verification email to acquire user login data. Additionally, recurring donation reminders also contain links to the pages of the organization the user agreed to donate to, providing another possible attack vector. Overall, the emails could be made more secure bby minimizing the amount of links, although I doubt the project leaders would like to trade the user friendliness of the current system for a more secure system with minimized links.
- Donation reminder links: [https://github.com/liberapay/liberapay.com/blob/master/emails/donate_reminder~v2.spt](https://github.com/liberapay/liberapay.com/blob/master/emails/donate_reminder~v2.spt)
- Account recovery / login request email: [https://github.com/liberapay/liberapay.com/blob/master/emails/login_link.spt](https://github.com/liberapay/liberapay.com/blob/master/emails/login_link.spt)
- Account verification email: [https://github.com/liberapay/liberapay.com/blob/master/emails/verification.spt](https://github.com/liberapay/liberapay.com/blob/master/emails/verification.spt)

*E10* \
The Liberapay project has a long history of trying to plug XSS vulnerabilities in their web site, and have many issues on their project page because of it. The data is there, but it also shows that there are a few XSS issues that remain, representing a gap between the diagram and the actual evidence.
- XSS issues page: [https://github.com/liberapay/liberapay.com/issues?q=xss+
](https://github.com/liberapay/liberapay.com/issues?q=xss+
)

*E11* \
Unfortunately, it seems that no concerted effort has been made to reduce the amount of vulnerable dynamic HTML used in the website. This means that no report has been created on the analysis of Liberapay's use of dynamic HTML. Likely some of the vulnerabilities have been patched as a result of the XSS vulnerability plugging already done on the website, but the use of dynamic HTML is not a focus of Liberapay's development efforts. This lack of documentation conflicts with the assurance case in claim 2.


### 2.3. Claim 3 Evidence Alignment

### 2.4. Claim 4 Evidence Alignment

### 2.5. Claim 5 Evidence Alignment

## 3. GitHub Information

Here are links to our Github Pages: \
Master Branch: [Home Page](https://github.com/JustinRobbins7/CSCI-8420-Team-1) \
Issues Page: [Issues Page](https://github.com/JustinRobbins7/CSCI-8420-Team-1/issues) \
Project Board: [Project Board](https://github.com/JustinRobbins7/CSCI-8420-Team-1/projects/3)

Contribution Records: \
Github Pulse: [Pulse Page](https://github.com/JustinRobbins7/CSCI-8420-Team-1/pulse) \
Github Contributors: [Contributors Page](https://github.com/JustinRobbins7/CSCI-8420-Team-1/graphs/contributors) 


## 4. Teamwork Reflection
