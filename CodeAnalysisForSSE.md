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

__2. CWE-79: Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')
  
  Cross-site tracking or XSS is one of the main areas of concern outlined in previous deliverables. Manual code review of the main Liberapay process suggests that there is no prevention or validation of xml code within Liberapay. This leads to a security deficit in regard to cross-site scripting attacks as a malicious party can modify xml data or introduce altered xml data to the main process.
  
 In the main Liberapay \_base.py file, under the Platform class, a specific example is listed where only file formats are checked before parsing.
  ```
  # Determine the appropriate response parser using `self.api_format`
        api_format = getattr(self, 'api_format', None)
        if api_format == 'json':
            self.api_parser = lambda r: r.json()
        elif api_format == 'xml':
            self.api_parser = lambda r: ET.fromstring(r.content)
        elif api_format:
            raise ValueError('unknown API format: '+str(api_format))
  ```
  
  Additionally, an example of the xml data used by the api processes is shown below. The following code snippet highlights a deficit in verification and reliability of xml data within Liberapay. It should be noted that the following code was sourced from the testing directory and has no bearing on actual runtime processes. However, it does bring to light a type of vulnerability and gap within the Liberapay codebase that pertains to CWE-79.
  
  ```
openstreetmap = lambda: ET.fromstring("""
 <!-- copied from http://wiki.openstreetmap.org/wiki/API_v0.6 -->
 <osm version="0.6" generator="OpenStreetMap server">
   <user id="12023" display_name="jbpbis" account_created="2007-08-16T01:35:56Z">
     <description></description>
     <contributor-terms agreed="false"/>
     <img href="http://www.gravatar.com/avatar/c8c86cd15f60ecca66ce2b10cb6b9a00.jpg?s=256&amp;d=http%3A%2F%2Fwww.openstreetmap.org%2Fassets%2Fusers%2Fimages%2Flarge-39c3a9dc4e778311af6b70ddcf447b58.png"/>
     <roles>
     </roles>
     <changesets count="1"/>
     <traces count="0"/>
     <blocks>
       <received count="0" active="0"/>
     </blocks>
   </user>
 </osm>
""")
```

