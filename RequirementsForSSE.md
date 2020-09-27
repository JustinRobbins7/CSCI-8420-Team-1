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

1) At first, the hacker tries to use existing leaked password information to see if he/she can access the user's account.

2) If the first method doesn't work, the hacker tries to analyze packets communications between the client and server and modifies the packet to gain entry.

3) a) If the second method doesn't work, the hacker then tries to perform SQL Injections on all the queries and tries to see if he/she is able to infiltrate with that method.
   b) Along with the SQL Injections, the hacker also tries to access the database and manipulate query string to alter information in the database.

4) If all of the methods above fail, the hacker tries to analyze user permissions and see if he/she can access the database through weak permissions settings.
If entry is successful, the hacker then alter information in the database.

#### *Security Requirements*

<ins>Derived Security Features</ins>

Below, we detail security measures that must be taken to mitigate hacker's success in accessing a user's account. The number of each security measure corresponds to the misuse case number in the misuse case section.

1) There should be password policies set in place to ensure that user enters strong password, check against leaked password, and observe unusual activities.

2) All communications between server and client should be secure by using protocols like SSL and firewalls, in order to mitigates issues like packet sniffing.

3) Every input sent to the server or DB should be verified and confirmed before moving to the next step. Each requests that is malicious should automatically be revoked and flagged.

4) Permissions for access/editing of database should be properly defined using standard policies.

<ins>Current Security Features</ins>

Below, we detail existing security measures that Liberaypay has taken to mitigate issues discussed above. The number of each security measure corresponds to the misuse case number in the misuse case section.

Replaced 'pickle' library in python to CBOR to avoid Remote Code Execution vulnerability through code injection

1) To mitigate against using leaked password information leak, Liberapay has taken the following measures:
 - All passwords get cross-checked agains a "pwned passwords" ([Link to Issue](https://github.com/liberapay/liberapay.com/issues/986))
 
2) [Liberay](https://liberapay.com/about/privacy) affirms that "All network connections are encrypted, except for some communications between machines located in the same datacenter".

3) There was an issue where a tester was able to inject SQL code. Although Liberapay deemed the impact as being low, they fixed the SQL interpolation to decrease that threat. ([Issue](https://github.com/liberapay/liberapay.com/issues/559))

4) Further inspection of the code base is needed to assure that permissions for access/editing are well defined.

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

### 1.4. Organization Donation Payment

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

1) Utilizing a compromised account/ known usernid & password information, the _fraudulent actor_ attempts to utilize existing account features to change an organizations account information.

2) If this attempt is thwarted, the _fraudulent actor_ attempts to pivot by modifying the original organization email effectively taking control of the account.

3) Barring this, the _fraudulent actor_ adds themselves as a teammember of the organization in order to receive a portion of donations received by the organization.

4) If the previous methods didn't work, the _fraudulent actor_ may attempt to directly change the payment address via a different process than account settings which would reroute any donations the organization receives.

5) If situation 4 is prevented, the _fraudulent actor_ may attempt to reroute payments via Liberapays API Widget — which acts as an extension of an organizations account.

6) If the method in situation 5 fails, the attacker attempts to modify the transaction ledger in order to divert funds away from the target organization.

7) Barring situation 6, the _fraudulent actor_ then attempts to intercept the payment information in an attempt to change the target address.

#### *Security Requirements*
<ins>Derived Security Features:</ins>

The following details security measures to counter a successful attack in accessing an organizations payments:

1) Liberapay's website should send account modification verification notifications to an organization in order to alert it of account changes.

2) Liberapay's notifications should be sent out to the organization head as well as 'team members' listed within the organization.

3) The site should provide a feature to see all teammembers listed within an organization as well as their activity details in a dashboard.

4) An organizations payment verification should be conducted from a payment-processor such as Stripe/PayPal/MangoPay as well as Liberapay itself.

5) An organizations Liberapay API key should be registered with their account, and only usable in a 'read-only' format.

6) Liberapay should implement a distributed transaction ledger shared across an organization to prevent changes/ modification to a ledger.

7) Liberapay should include strong encryption with payment related communications to prevent attackers intercepting payment information.

<ins>Current Security Features:</ins>

1) Liberapay doesn't currently support account modification notifications for individuals or organizations.

2) Liberapay sends payment notifications to an organizations team, but currently does not send team change notifications.

3) Liberapay supports team [dashboards](https://en.liberapay.com/about/teams) showing members and team history as well.

4) Liberapay currently supports using merchantIDs in order to enable payment account verifications on Stripe/PayPal.

