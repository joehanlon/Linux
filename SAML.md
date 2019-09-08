
[Spring Security SAML + Keycloak](https://qiita.com/daian183/items/e4388e1be6634b2dc789)  
https://hotchpotchj37.wordpress.com/2018/07/23/spring-security-saml%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%A6saml%E8%AA%8D%E8%A8%BC%E3%81%AB%E3%83%81%E3%83%A3%E3%83%AC%E3%83%B3%E3%82%B8%E3%81%97%E3%81%9F%E3%83%A1%E3%83%A2/  
http://www.java2s.com/  
https://gitee.com/caiman/spring-boot-security-saml-sample  
https://devhub.io/repos/ulisesbocchio-spring-boot-security-saml  
https://www.owasp.org/index.php/SAML_Security_Cheat_Sheet  



[SS + Keycloak + OIDC](https://www.javaland.eu/formes/pubfiles/11145218/2019-nn-thomas_darimont-sichere_spring-anwendungen_mit_keycloak-praesentation.pdf)

[Secure Spring Boot Microservices w/ Keycloak Pt1](https://www.youtube.com/watch?v=Bdg_DjuoX0A "DevNation Live")

[Common Issues w/ SAML Authentication](https://help.blackboard.com/Learn/Administrator/SaaS/Authentication/Implement_Authentication/SAML_Authentication_Provider_Type/Common_Issues_with_SAML_Authentication)

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

### Vocabulary 
- [Assertions](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language#Assertions "Wiki") 
   * http://saml.xml.org/assertions
   * _An assertion is a package of information that supplies one or more statements made by a SAML authority. SAML defines three different kinds of assertion statement that can be created by a SAML authority_  
         1. **Authentication** : The specified subject was authenticated by a particular means at a particular time by an Identity Provider, which is in charge of keeping track of users and their info   
         2. **Attribute** : The supplied subject is associated with the supplied attributes  
         3. **Authorization decision** : A request to allow specified subject to access the specified resource has been granted or denied  
- [Protocols](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language#Protocols "Wiki")
   * http://saml.xml.org/protocols
   * _There are a number of request/response protocols. The protocol is encoded in an XML schema as a set of request-response pairs. They allow the user to request / query for an assertion, ask for a subject to be authenticated, create and manage name identifier mappings, and request near-simultaneous logout of a collection of related sessions_
         1. **Assertion Query and Request Protocol**  
         2. **Authentication Request Protocol**  
         3. **Artifact Protocol**  
         4. **Name Identifier Management Protocol**  
         5. **Single Logout Protocol**  
         6. **Name Identifier Mapping Protocol**  

- [Bindings](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language#Bindings "Wiki")
   * http://saml.xml.org/bindings  
   * _Mappings from SAML request request-response message exchanges into standard messaging or communication protocols. Two main types include : _  
         1. Redirect  
         2. POST  
- [Profiles](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language#Profiles "Wiki")
   * _How SAML assertions, protocols, and bindings combine to support a defined use case_   
         1. Web Browser SSO  
         2. Single Logout  
         3. Name Identifier Management  
         4. Etc..  
- [Security](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language#Security "Wiki") 
   * _Requirements are often phrased in terms of (mutual) authentication, integrity, and confidentiality, leaving the choice of security mechanism to implementers and deployers. Examples include :_  
         1. TLS  
         2. XML Signature & XML Encryption for message-level security  

### Methods of communication

### [Types of Bindings](https://docs.oasis-open.org/security/saml/v2.0/saml-bindings-2.0-os.pdf "SAML2.0 Docs") 
1. [HTTP **Artifact** Binding](https://everything1know.wordpress.com/2019/02/19/saml-2-0-artifact-binding/ "Explanation")  
   * SAML requests & responses are sent by "artifact" reference. 
   * Can be combined with Redirect and POST 
   * Its purpose is to provide an option in SAML 1.x SSO where SPs didn't have to validate XML Digital Signatures over each SAML Assertion
   * Allowed the signature to be optional, and instead allowed the deployer to leverage transport level security
2. [HTTP **Redirect** Binding](https://en.wikipedia.org/wiki/SAML_2.0#HTTP_Redirect_Binding "Wiki")  
   * Allows for SAML messages to be embedded within URL parameters
   * Better for short messages
3. [HTTP **POST** Binding](https://en.wikipedia.org/wiki/SAML_2.0#HTTP_POST_Binding "Wiki")
   * SAML message is transmitted within the base64-encoded content within an HTML form
   * SAML requestors and responders to communicate by using an HTTP user agent as an intermediary
   * The agent might be necessary if the communicating entities don't have direct path of communication
   * The intermediary might also be necessary if the responder requires interaction with a user agent such as an authentication agent
4. [SAML **SOAP** Binding](https://kb.novaordis.com/index.php/SAML_SOAP_Binding "Wiki")  
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

In Web Browser SSO profile, we have two main categories : 
   1. SP-Initiated SSO 
      * Our Server + Hello World app
         * Spring Boot 
         * Maven 
         * Tomcat
   2. IdP-Initiated SSO 
      * Our Identity Provider
         * Keycloak --> PingFederate
         * Wildfly Server --> ???
         
### Example 

### Okta Spring Security SAML Overview
[Okta Spring Security SAML Overview](https://developer.okta.com/code/java/spring_security_saml/)
Dependencies : 
   1. Java
   2. Maven or Gradle
   3. Tomcat or some other Java web server

#### Download the Spring SAML Extension
```console
git clone https://github.com/spring-projects/spring-security-saml.git
```
#### Checkout Okta's Mac OS Sample App
```console
git clone https://github.com/nshobayo/okta-SpringSAML.git

# Copy the sample Okta app into the Extension's "src" folder
rm -rf spring-security-saml/sample/src/main
cp -r okta-SpringSAML/src/main spring-security-saml/sample/src
```
#### Compilation
```
cd spring-security-saml/sample
../gradlew build install
```
#### Etc etc etc

### Spring Security SAML Documentation
[Documentation](https://docs.spring.io/spring-security-saml/docs/ "All Spring Security SAML Docs")  
[Reference](https://docs.spring.io/spring-security-saml/docs/current/reference/html/ "Spring Security SAML Extension Ref Docs")  
[API](https://docs.spring.io/spring-security-saml/docs/current/api/ "Current Spring Security SAML API")  
[GitHub](https://github.com/spring-projects/spring-security-saml)


## Spring Boot CLI Tutorial
[Spring Boot CLI Tutorial](https://www.tutorialspoint.com/springbootcli/index.htm "tutorialspoint.com")


## Integrating Keycloak into Spring Boot App
Need to declare a Spring Bean for Keycloak as the IdP.  
Most likely needs to be added to the **WebSecurityConfig** class.  
Need to ensure that the Keycloak IdP definition is automatically picked up by the Spring Security SAML infrastructure.  
This is done by modifying the **metadata** bean to let Spring autowire all available **MedadataProvider** instances via the **providers** parameter.  

Create a **realm** with a name, e.g. "demo".  
Might need to adapt the URL to your Keycloak server in the **application.properties**  config file of the Spring Boot app or just pass a JVM System Property like:
```
-Dkeycloak.auth-server-url=http://localhost:8081/auth/realms/demo
```

In this **demo** realm, we need to create a client configuration for our Spring Boot app. 
#### Set-up Keycloak Client
   1. Create a new **SAML** client in Keycloak with the client-id **com:vdenotaris:spring:sp**. 
   2. In the client **settings** tab, configure the following :
      * Include AuthnStatement On
      * Include OneTimeUse Condition Off
      * Sign Documents On
      * Optimize REDIRECT signing key lookup Off
      * Sign Assertions On
      * Signature Algorithm RSA_SHA256
      * SAML Signature Key Name KEY_ID
      * Canonicalization Method EXCLUSIVE
      * Encrypt Assertions Off
      * Client Signature Required On
      * Force POST Binding On
      * Front Channel Logout On
      * Force Name ID Format Off
      * Name ID Format username
      * Root URL http://localhost:8080
      * Valid Redirect URIs /saml/*
      * Base URL /
      * Open Fine Grain SAML Endpoint Configuration
      * Assertion Consumer Service POST Binding URL /saml/SSO
      * Logout Service POST Binding URL /saml/logout
      
      
      
## Links  
- [Spring Security SAML](https://projects.spring.io/spring-security-saml/)  
- [SAML](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language "Security Assertion Markup Language")  
- [SSO](https://en.wikipedia.org/wiki/Single_sign-on "Single Sign On")  
- [Keycloak](https://www.keycloak.org)  
- [OneLogin Overview of SAML](https://developers.onelogin.com/saml)  
- [OneLogin Best Practices](https://developers.onelogin.com/saml/best-practices-and-faqs)  
- [SAML2.0 Profiles Explained](https://help.scorpionsoft.com/hc/en-us/articles/218317597-SAML-2-0-Profiles-explained-Building-your-own-SAML-integrations)
- [IBM SAML2.0 Profiles](https://www.ibm.com/support/knowledgecenter/SSPREK_9.0.3/com.ibm.isam.doc/config/concept/fed_SAML20_profiles.html)
- [IBM SAML2.0 Bindings](https://www.ibm.com/support/knowledgecenter/SSPREK_9.0.3/com.ibm.isam.doc/config/concept/fed_SAML20_bindings.html "Redirect, POST, Artifact & SOAP")
- [How SAML Works](https://gravitational.com/blog/how-saml-authentication-works/)
- [1. SAML Use Cases](https://medium.com/@sagarag/reloading-saml-do-you-really-need-saml-931976b3b5e3)
- [2. SAML Basics](https://medium.com/@sagarag/reloading-saml-saml-basics-b8999995c73e)
- [3. SAML Metadata](https://medium.com/@sagarag/reloading-saml-why-do-you-need-metadata-3fbeb43320c3)
- [4. SAML IdP](https://medium.com/@sagarag/reloading-saml-idp-discovery-693b6bff45f0)
- [5. SAML Web Browser SSO](https://medium.com/@sagarag/reloading-saml-web-browser-sso-profile-1b1775539101)
- [CAS Protocol](https://apereo.github.io/cas/4.2.x/protocol/CAS-Protocol.html)
- [Intro to SAML2.0](https://wso2.com/library/articles/2014/02/introduction-to-security-assertion-markup-language-2.0/)
- [Spring Security SAML & Keycloak](https://blog.codecentric.de/en/2019/03/secure-spring-boot-app-saml-keycloak/)
- [Spring Security SAML Example Project](https://github.com/vdenotaris/spring-boot-security-saml-sample/blob/master/pom.xml "Vincenzo De Notaris")
- [SSOCirle IdP](https://www.ssocircle.com/en/portfolio/publicidp/?source=post_page-----7ffe0dd38465----------------------)
- https://www.future-processing.pl/blog/exploring-spring-boot-and-spring-security-custom-token-based-authentication-of-rest-services-with-spring-security-and-pinch-of-spring-java-configuration-and-spring-integration-testing/  
- 
