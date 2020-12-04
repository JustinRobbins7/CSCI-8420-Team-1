# Full Code Review Checklist

## User Input Validation / Remote Code Execution

### CWE-20: Improper Input Validation
  - “The product receives input or data, but it does not validate or incorrectly validates that the input has the properties that are required to process the data safely and correctly.”
  - [https://cwe.mitre.org/data/definitions/20.html](https://cwe.mitre.org/data/definitions/20.html)

### CWE-94: Improper Control of Code Generation (Code Injection)
  - “The software constructs all or part of a code segment using externally-influenced input from an upstream component, but it does not neutralize or incorrectly neutralizes special elements that could modify the syntax or behavior of the intended code segment.”
  - [https://cwe.mitre.org/data/definitions/94.html](https://cwe.mitre.org/data/definitions/94.html)


## Data Flow Sniffing / Interruption

### CWE-311: Missing Encryption of Sensitive Data
  - [https://cwe.mitre.org/data/definitions/311.html](https://cwe.mitre.org/data/definitions/311.html)
  - Parent of the following:

### CWE-312: Cleartext Storage of Sensitive Information
  - “The application stores sensitive information in cleartext within a resource that might be accessible to another control sphere.”
  - [https://cwe.mitre.org/data/definitions/312.html](https://cwe.mitre.org/data/definitions/312.html)

### CWE-319: Cleartext Transmission of Sensitive Information
  - “The software transmits sensitive or security-critical data in cleartext in a communication channel that can be sniffed by unauthorized actors.”
  - [https://cwe.mitre.org/data/definitions/319.html](https://cwe.mitre.org/data/definitions/319.html)

### CWE-614: Sensitive Cookie in HTTPS Session Without 'Secure' Attribute
  - “The Secure attribute for sensitive cookies in HTTPS sessions is not set, which could cause the user agent to send those cookies in plaintext over an HTTP session.”
  - [https://cwe.mitre.org/data/definitions/614.html](https://cwe.mitre.org/data/definitions/614.html)

## Cross Site Request Forgery (XSS)

### CWE-79 - Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')
  - “The software does not neutralize or incorrectly neutralizes user-controllable input before it is placed in output that is used as a web page that is served to other users.”
  - [https://cwe.mitre.org/data/definitions/79.html](https://cwe.mitre.org/data/definitions/79.html)

## Payment Host Spoofing

### CWE-290: Authentication Bypass by Spoofing
  - [https://cwe.mitre.org/data/definitions/290.html](https://cwe.mitre.org/data/definitions/290.html )
  - Parent of the following:

### CWE-291: Reliance on IP Address for Authentication
  - “The software uses an IP address for authentication.”
  - [https://cwe.mitre.org/data/definitions/291.html](https://cwe.mitre.org/data/definitions/291.html)

### CWE-350: Reliance on Reverse DNS Resolution for a Security-Critical Action
  - “The software performs reverse DNS resolution on an IP address to obtain the hostname and make a security decision, but it does not properly ensure that the IP address is truly associated with the hostname.”
  - [https://cwe.mitre.org/data/definitions/350.html](https://cwe.mitre.org/data/definitions/350.html)

### CWE-602: Client-Side Enforcement of Server-Side Security
  - “The software is composed of a server that relies on the client to implement a mechanism that is intended to protect the server.”
  - [https://cwe.mitre.org/data/definitions/602.html](https://cwe.mitre.org/data/definitions/602.html)

## Brute Force Weakness (Implement 2FA and login limits)

### CWE 307 - Improper Restriction of Excessive Authentication Attempts
  - “The software does not implement sufficient measures to prevent multiple failed authentication attempts within a short time frame, making it more susceptible to brute force attacks.”
  - [https://cwe.mitre.org/data/definitions/307.html](https://cwe.mitre.org/data/definitions/307.html)
  
### CWE 308 - Use of Single-factor Authentication
  - “The use of single-factor authentication can lead to unnecessary risk of compromise when compared with the benefits of a dual-factor authentication scheme.”
  - [https://cwe.mitre.org/data/definitions/308.html](https://cwe.mitre.org/data/definitions/308.html)
  
### CWE-770 - Allocation of Resources Without Limits or Throttling
  - “The software allocates a reusable resource or group of resources on behalf of an actor without imposing any restrictions on the size or number of resources that can be allocated, in violation of the intended security policy for that actor.”
  - [https://cwe.mitre.org/data/definitions/770.html](https://cwe.mitre.org/data/definitions/770.html)