A further vulnerability relating to XSS exists in the user authentication codebase. When the system checks for an appropriate cookie, the b64 decoder is susceptible to a self XSS attack. This is because the value assigned to the _then_ parameter holds the information in the upstream POST request. This was also highlighted in [Issue #1134](https://github.com/liberapay/liberapay.com/issues/1134).

```
cookie_obj = json.loads(b64decode_s(cookie_value))
query_data, action, then, action_user_id = cookie_obj[:4]
p_id = cookie_obj[4] if len(cookie_obj) > 4 else None
then = b64decode_s(then)  # double-b64encoded to avoid other encoding issues w/ qs
```


__4. CWE-291: Reliance on IP Address for Authentication
  
  An issue with the reliance of using the IP Address to authenticate users was found in the authentication.py script under the security directory listed [here](https://github.com/liberapay/liberapay.com/blob/748f4aa8be32f75ca9eb112be18d32ac6b92aef5/liberapay/security/authentication.py). Specifically, if a password and userid are entered multiple times from the same ip address, Liberapay refuses to admit a user. While this is designed to prevent multiple login attempts from a single IP address, there is no further safeguard to prevent an attacker from using multiple ip addresses to attempt to gain access to an account. 
  
  ```
  if password and id_type:
            website.db.hit_rate_limit('log-in.password.ip-addr', str(src_addr), TooManyLogInAttempts)
            website.db.hit_rate_limit('hash_password.ip-addr', str(src_addr), TooManyRequests)
  ```
  
  It should also be noted that obtaining the IP address for authentication purposes also poses a risk. THis can be found in the main.py script on the liberapay directory. Here the ip address used for authentication purposes is obtained from an attribute of the main process. The attribute self.environ is initialized from information obtained from a user's browser, and is not further verified.
  
  ```addr = self.environ.get('REMOTE_ADDR') or self.environ[b'REMOTE_ADDR']
        addr = ip_address(addr.decode('ascii') if type(addr) is bytes else addr)
        trusted_proxies = getattr(self.website, 'trusted_proxies', None)
        forwarded_for = self.headers.get(b'X-Forwarded-For')
        self.__dict__['bypasses_proxy'] = bool(trusted_proxies)
        if not trusted_proxies or not forwarded_for:
            return addr
  ```
  
  Both of those code snippets contribute to a security deficit in authenticating users on login via their ip address.

__5. CWE-307: Improper Restriction of Excessive Authentication Attempts
  
  Found in the constants.py script, restrictions of excessive authentication attempts are controlled by predefined limits within Liberapay. The enumerator RATE_LIMITS highlights some of these restrictions. Here we see that Liberapay does in fact have appropriate, albeit hardcoded limits for multiple authentication attempts. Notably, the ip address and net are two of the most prolific authentication controls within Liberapay. Their rate limits defined here are adequate for the platform since they are combined with further preventions provided by Cloudflare. The contributors of Liberapay have also identified deficiencies with the two tier system for excessive authentication attempts and currently attempting to resolve this security deficit. Further discussion of the proposed resolution can be found [here](https://github.com/liberapay/liberapay.com/issues/1727).
  
  ```
  RATE_LIMITS = {
    'add_email.source': (5, 60*60*24),  # 5 per day
    'add_email.target': (2, 60*60*24),  # 2 per day
    'log-in.email': (10, 60*60*24),  # 10 per day
    'log-in.email.not-verified': (2, 60*60*24),  # 2 per day
    'log-in.email.verified': (10, 60*60*24),  # 10 per day
    'log-in.password': (3, 60*60),  # 3 per hour
    'sign-up.ip-addr': (5, 60*60),  # 5 per hour per IP address
    'sign-up.ip-net': (15, 15*60),  # 15 per 15 minutes per IP network
    'sign-up.ip-version': (15, 15*60),  # 15 per 15 minutes per IP version
}
```

__7. CWE-312: Cleartext Storage of Sensitive Information:__

An analysis has been conducted on the parts of the Liberapay code that deal with database storage to determine whether they're storing any sensitive information without encrypting it. The result of the manual code review concluded that Liberapay utilizes: __pbkdf2_hmac__ encryption which is a type of encryption available from the hashlib python library. Sensitive data gets encrypted with it before it's inserted into the database. We've researched this encryption algorithm to determine it's encryption capabilities and we found the following description of it in the [python hashlib documentation](https://docs.python.org/3/library/hashlib.html) "The function provides PKCS#5 password-based key derivation function 2. It uses HMAC as pseudorandom function." Hence we've concluded that CWE-312 is addressed adequately. 

PBKDF2_HMAC utilization:
```
    @classmethod
    def hash_password(cls, password):
        # Using SHA-256 as the HMAC algorithm (PBKDF2 + HMAC-SHA-256)
        algo = 'sha256'
        # Generate 32 random bytes for the salt
        salt = urandom(32)
        rounds = website.app_conf.password_rounds
        hashed = cls._hash_password(password, algo, salt, rounds)
        hashed = '$'.join((
            algo,
            str(rounds),
            b64encode(salt).decode('ascii'),
            b64encode(hashed).decode('ascii')
        ))
        return hashed

    @staticmethod
    def _hash_password(password, algo, salt, rounds):
        return pbkdf2_hmac(algo, password.encode('utf8'), salt, rounds)
```

__8. CWE-319: Cleartext Transmission of Sensitive information:__

Liberapay communicates with two external systems with sensitive information. Those two external systems are Paypal and Stripe. A code review has been conducted on the code that communicates with Paypal and Stripe to determine the mechanism of communciation and whether it poses any security concerns. Liberapay makes requests to RESTful APIs exposed by Paypal and Stripe. The API's are of type "POST, and Liberapay passes in the request body data pertaining to a particular donation order. The request body for both of the API calls (Paypal and Stripe) abides by their respective specs. The notable information being passed include the donator's user name, transaction amount, and the team/user name the donated is being sent to. This information does not include any monetary concerns or sensitive information per say such as a password or a credit card number. The actual transaction is conducted on Paypal/Stripe and Liberapay simply acts as a delegator. However one could argue that the donation that is happening could be a piece of information that a user wishes to keep private hence it's important that the request body is encrypted in some fashion to protect it from potential attackers. Going back to the fact that the communication is conducted via RESTful APIs. Those APIs use HTTP and support Transport Layer Security (TLS) encryption. TLS is a standard that keeps an internet connection private and checks that the data sent between two systems (a server and a server, or a server and a client) is encrypted and unmodified. Since Liberapay is using restful services to communicate with Paypal and stripe. We deem that the communication is safe and Liberapay has sufficient protections in place.


__9. CWE-350: Reliance on Reverse DNS Resolution for a Security-Critical Action:__

A code review has been conducted to determine whether Liberapay makes use of reverse DNS lookups. But it does not appear that Liberapay does any reverse look ups. Hence we've determined that CWE-350 is not of concern as we've thought previously.

__11. Sensitive Cookie in HTTPS Session Without 'Secure' Attribute__

Sensitive Cookie in HTTPS without the 'secure' attribute could result in sending information about the cookie via plain text. If such is a case and the cookie contains critical data, this could jeopardize Liberapay and its users. Liberpay uses cookies which are required to authenticate the user or to perform a specific operation. These cookies are restricted to same-site requests. During the manual coding review, the code was thoroughly analyzed to try and find spots where the setting of cookies was not done securely. Howevever, no such place was found. Liberapay does indeed secure sensitive cookies in HTTPS sessions. In fact, everytime a cookie is created/set, as long as the session was HTTPS protocol (which Liberapay uses), the particurlar cookie is made secure. As one can see in the code snippet below, everytime a cookie is set, Liberapay sets a lot of options associated with the cookie, such as the path, samesite, httponly, security of the cookie, and more. Therefore, Liberapay eradicates the potential data interception by a bad actor via cookies. Below, we highlight the snippet of code that showcases the findings:

```
  def set_cookie(cookies, key, value, expires=None, httponly=True, path='/', samesite='lax'):
    key = ensure_str(key)
    cookies[key] = ensure_str(value)
    cookie = cookies[key]
    if expires:
        if isinstance(expires, timedelta):
            expires += utcnow()
        if isinstance(expires, datetime):
            expires = to_rfc822(expires)
        cookie['expires'] = ensure_str(expires)
    if httponly:
        cookie['httponly'] = True
    if path:
        cookie['path'] = ensure_str(path)
    if samesite:
        cookie['samesite'] = ensure_str(samesite)
    if website.cookie_domain:
        cookie['domain'] = ensure_str(website.cookie_domain)
    if website.canonical_scheme == 'https':
        cookie['secure'] = True
```

### 1.3. Findings from Automatic Tools
Our team utilized two distinct tools to try and locate common security issues/vulnerabilities in LiberaPay.

**SonarQube**:

We've setup a SonarQube server along with a Python plugin. We used a Python  plugin because the Liberpay code base is comprised predominantely of Python. And consequently all of the logic that deals with security related concerns is written in Python. We also installed a SonarQube Python-Based scanner to conduct a scan on the Liberapay code base. The scanner generated a report that we accessed through the SonarQube server. Over 16000 lines of code were scanned by the Python scanner. The findings of the scanner did not indicate any vulnerabilities in the code. Attached below is a screenshot of the result. However it did detect three bugs in the codebase, we've decided to manually validate the bugs to ensure that they did not pose any security concerns. But we deemed that those bugs are just related to code maintainability and do not pose any security threats.

![SonarQube Scan Results](/Images/sonarqube-scan-result.png)


**Bandit**

For automated code scanning, we used [Bandit](https://pypi.org/project/bandit/) over the project files related to the areas of interest revealed in previous deliverables. Initially, the automated code scan resulted in over one thousand issues of low priority. However, once we filtered based on issue severity we found that a lot of the issues brought up were code style or convention related problems. Filtering for issues of medium to high security risk revealed ten major issues in the areas of interest. From these issues two stood out as new findings which we didn't cover in the manual review process.

The findings of the automated code scan performed by Bandit are linked below:

- [Bandit Scan](/Images/Automated_Scan_Bandit.pdf)

 - CWE-916: Use of Password Hash with Insufficient Computational Effort

Apart from issues covered during the manual code review, a new recommendation by Bandit which correspond to CWE-916. Bandit recommended in its _Issue B303_ that there was use of an insecure hash function in both the \_base.py file as well as the extract.py files. These are both located in the main Liberapay project repository and involve message encryptions. 

The \_base.py issue is as follows : 
```
264 bs = r.email.strip().lower().encode('utf8')
265 gravatar_id = hashlib.md5(bs).hexdigest()
266 if gravatar_id:
```
The extract.py issue is as follows :
```
12 if isinstance(msg, tuple) and msg[0] == '':
13 unused = "<unused singular (hash=%s)>" % md5(msg[1].encode('utf8')).hexdigest()
14 msg = (unused, msg[1], msg[2])
```

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
