### Our protocol for Single Sign On (SSO) is SAML. 
- Mature protocol, very secure, operates over a web browser
- Utilizes XML files 
- Stateful, whereas OIDC is RESTful and stateless  
- [SAML2.0](https://docs.oasis-open.org/security/saml/v2.0/saml-profiles-2.0-os.pdf "SAMLv2.0 Docs") is the latest standard

### SSO Handshake requires two parties talking to one another
1. **Service Provider (SP)** = Spring Security SAML  
   - The service we are offering (loads of data on opioids via Spring Boot)  
2. **Identity Provider (IdP)** = Keycloak
   - Our means to authenticate a user for our Service Provider

### Methods of communication

### [Types of Bindings](https://docs.oasis-open.org/security/saml/v2.0/saml-bindings-2.0-os.pdf "SAML2.0 Docs") 
1. [HTTP **Artifact** Binding](https://everything1know.wordpress.com/2019/02/19/saml-2-0-artifact-binding/ "Explanation")  
   * SAML requests & responses are sent by "artifact" reference. 
   * Can be combined with Redirect and POST 
   * Its purpose is to provide an option in SAML 1.x SSO where SPs didn't have to validate XML Digital Signatures over each SAML Assertion
   * Allowed the signature to be optional, and instead allowed the deployer to leverage transport level security
2. HTTP **Redirect** Binding  
   * Allows for SAML messages to be embedded within URL parameters
3. HTTP **POST** Binding  
   * SAML message is transmitted within the base64-encoded content within an HTML form
   * SAML requestors and responders to communicate by using an HTTP user agent as an intermediary
   * The agent might be necessary if the communicating entities don't have direct path of communication
   * The intermediary might also be necessary if the responder requires interaction with a user agent such as an authentication agent
4. SAML **SOAP** Binding  
   * Turn SAML message into Simple Object Access Protocol (SOAP) message  
   * Performance optimization for SLO, as opposed to having an IdP redirect the browser to every SP to indicate a logout has occurred
   * Each SP is required to track local user session state in some backend database so that the user session can be cancelled without the browser being present
   * Common practice is to maintain user session state in a browser cookie (which obviously requires the browser to be present)
   * SOAP SLO has limited use
5. Reverse SOAP (**PAOS**) Binding  
   * Allows an HTTP requestor to act as a SOAP responder or process SOAP messages containing messages  
6. SAML **URI** Binding  
   * A Uniform Resource Identifier refers to a resource independent of the protocol being used.
   * This binding is the combination of an AssertionIDRequest message with an AssertionIDRef message into a single URI
   * Similar to SOAP, URI can be transported by multiple protocols
   
#### Additional Examples 
- [Bindings](http://blog.pistolstar.us/blog/2012/12/14/security-assertion-markup-language-saml-bindings-explained/ "6 types")
- [Redirect & POST](https://www.samltool.com/generic_sso_req.php "Authentication Request")

#### SAML Bindings Best Practices
- Avoid back channel bindings --> federation will run more smoothly 
- Disallow username/password login  
- Disallow password resets  
- Disallow email address change  
- Enforce session timeouts  
- Force sign-in  

## Links  
- [Spring Security SAML](https://projects.spring.io/spring-security-saml/)  
- [SAML](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language "Security Assertion Markup Language")  
- [SSO](https://en.wikipedia.org/wiki/Single_sign-on "Single Sign On")  
- [Keycloak](https://www.keycloak.org)  
- [OneLogin Overview of SAML](https://developers.onelogin.com/saml)  
- [OneLogin Best Practices](https://developers.onelogin.com/saml/best-practices-and-faqs)  
- [IBM SAML2.0 Bindings](https://www.ibm.com/support/knowledgecenter/SSPREK_9.0.3/com.ibm.isam.doc/config/concept/fed_SAML20_bindings.html "Redirect, POST, Artifact & SOAP")
