# Team 1 Requirements for Software Security Engineering: Liberapay

Written by Team 1:
Joel Allou, Mohammad H. Al Huneidi, Miti Mareddy, Justin Robbins, and Nicholas Sabata

Link to Liberapay Github: [Liberapay Project](https://github.com/liberapay/liberapay.com)

## 1. Use/Misuse Case Analysis

### 1.2. Use/Misuse Case

### 1.1 - Customer Login

#### *Use Case*

<ins>Goals/Description:</ins>
Provide authentication mechanism.

<ins>Scenario Example:</ins>
A user logs in into his/her Liberapay's account.

<ins>Description</ins>
- A user visits Liberaypay's website
- The user enters email and password
- The systems validates credentials and accept and reject request

#### *Mis-Use Case(s)*

Below, we detail couple of ways that a hacker tries to infiltrate a user's account.

1) The hacker attempts to login into a user's account through brute force method.
The hacker uses system to automatically checks all the combinations of password against the database and manages to infiltrate the user's account.

2) If the first method doesn't work, the hacker tries to analyze packets communications between the client and server and modifies the packet to gain entry.

3) If the second method doesn't work, the hacker then tries to perform SQL Injections on all the queries and tries to see if he/she is able to infiltrate with that method.

4) Along with the SQL Injections, the hacker also tries to access the database and manipulate query string to alter information in the database.

5) If all of the methods above fail, the hacker tries to analyze user permissions and see if he/she can access the database through weak permissions settings.
If entry is successful, the hacker then alter information in the database.

#### *Security Requirements*

<ins>Derived Security Features</ins>

Below, we detail security measures that must be taken to mitigate hacker's success in accessing a user's account. The number of each security measure corresponds to the misuse case number in the misuse case section.

1) There should be password policies set in place to rate limit login attempts, ensure that user enters strong password, and observe unusual activities.

2) All communications between server and client should be secure by using protocols like SSL and firewalls, in order to mitigates issues like packet sniffing.

3) Every input sent to the server or DB should be verified and confirmed before moving to the next step. Each requests that is malicious should automatically be revoked and flagged.

4) Same as above.

5) Permissions for access/editing of database should be properly defined using standard policies.

<ins>Current Security Features</ins>

Below, we detail existing security measures that Liberaypay has taken to mitigate issues discussed above. The number of each security measure corresponds to the misuse case number in the misuse case section.

Replaced 'pickle' library in python to CBOR to avoid Remote Code Execution vulnerability through code injection

1) To mitigate brute force password checking, Liberapay has taken the following measures:
 - All passwords get cross-checked agains a "pwned passwords" ([Link to Issue](https://github.com/liberapay/liberapay.com/issues/986))
 - A generic rate limit has been implemented for "unsafe" HTTP requests, among other components. ([Link to Issue](https://github.com/liberapay/liberapay.com/issues/658))
2) [Liberay](https://liberapay.com/about/privacy) affirms that "All network connections are encrypted, except for some communications between machines located in the same datacenter".

3) ...

4) ...

5) ...

#### *(Mis)use case Diagram*
 
 ![Diagram 1](/Images/RequirementsDiagram1.png)

### 1.2. Donation Renewal

#### *Use Case*
<ins>Goals/Description:</ins>
Donator renews donation

<ins>Scenario Example:</ins>
A user is sent a donation renewal email and logs into their account to pay.

<ins>Description</ins>
- A user is sent an email by Liberapay
- The user enters email and password into Liberapay
- The user enters email and password into their chosen payment system

#### *Mis-Use Case(s)*

Below, we detail couple of ways that an attacker tries to acquire and use user information.

1) The attacker attempts to phish a user's password by sending them a phony donation email that links to a site where they can steal the user's information. To extends this method, the attacker can duplicate Liberapay's existing standard email template to appear more authentic.

2) If the first method doesn't work, the attack can try to brute force the user's password and login through a dictionary attack.

3) If the second method doesn't work, the attacker can try to wide nthe scope of the attack to other website the user may use, trying to exploit weaknesses in their systems.

4) If the first or third method works, the attacker tries to login in with the stolen username and password information and access the user's account.

5) If the fourth method fails, the attacker tries to steal a session cookie with an XSS scripting attack to spoof an authentic user session.

#### *Security Requirements*
<ins>Derived Security Features</ins>

Below, we detail security measures that must be taken to mitigate and prevent the attacker's success in accessing a user's account.

1) Liberapay should use a standard, official-looking emailing template to mitigate the chance that users mistake a poorly crafted email for a Liberapay email.