5) Liberapay supports a read-only [API](https://github.com/liberapay/liberapay.com/issues/688), with APIs from Stripe/PayPal for payment processing.

6) Liberapay currently does not support a distributed/ duplicate transaction ledger across an organization. There is support for a duplicate register between Liberapay and a payment-processor such as Stripe/PayPal [suggested here](https://gitter.im/liberapay/salon) — however that is only accessible to the payment-processor account holder and not each team member.

7) Liberapay currently utilizes [Fernet](https://github.com/liberapay/liberapay.com/blob/master/liberapay/security/crypto.py) for symmetric encyption of all messages passed between it and payment-platforms.
     
#### *Written Summary*
   This case focuses on the scenario of using an organization account and receiving donations from that account. The associated misuse case gleaned from the regular use case for this scenario involves a _fraudulent actor_ who intends on diverting or siphoning payments from an organization. In order to achieve this goal, the attacker first attempts to exploit weaknesses in Liberapay's account management features. These include using a compromised account to change an organizations account information or changing the main email associated with an organizations account. These attack vectors would allow a _fraudulent actor_ to essentially gain control of an account or reroute payments to an alternative destination. Another extension of these attacks is the situation wherein an attacker adds themselves as a member of an organization's team. Since Liberapay doesn't set limits or defined payment allocations to individual teammembers, this type of attack would allow a _fraudulent actor_ to receive a cut of donations given to an organization. Barring these types of attacks, an attacker could always directly change the payment address. This setting is performed separately from account settings, and would allow all donations to reroute away from the intended payment accounts. An extension of this type of misuse is changing payment information via Liberapay's API Widget. If modifications to an organization fail, a _fraudulent actor_ could always change the transaction ledger itself, or intercept the payment information as it is communicated across.

   This misuse case generates the following security requirements: Account Change Notifications, Team-based Notifications, Team Dashboards, External Payment Verification, API Limitation, Distributed Transaction Ledgers, and Suitable Encryption. In providing notifications for changes in account settings unauthorized modifications could be caught and attacks related to compromised accounts could be mitigated. Similarly, having notifications sent out to all members within a team as opposed to an organization's primary contact would prevent a primary email being changed and thus locking out an organization. Having a team dashboard would allow for members of an organization to be aware of each other and everyone's contributions. A requirement to have an external payment verifications would mitigate and prevent misuse from the perspective of accepting payments. Similarly having a registered API key with an organization with 'read-only' capabilities prevents abuse of the LiberapayAPI for modifying transactions/ destination information. Having distributed ledgers and suitable encryption for payment information go hand-in-hand in preventing a _fraudulent actor_ from modifying or intercepting that type of information and siphoning funds away from an organization.

   Existing measure within Liberapay to address these security requirements include support for team [dashboards](https://en.liberapay.com/about/teams). These allow registered members of a team to view other members as well as participation history within that specific Liberapay organization. The platform also offers support for external payment verification via its partnered payment-platforms. The platform also currently supports a read-only [API](https://github.com/liberapay/liberapay.com/issues/688) with no native support for payment processing/ details. Liberapay also utilizes strong encryption protocols — specifically [Fernet](https://github.com/liberapay/liberapay.com/blob/master/liberapay/security/crypto.py) for symmetric encyption. There is still scope for Liberapay to implement features to address the other security requirements mentioned above. The platform does not support notifications for account changes, nor does it send notifications to all members of an organization. Liberapay also has scope to include a distributed/ duplicated ledger across an organization — where currently only a master copy is in use (refer [here](https://gitter.im/liberapay/salon) for community discussion for this topic).


#### *(Mis)use case Diagram*
 
![Diagram 4](/Images/SA%20Organization%20Process%20(2).png)


### 1.5. Use/Misuse Case

## 2.1 Liberapay Documentation Review

#### OSS Security Configuration


For the security configuration of the Liberapay, we will look at both the configurations that the system does as well as the configuration that the user can do.

#### System Security Configuration

 - Cryptography: 
  For handling security data, Liberaypay uses Symmetric encryption and decryption. According to the [comments](https://github.com/liberapay/liberapay.com/blob/master/liberapay/security/crypto.py) on their code base, they currently use Fernet. They also use Concise Binary Object Representation (CBOR) to serialize objects before encryption.

 - Security Headers: 
  Liberaypay also has several [security headers](https://github.com/liberapay/liberapay.com/blob/master/liberapay/security/__init__.py) configurations to prevent attakcs. Some of the headers they use is allowing CORS for assets, X-Frame Options to prevent click-jacking, CSP to prevent against code injection, and more.

#### User Security Configuration


   From the customer's point of view, Liberapay does not have a lot configureable items. One of the only thing that a customer can configure, however, is based on security and privacy. Liberapay allows customers to be able to define who can access their profile and information, which is not only a privacy concern but also a security one. Having ownership about viewership amplifies protection of assets and information. (See screenshot below):

![Security Config](/Images/privacysecurityconfig.PNG)

   There is a security-related configuration related to changin passwords that could be a risk factor. When the user changes passwords, Liberapay double checks the password against known vulnerable passwords that have been leaked. If the user's password turns out to be one of those vulnerable passwords, Liberapay warns the user, but still allows to move forward with that password. Although it is good to give user options, in this case, the user should NOT be given the option to move forward with a vulnerable password as that will immediately endager the customer's account, which could result in a seucurity breach. (See screenshot below):


 ![Password Config](/Images/passwordconfig.PNG)
 
 
### Documentation Issues

   Liberaypay is a website-based product and therefore does not require any installation. Configuration occurs within the client view as denoted in the section above. However, when using the application, there are various complaints that have come from customers about the lack of information and documentation available. Below, we highlight some of them:

 - Lack of information about pledges:
 Liberapay allows pledging to people who haven't joined the site yet. No money is collected for pledges, they only become real donations when the recipients join. This could be a security issue as if a hacker was able to access a user's account, they could use that feature and potentially steal money from the user.
 There was a customer who complained that they were not understanding how pledges work and that there was a lack of documentation about this topic, which unable them to proceed. [Issue](https://github.com/liberapay/liberapay.com/issues/1863)
 
 - Lack of information about privacy:
 There was another customer who sent an email regarding what information was saved when it came to payment process. More importantly, there were concerned about the information cookies was transporting, whether Liberapay sells the customer's data, and whether there are any third parties involved. [Issue](https://github.com/liberapay/liberapay.com/issues/1719)
 
For the complaints above, it is worth noting that Liberapay took on those issues and addressed them by adding more documentation about security and privacy concerns within the platform.

The overall documentation on installation and configuration within Liberapay suggests that most security issues stem from interaction with external payment platforms and recent compliance efforts to adhere to GDPR. As Liberapay is primarily based in France it is subject to EU regulations — including the General Data Protection Regulation (GDPR). For example — [GDPR](https://github.com/liberapay/liberapay.org/issues/35). 
 

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
 
