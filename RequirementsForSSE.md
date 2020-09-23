# Team 1 Requirements for Software Security Engineering: Liberapay

Written by Team 1:
Joel Allou, Mohammad H. Al Huneidi, Miti Mareddy, Justin Robbins, and Nicholas Sabata

Link to Liberapay Github: [Liberapay Project](https://github.com/liberapay/liberapay.com)

## 1. Use/Misuse Case Analysis

### 1.1. *Use Cases*
 - Identify five essential interactions of your open-source software (system-of-interest) with its environment of operation. You will most likely identify these interactions based on the enabling systems or other systems in your systems engineering view from the previous assignment. Ideally, the interactions need to be spread across different external interactors (humans or systems) of your software. Once different external interactors have been identified, the interactions can focus on the most sensitive features used by the external interactor. 
 - Develop use-cases diagrams related to the five interactions. The use-cases should be about features supported by your software (system of interest) 
 
 ### 1.2. *Misuse Cases*
 - The misusers should be contextualized in your environment of operation and relevant to the interactions identified above. Use names for mis-users that help the reader understand their motives, resources, attack of choice, and the available access to the system to carry out their attack.
 - Iterate between use and misuse cases to elaborate additional security functions. Embed the final diagram in your report.

### 1.3. *Reflection*
 - Assess alignment of security requirements derived from mis-use case analysis with advertised features of the open-source software. Review OSS project documentation and codebase to support your observations. Summarize your observations.
 
 
### Customer - Login (1/5)

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


## 2. Liberapay Documentation Review
 - Review OSS project documentation for security-related configuration and installation issues. Summarize your observations.

## 3. Planning and Reflection
 - Include a link to your team GitHub repository that shows your internal project task assignments and collaborations to finish this task. 
 - Include a reflection of your teamwork for this assignment. What issues occurred? How did you resolve them? What did you plan to change moving forward? 