2) Liberapay should have some system in place to help users verify that their emails are from the Liberapay notification system.

3) There should be password policies set in place to limit the amount of login attempts that can be attempted in a short amount of time.

4) Liberapay should implement two-factor authentication to prevent malicious users from accessing a user's account even if they successfully acquire their login information.

5) Liberapay should implement timed session cookies to mitigate the time malicious users can use stolen session cookies to spoof a fake logged-in user session.

6) Liberapay should implement systems to prevent XSS attacks from bypassing their security to access users' private data and sessions.

<ins>Current Security Features</ins>

1) Liberapay utilizes a standard email format since they automatically send emails to recurring donators.

2) Liberapay does not have any official emailing authentication method. This is somewhat expected, since all this requirement would do is mitigate the issue, not prevent it.

3) Liberapay does limit the amount of login attempts a single user can perform; once this occurs, users can only login through emails sent by Liberapay when requested.
     - The issue [here](https://github.com/liberapay/liberapay.com/issues/1609) briefly discusses how Liberapay handles limited logins.

4) Liberapay does implement a time-based one time password algorithm to add 2FA to their system, dramatically improving login security in the face of stolen login data.
     - The issue [here](https://github.com/liberapay/liberapay.com/issues/926) shows the debate on which method of 2FA Liberapay was to implement.
     
5) Liberapay does not seem to implement timed session cookies.

6) Liberapay has resolved a lot of issues related to XSS attacks; however, a few known attack vector still remain, and could be used to access data unintended or most users.
     - The issue page [here](https://github.com/liberapay/liberapay.com/issues?q=xss) displays the issues related to past and current XSS attack issues.
     
#### *Written Summary*
This use case focuses around a user being notified that a donation they have chosen to start is about to recur. Liberapay allows for both automatic withdrawal from accounts and the manual payment of recurring donations. The Liberapay scheduler tells the email system to send reminder emails to donators when the time comes to pay their donation once again. While this use case primarily works under the assumption that the user has set up manual payments, the phishing emails are also applicable to the automatic payments as reminder emails are sent regardless of the type of repayment. Once the email is received, a donator using manual repayment must login to Liberapay and then login to their chosen payment handler. 

The misuse case for this use case focuses on an attacker trying to leverage the email system and other methods to acquire the login information of the donator and to use it to login to their Liberapay account, or possibly their payment vendor account if they use the same email and password. Sending a phishing email to trick users into giving the attacker their login data is an old trick, but still works on many users. To mitigate this, Liberapay could use a standardized emailing template to make sure that their emails look professional and ensure that their emails are easily recognizable by donators. Unfortunately, this kind of standardized emailing also lends itself to duplication if the attacker has access to the template. Liberapay would then need to have some form of authentication to ensure that hackers cannot duplication their emails and fool consumers. Unfortunately, phishing countermeasures will only mitigate the danger of users being fooled by phony emails. Other ways the attacker could acquire this login information is by executing a dictionary attack / brute force attack on the donator's login account to simply guess their password. Implementing a limit on login attempts heavily mitigates this problem. However, if the attacker executes the attack on other websites, they may still be able to acquire login information if the user uses the same password on multiple web sites. This sort of attack cannot be prevented by Liberapay, because it reaches web sites otuside of the system's operations. 

Despite this, Liberapay can prevent an attacker from logging in with stolen user data. Implementing two-factor authentication ensures that the donator's account cannot be accessed even with their account information, as long as their secondary security device is uncompromised. The final interactions in this misuse case deal with the attacker trying to leverage an XSS attack to acquire the session cookies from a legitmate user, and then use that session cookie to access the donators' account directly, without needing to authenticate via 2FA. By implementing measures that mitigate or prevent XSS attacks, Liberapay can protect their users from such an attack. Liberapay can also mitigate this issue by only using timed session cookies that expire after a short time, makign stolen cookies useless shortly after they are acquired. 

In summary, this misuse case derives the following security requirements: login authentication, standardized emailing, email authentication, login attempt limits, two-factor authentication, short timed session cookies, and XSS attack countermeasures. Liberapay support basic login information, storing this information in an encrypted database. Liberapay also limits the attempts that can be made to log in from a certain device, preventing login for a few hours once a login has failed. The issue [here](https://github.com/liberapay/liberapay.com/issues/1609) briefly discusses how Liberapay handles limited logins, by forcing a user to directly login with their email if they make too many failed attempts. Liberapay implements a [timed-based one time password algorithm](https://github.com/liberapay/liberapay.com/issues/926) to include two-factor authentication in their system, significantly increasing the security of Liberapay. Unfortunately, it seems that while [many](https://github.com/liberapay/liberapay.com/issues?q=xss) XSS attack-related issues have been dealt with, there are still some attack vectors that have not been plugged. These issues could allow attackers to bypass the other security measures of Liberapay and access customer sessions. Liberapay does not satisfy the timed session cookie security requirement or the authenticated email requirements. The latter is understandable, as the resources put into the project would be wasted by sufficiently ignorant donators. However, Liberapay does use automated, and thus standardized emailing, so their messages are recognizable.

#### *(Mis)use case Diagram*
![Donation Renewal Misuse Case Diagram](/Images/LiberapayDonationRenewalUseCase.png)
 



### 1.3. Use/Misuse Case


#### *Use Case*

<ins>Goals/Description:</ins>
The user's goal here is to sign up on LiberaPay .

<ins>Scenario Example:</ins>
A user visits LiberaPay to create an account

<ins>Description</ins>
- User visits LiberaPay
- The user utilizes the sign up feature to create an account
- The user provides his email to create his account and clicks on sign up.

#### *Mis-Use Case(s)*

Below, we detail a way in which an attacker can abuse the sign up feature to flood the database with spam accounts.

1) The hacker utilizes the pre-existing sign up feature available on LiberaPay

2) The hacker utilizes a script to create lots of accounts. 

