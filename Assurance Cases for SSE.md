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
![Organization Diagram](/Images/Assurance_Case_Diagrams/Miti_AC_4.png)
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

#### *2.4.1 Available Evidence*
*E1 & E10* \
An argument for the claims "Liberapay has account protections" and "Liberapay reduces unauthorized acccess to data" can be found in section(s) 1.1 and 1.2 above.

*E2* \
Evidence exists to suggest that Liberapay currently implements organization level acccount protections within their participant system backend service. This involves a mix of permission restrictions and validations centered around 'eslewhere accounts', 'duplicate accounts', and 'elevation' restrictions.
- Backend system for participant code (including account protections) [Account Protections](https://github.com/liberapay/liberapay.com/blob/ca814ed2478108b119fef4498a8ee521a5553914/liberapay/models/participant.py)

*E3*\
Liberapay requires business accounts to be validated when registering with the service. This process is evident when trying to elevate an individual account to an organization level account.
- Validation simplate: [Validate Simplate](https://github.com/liberapay/liberapay.com/blob/89cad02be0bdc42eafb02df2e38aff6535339095/www/%25username/identity-docs.spt)
- Business signup page (note: only available with account) [Business Signup](https://liberapay.com/mmrdy/receiving)

*E4*\
Liberapay implements logging of donation histories for an organization/account. Their implementation allows for histories of organizations created by elevating individual accounts as well as newly formed organizations.
- Python implementation of history tracking [History](https://github.com/liberapay/liberapay.com/blob/40454cf8d899d08f8fe28559fc40a685a4033f94/liberapay/utils/history.py)

*E5*\
Liberapay incorporates viewing an organizations donation history over its entire lifecycle. This can be achieved either through viewing the organization card on the search page, or by viewing an organizations income history within its profile page.
- View of multiple organizations and income per week on their cards [Income organization](https://liberapay.com/explore/organizations)
- Select "view income history" at the bottom of the page [Organization donation history](https://liberapay.com/LiberapayOrg/)

*E6*\
Liberapay leverages quarantining of payments in order to protect against prematrue disbursement as well as transaction related fraud activities. This protection feature is evident through test cases as well as a simplate. The simplate is utilized by Liberapay to send standardized emails to users notifying them of quarantined funds. 
- Test case showcasing quarantining feature [Quarantine Test](https://github.com/liberapay/liberapay.com/blob/0ec40ad7527bbb446f59c61e53d856352b7a6d1e/liberapay/testing/__init__.py)
- Notification simplate for quarantined funds warning [Quarantine notification](https://github.com/liberapay/liberapay.com/blob/a5290b528c5d7e0f2191208305eb3d1b1b8889ad/www/about/money.spt)

*E8*\
Team Dashboards

*E9*\
Paypal/Stripe Handler

Membership simplate
https://github.com/liberapay/liberapay.com/blob/3c77591ccf38093732c555ff588b62932857a9cc/www/%25username/membership/%25action.spt
#### *2.4.2. Unavailable/Insufficient Evidence*
*E7*\
Account activity



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
