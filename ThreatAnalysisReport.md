# Team 1 Liberapay DFD Threat Analysis

Written by Team 1:
Joel Allou, Mohammad H. Al Huneidi, Miti Mareddy, Justin Robbins, and Nicholas Sabata

Link to Liberapay Github: [Liberapay Project](https://github.com/liberapay/liberapay.com)


## 1. Threat Modeling

## 2. Observations
Liberapay Threat Analysis Report: [https://justinrobbins7.github.io/CSCI-8420-Team-1/LiberapayThreatAnalysis.htm](https://justinrobbins7.github.io/CSCI-8420-Team-1/LiberapayThreatAnalysis.htm)


10. _Elevation by Changing the Execution Flow in Liberapay_
- Description: An attacker may pass data into Liberapay in order to change the flow of program execution within Liberapay to the attacker's choosing.
- Justification: Need to investigate how ubiquitious Liberapay's sanitization is.
- __Observation:__ This issue has been mitigated by a recent commit to the Liberapay system. Liberapay provides ubiquitous sanitization on user input based on the id of the element. More spicifically, this functionality is limited to sanitization in the front-end system by adding required elements for given inputs and enforcing input types on them. Since this interaction occurs between the users and the Liberapay sytstem, having sanitization on inputs that can be passed from a user mitigates the issue of concern. 

Added in commit [#8a99590 ](https://github.com/liberapay/liberapay.com/commit/8a995901798e939968aaa367ea346b56b3688488)


11. _Cross Site Request Forgery_
- Description: Cross-site request forgery (CSRF or XSRF) is a type of attack in which an attacker forces a user's browser to make a forged request to a vulnerable site by exploiting an existing trust relationship between the browser and the vulnerable web site.  In a simple scenario, a user is logged in to web site A using a cookie as a credential.  The other browses to web site B.  Web site B returns a page with a hidden form that posts to web site A.  Since the browser will carry the user's cookie to web site A, web site B now can take any action on web site A, for example, adding an admin to an account.  The attack can be used to exploit any requests that the browser automatically authenticates, e.g. by session cookie, integrated authentication, IP whitelisting. The attack can be carried out in many ways such as by luring the victim to a site under control of the attacker, getting the user to click a link in a phishing email, or hacking a reputable web site that the victim will visit. The issue can only be resolved on the server side by requiring that all authenticated state-changing requests include an additional piece of secret payload (canary or CSRF token) which is known only to the legitimate web site and the browser and which is protected in transit through SSL/TLS. See the Forgery Protection property on the flow stencil for a list of mitigations.
- Justification: Use of HTTPS protocols and encryption should mitigate this threat by protected session information in transit.
- __Observation:__ The Liberapay system addresses this security concern by protecting session information in transit. It has CSRF protection in place, as well as encryption on site data. The CSRF protection is done via a token-generation procedure which is added to each state. Additionally, the Liberapay system encrypts sensitive data in transit between the user and the system. This is done via a modified Fernet protocol. First messages in plain text are serialized using the Concise Binary Object Representation. After which, they're encyrpted with an AES cipher as done in the regular Fernet protocol. It should be noted however, that Liberapay does not explicitly enforce the use of the HTTPS protocol for requests/replies from a User. While the codebase seems to enforce the use of HTTPS for internal links and form submissions, a gap exists when interacting from an external e-mail and the system. In this case, the mitigation that exists involves a hit-rate limitation on requests that arrive without HTTPS, as well as replying with a security message 'http-unsafe'.

CSRF Protection can be found [here](https://github.com/liberapay/liberapay.com/blob/83a1d767c7723d8bc0fff9aa5bb8bb9b8e4e8205/liberapay/security/csrf.py)
Encryption is currently done via the Fernet protocol and can be found [here](https://github.com/liberapay/liberapay.com/blob/77f6df133ee434495e2f268d3682a711142513ea/liberapay/security/crypto.py)
HTTP rate limitation can be found [here](https://github.com/liberapay/liberapay.com/commit/f73b4f23d28aaf19b0e1e6c0b257397e40dfee77)

12. _Spoofing of the Payment Host External Destination Entity_
- Description: Payment Host may be spoofed by an attacker and this may lead to data being sent to the attacker's target instead of Payment Host. Consider using a standard authentication mechanism to identify the external entity.
- Justification: Both Liberapay and the payment hosts implement standard authentication methods.
- __Observation:__ Liberapay authenticates the two payment hosts it works with (Strip and PayPal) by verifying the connection each time communication is initiated. Upon the initialization of a session with one of the two payment hosts, Liberapay sends an encrypted authorization header with the given secret code. Connection is only fully established if the appropriate response is sent from the payment host. For this reason, the threat originating from the possibility of spoofing a payment host is addressed by the Liberapay system.

Authentication can be found [here](https://github.com/liberapay/liberapay.com/blob/9739f0366d962274fa10a9e371dbb8e2d2fba4c8/liberapay/payin/paypal.py)

13. _Not Applicable_

14. _Data Flow Credentials Is Potentially Interrupted_
- Description: An external agent interrupts data flowing across a trust boundary in either direction.
- Justification: HTTPs protocol and encryption should mitigate this threat.
- __Observation:__ The Liberapay system encrypts data which crosses the application boundary between itself and its payment hosts. This is done via a modified Fernet protocol where messages are first serialized using CBOR. Then, the serialized messages are encrypted via an AES cipher. Liberapay enforces the use of the HTTPS protocol for requests/replies across the application boundary between itself and the payment hosts. The codebase enforces the use of HTTPS for internal links and system generated communications, and it is assumed that payment hosts send via HTTPS as well. For messages crossing the application boundary, this process flow adequately addresses the threat of concern here as credentials being sent across the application boundary or replies from the payment host are sent via HTTPS. The only gap that exists occurs from the side of the payment host, but as they are an external interactor it is unclear whether they enforce sending messages via the HTTPS protocol (although it is strongly assumed).

Encryption is currently done via the Fernet protocol and can be found [here](https://github.com/liberapay/liberapay.com/blob/77f6df133ee434495e2f268d3682a711142513ea/liberapay/security/crypto.py)
HTTPS connection to payment hosts can be found [here](https://github.com/liberapay/liberapay.com/blob/2cb5fb4f9f0849a5d188773981b0fb89f4747627/www/payment-providers/%25provider/connect.spt)


15. _Data Flow Info Request Is Potentially Interrupted_
- Description: An external agent interrupts data flowing across a trust boundary in either direction.
- Justification: Should be mitigated through the use of HTTPS. 
- __Observation:__ Liberapay mitigates interruptions to data flowing across the Database boundary in either direction through the use of the HTTPS protocol. Messages originating from the Liberapay system are automatically enforced to send in the HTTPS protocol, so interruptions occuring across the boundary are secured by this protocol. Alternatively, interruptions from Amazon Web Services into the Liberapay system are assumed to use HTTPS (although this is unconfirmed as it originates from an external interactor). This presents a gap in the threat mitigation across this boundary. The Liberapay system addresses this through the use of rate-limitation techniques as well as request/reply messaging. This addresses the potential gap by refusing connections that originate from protocols other than HTTPS.

HTTP rate-limitation for queries can be found [here](https://github.com/liberapay/liberapay.com/commit/f73b4f23d28aaf19b0e1e6c0b257397e40dfee77)

16. _Not Applicable_

17. _Spoofing of the Amazon AWS Database External Destination Entity_
- Description: Amazon AWS Database may be spoofed by an attacker and this may lead to data being sent to the attacker's target instead of Amazon AWS Database. Consider using a standard authentication mechanism to identify the external entity.
- Justification: Liberapay uses standard authentication mechanisms and HTTPS.
- __Observation:__ The Liberapay system authorizes the initial database connection in a configuration file that specifies the hostname, port, and password. This is a persistent connection which initially authorizes the correct external interactor to interface with. Apart fromt the initialized connection, the authenticity of the database connection is checked through the use of a session manager which tokenizes each session and subsequent database connection. Apart from those authentication mechanisms, communications performed between the Liberapay system and Amazon Web Services are conducted via the HTTPS protocol.

The closed issue related to the Amazon Database and authentication can be found [here](https://github.com/liberapay/liberapay.com/issues/553)
Database environment configuration can be found [here](https://github.com/liberapay/liberapay.com/blob/00a0059ffe63dc6171a0ffcd0090a84c005763d8/.ebextensions/01_env.config)
Authentication code file can be found [here](https://github.com/liberapay/liberapay.com/blob/f57a9fa44ce79a5339049603728be5f6a8334980/liberapay/security/authentication.py).

18. _Spoofing of the User External Destination Entity_
- Description: User may be spoofed by an attacker and this may lead to data being sent to the attacker's target instead of User. Consider using a standard authentication mechanism to identify the external entity.
- Justification: Standard authentication methods and two factor authentication should reduce the threat of this happening.
- __Observation:__ Liberapay does not currently have a two-factor authentication process in place. This presents a gap in the security enhancements offered by the platform in regard to protecting user accounts from spoofing. Alternatively, there are some mitigations in place that don't utilize 2FA to prevent the spoofing of user accounts. These include session management, as well as email based authorization (which acts like 2 factor authentication). Session management only mitigates the threat of altered user accounts after the initial session is established. To authenticate a user however, Liberapay provides the ability for a user to login via email link instead of direct password. This still presents a gap in verifying the authenticity of a user, but allows for a slight added layer of protection.

Authentication code file can be found [here](https://github.com/liberapay/liberapay.com/blob/f57a9fa44ce79a5339049603728be5f6a8334980/liberapay/security/authentication.py).
Potential incorporation of 2FA [here](https://github.com/liberapay/liberapay.com/issues/926)

## 3. GitHub information
Here are links to our Github Pages: \
Master Branch: [Home Page](https://github.com/JustinRobbins7/CSCI-8420-Team-1) \
Issues Page: [Issues Page](https://github.com/JustinRobbins7/CSCI-8420-Team-1/issues) \
Project Board: [Project Board](https://github.com/JustinRobbins7/CSCI-8420-Team-1/projects/2)

Contribution Records: \
Github Pulse: [Pulse Page](https://github.com/JustinRobbins7/CSCI-8420-Team-1/pulse) \
Github Contributors: [Contributors Page](https://github.com/JustinRobbins7/CSCI-8420-Team-1/graphs/contributors) 

## 4. Teamwork Reflection
For this project deliverable, team members continued to meet regularly after a brief hiatus for Fall Break. Each team member was initially tasked with familiarizing themselves with DFD diagrams as well as the Microsoft TMT. After meting with Dr. Ghandi, we gained clarity on requirements for the deliverable and were able to make significant progress towards the document. We met prior to the deadline an additional time to regroup and coordinate with each other on tasks left to accomplish. All team members were then provisioned a section of the threat analysis report to investigate on the OSS platform. With tasks delegated, the assignment progressed smoothly with no conflicts or major impediments.

No major issues occurred during the build and finalization of this deliverable. There was additional clarification needed prior to delivery, but was mitigated via e-mail correspondence. For upcoming tasks we plan to continue our weekly team meetings and keep everyone up-to-date on both Github and other communication platforms.