3) If the system prevents the hacker from creating the accounts, then the hacker utilizes a VPN.

#### *Security Requirements*

<ins>Derived Security Features</ins>

Below, we detail security measures that must be taken to prevent the above from occuring. 

1) There should be a limit to how many accounts a person can be created from one IP Address in a certain time frame.

2) There should be a limit to how many accounts which can be created globally in a certain time span.  

3) Captcha technology should be utilized to slow down and prevent scripts/bots from flooding spam accounts.

4) LiberaPay should contain spam removal jobs that remove from their database spam accounts by analyzing patterns.

<ins>Current Security Features</ins>

From our analysis it appears that LiberaPay does not currently implement any measures to prevent flooding of database with spam accounts.



![Signup Flood Attack Misuse Case Diagram](/Images/Sign%20up%20flood%20misuse%20case.png)

### 1.4. Use/Misuse Case

#### *(Mis)use case Diagram*
 
![Diagram 4](/Images/SA%20Organization%20Process%20(1).png)

### 1.2. Donation Renewal

#### *Use Case*
<ins>Goals/Description:</ins>
The Organization receives a payment.

<ins>Scenario Example:</ins>
An organization sets up a team and receives a payment.

<ins>Description</ins>
- An organization sets up an account.
- An organization receives a donation.

#### *Mis-Use Case(s)*

The following details ways a '_Fraudulent Actor_' tries to access an organizations payments:

1) 

#### *Security Requirements*
<ins>Derived Security Features:</ins>

The following details security measures to counter a successful attack in accessing an organizations payments:

1) 

<ins>Current Security Features:</ins>

1) 
     
#### *Written Summary*






### 1.5. Use/Misuse Case

## 2.1 Liberapay Documentation Review
 - Review OSS project documentation for security-related configuration and installation issues. Summarize your observations.

## 2.2. GitHub Information
Here are links to our Github Pages: \
Master Branch: [Home Page](https://github.com/JustinRobbins7/CSCI-8420-Team-1) \
Issues Page: [Issues Page](https://github.com/JustinRobbins7/CSCI-8420-Team-1/issues) \
Project Board: [Project Board](https://github.com/JustinRobbins7/CSCI-8420-Team-1/projects/2) 

Contribution Records: \
Github Pulse: [Pulse Page](https://github.com/JustinRobbins7/CSCI-8420-Team-1/pulse) \
Github Contributors: [Contributors Page](https://github.com/JustinRobbins7/CSCI-8420-Team-1/graphs/contributors) 

## 2.3. Planning and Reflection

 Our team continued to collaborate, and conduct meetings once or twice a week to report progress. We collectively analyzed LiberaPay in greater detail and each team member was tasked with preparing a misuse case. During our weekly scheduled meetings we evaluated and provided constructive feedback to each other's work to make sure that we are all on track. We've realized after meeting with Dr. Robin Ghandi that we needed to simplify some of our diagrams. No major issues occurred during this phase and the team plans to continue conducting weekly meetings to stay on track and resolve any blockers.
 
