[Spring](https://spring.io)  
[Spring Security](https://spring.io/projects/spring-security)  
[Spring Security SAML](https://projects.spring.io/spring-security-saml/)  
[Spring Security SAML Reference](https://docs.spring.io/spring-security-saml/docs/current/reference/htmlsingle/)  
[Spring Security SAML API](https://docs.spring.io/spring-security-saml/docs/current/api/)  
[Spring Security SAML GitHub](https://github.com/spring-projects/spring-security-saml)  
[Spring Security Extension RELEASES : Spring Security SAML](https://repo.spring.io/list/release/org/springframework/security/extensions/spring-security-saml/)


## [Spring Initializr](https://start.spring.io/)

Defaults (as of 9/2019):

- Project : Maven
- Language : Java
- Spring Boot : 2.1.8
- Project Metadata :
  - Group : com.example  -->  group
  - Artifact : demo  -->  artifact
  - Name : demo  -->  
  - Package Name : group.artifact
  - Packaging : Jar
  - Java : 8  --> 11
- Web
  - Spring Web

## First Project
```
sbss/
|_ .mvn/
| |_ wrapper/
|   |_ maven-wrapper.jar
|   |_ maven-wrapper.properties
|   |_ MavenWrapperDownloader.java
|
|_ .settings/
| |_ org.eclipse.core.resources.prefs
| |_ org.eclipse.jdt.apt.core.prefs
| |_ org.eclipse.jdt.core.prefs
| |_ org.eclipse.m2e.core.prefs
|
|_ src/
| |_ main/
| | |_ java/
| | | |_ group/
| | |   |_ artifact/
| | |     |_ config/
| | |     | |_ MvcConfig.java
| | |     | | |_ MvcConfig implements WebMvcConfigurer
| | |     | |   |_ currentUserHandlerMethodArgumentResolver
| | |     | |   |_ registry.addViewController("/").setViewName("pages/index");
| | |     | |   |_ registry.addResourceHandler("/static/**").addResourceLocations("/static/");
| | |     | |   |_ argumentResolvers.add(currentUserHandlerMethodArgumentResolver);
| | |     | |
| | |     | |_ WebSecurityConfig.java
| | |     |   |_ WebSecurityConfig extends WebSecurityConfigurerAdapter implements InitializingBean, DisposableBean
| | |     |     |_ init()
| | |     |     |_ shutdown()
| | |     |     |_ samlUserDetailsServiceImpl
| | |     |     |_ velocityEngine()
| | |     |     |_ StaticBasicParserPool parserPool()
| | |     |     |_ ParserPoolHolder parserPoolHolder()
| | |     |     |_ HttpClient httpClient()
| | |     |     |_ SAMLAuthenticationProvider samlAuthenticationProvider()
| | |     |     |_ SAMLContextProviderImpl contextProvider()
| | |     |     |_ SAMLBootstrap sAMLBootstrap()
| | |     |     |_ SAMLDefaultLogger samlLogger()
| | |     |     |_ WebSSOProfileConsumer webSSOprofileConsumer()
| | |     |     |_ WebSSOProfileConsumerHoKImpl hokWebSSOprofileConsumer()
| | |     |     |_ WebSSOProfile webSSOprofile()
| | |     |     |_ WebSSOProfileConsumerHoKImpl hokWebSSOProfile()
| | |     |     |_ WebSSOProfileECPImpl ecpprofile()
| | |     |     |_ SingleLogoutProfile logoutprofile()
| | |     |     |_ KeyManager keyManager()
| | |     |     | |_ storePass = "nalle123"
| | |     |     | |_ defaultKey = "apollo"
| | |     |     | |_ storeFile = loader.getResource("classpath:/saml/samlKeystore.jks")
| | |     |     |_ TLSProtocolConfigurer tlsProtocolConfigurer()
| | |     |     |_ WebSSOProfileOptions defaultWebSSOProfileOptions()
| | |     |     |_ SAMLEntryPoint samlEntryPoint()
| | |     |     |_ ExtendedMetadata extendedMetadata()
| | |     |     |_ SAMLDiscovery samlIDPDiscovery()
| | |     |     |_ ExtendedMetadataDelegate ssoCircleExtendedMetadataProvider() throws MetadataProviderException
| | |     |     | |_ @Bean
| | |     |     | |_ @Qualifier("idp-ssocircle")
| | |     |     | |_ idpSSOCircleMetadataURL = "https://idp.ssocircle.com/idp-meta.xml"
| | |     |     |_ CachingMetadataManager metadata() throws MetadataProviderException
| | |     |     | |_ @Bean
| | |     |     | |_ @Qualifier("metadata")
| | |     |     |_ MetadataGenerator metadataGenerator()
| | |     |     | |_ metadataGenerator.setEntityId("com:joe:spring:sp")
| | |     |     |_ MetadataDisplayFilter metadataDisplayFilter()
| | |     |     |_ SavedRequestAwareAuthenticationSuccessHandler successRedirectHandler()
| | |     |     | |_ successRedirectHandler.setDefaultTargetUrl("/landing")
| | |     |     |_ SimpleUrlAuthenticationFailureHandler authenticationFailureHandler()
| | |     |     | |_ failureHandler.setDefaultFailureUrl("/error")
| | |     |     |_ SAMLWebSSOHoKProcessingFilter samlWebSSOHoKProcessingFilter() throws Exception
| | |     |     |_ SAMLProcessingFilter samlWebSSOProcessingFilter() throws Exception
| | |     |     |_ MetadataGeneratorFilter metadataGeneratorFilter()
| | |     |     |_ SimpleUrlLogoutSuccessHandler successLogoutHandler()
| | |     |     | |_ successLogoutHandler.setDefaultTargetUrl("/")
| | |     |     |_ SecurityContextLogoutHandler logoutHandler()
| | |     |     |_ SAMLLogoutProcessingFilter samlLogoutProcessingFilter()
| | |     |     |_ SAMLLogoutFilter samlLogoutFilter()
| | |     |     |_ ArtifactResolutionProfile artifactResolutionProfile()
| | |     |     |_ HTTPArtifactBinding artifactBinding(
| | |     |     |_ HTTPSOAP11Binding soapBinding()
| | |     |     |_ HTTPPostBinding httpPostBinding()
| | |     |     |_ HTTPRedirectDeflateBinding httpRedirectDeflateBinding()
| | |     |     |_ HTTPSOAP11Binding httpSOAP11Binding()
| | |     |     |_ HTTPPAOS11Binding httpPAOS11Binding()
| | |     |     |_ SAMLProcessorImpl processor()
| | |     |     |_ FilterChainProxy samlFilter() throws Exception
| | |     |     | |_ AntPathRequestMatcher("/saml/login/**")
| | |     |     | |_ AntPathRequestMatcher("/saml/logout/**")
| | |     |     | |_ AntPathRequestMatcher("/saml/metadata/**")
| | |     |     | |_ AntPathRequestMatcher("/saml/SSO/**")
| | |     |     | |_ AntPathRequestMatcher("/saml/SSOHoK/**")
| | |     |     | |_ AntPathRequestMatcher("/saml/SingleLogout/**")
| | |     |     | |_ AntPathRequestMatcher("/saml/discovery/**")
| | |     |     |_ AuthenticationManager authenticationManagerBean() throws Exception
| | |     |     |_ protected void configure(HttpSecurity http) throws Exception
| | |     |     | |_ @param http
| | |     |     | |_ @throws Exception
| | |     |     | |_ @Override
| | |     |     | |_ http.authorizeRequests().antMatchers("/").permitAll()
| | |     |     |   |_ .antMatchers("/saml/**").permitAll()
| | |     |     |   |_ .antMatchers("/css/**").permitAll()
| | |     |     |   |_ .antMatchers("/img/**").permitAll()
| | |     |     |   |_ .antMatchers("/js/**").permitAll()
| | |     |     |   |_ .anyRequest().authenticated();
| | |     |     |_ protected void configure(AuthenticationManagerBuilder auth) throws Exception
| | |     |     | |_ @param auth
| | |     |     | |_ @throws Exception
| | |     |     | |_ @Override
| | |     |     |_ public void afterPropertiesSet() throws Exception
| | |     |     | |_ init()
| | |     |     |_ public void destroy() throws Exception
| | |     |       |_ shutdown()
| | |     | 
| | |     |_ controllers/
| | |     | |_ LandingController.java
| | |     | |_ SSOController.java
| | |     |
| | |     |_ core/
| | |     | |_ CurrentUserHandlerMethodArgumentResolver.java
| | |     | |_ SAMLUserDetailsServiceImpl.java
| | |     |
| | |     |_ stereotypes/
| | |     | |_ CurrentUser.java
| | |     |
| | |     |_ Application.java
| | |
| | |_ resources/
| |   |_ application.properties
| | 
| |_ test/
|   |_ 
|
|_ target/

```
