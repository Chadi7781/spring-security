
<h1 align="center">Wram Up</h1>
<p> 
Before diving into the technical details, I want to explicitly define the terminology used in the Spring Security context just to be sure that we all speak the same language.

These are the terms we need to address:

Authentication refers to the process of verifying the identity of a user, based on provided credentials. A common example is entering a username and a password when you log in to a website. You can think of it as an answer to the question Who are you?.
Authorization refers to the process of determining if a user has proper permission to perform a particular action or read particular data, assuming that the user is successfully authenticated. You can think of it as an answer to the question Can a user do/read this?.
Principle refers to the currently authenticated user.
Granted authority refers to the permission of the authenticated user.
Role refers to a group of permissions of the authenticated user.
</p>
<h4 align ="center">Spring JPA Repository</h4>
<p align="center">Spring Data JPA Repositories
Spring Data JPA Repositories help you reduce boilerplate code required to implement data access layers for various persistence stores such as MySQL and PostgreSQL
</p>
<p align ="center">They provide some CRUD functions to query, create, update and delete against the underlying database such as findAll, findById, save, saveAll, delete and deleteAll</p>
<h4 align="center">Authentification-App-Srping-Security</h4>
<img src="https://bs-uploads.toptal.io/blackfish-uploads/uploaded_file/file/412345/image-1602672495860.085-952930c83f53503d7e84d1371bec3775.png">
<h3 align="center">AuthenticationFilter</h3>
<p align ="center">This is the filter that intercepts requests and attempts to authenticate it. In Spring Security, it converts the request to an Authentication Object and delegates the authentication to the AuthenticationManager.
</p>

<h3 align="center">AuthenticationManager</h3>
<p align ="center">
It is the main strategy interface for authentication. It uses the lone method authenticate() to authenticate the request. The authenticate() method performs the authentication and returns an Authentication Object on successful authentication or throw an AuthenticationException in case of authentication failure. If the method can’t decide, it will return null. The process of authentication in this process is delegated to the AuthenticationProvider which we will discuss next.
</p>
<h3 align="center">AuthenticationProvider</h3>
The AuthenticationManager is implemented by the ProviderManager which delegates the process to one or more AuthenticationProvider instances. Any class implementing the 

AuthenticationProvider interface must implement the two methods – authenticate() and supports(). First, let us talk about the supports() method. It is used to check if the particular authentication type is supported by our AuthenticationProvider implementation class. If it is supported it returns true or else false. Next, the authenticate() method. Here is where the authentication occurs. If the authentication type is supported, the process of authentication is started. Here is this class can use the loadUserByUsername() method of the UserDetailsService implementation. If the user is not found, it can throw a UsernameNotFoundException.

On the other hand, if the user is found, then the authentication details of the user are used to authenticate the user. For example, in the basic authentication scenario, the password provided by the user may be checked with the password in the database. If they are found to match with each other, it is a success scenario. Then we can return an Authentication object from this method which will be stored in the Security Context, which we will discuss later.

<h3 align="center">UserDetailsService</h3>
It is one of the core interfaces of Spring Security. The authentication of any request mostly depends on the implementation of the UserDetailsService interface. It is most commonly used in database backed authentication to retrieve user data. The data is retrieved with the implementation of the lone loadUserByUsername() method where we can provide our logic to fetch the user details for a user. The method will throw a UsernameNotFoundException if the user is not found.

<h3 align="center">PasswordEncoder</h3>
PasswordEncoder to store passwords. This encodes the user’s password using one its many implementations. The most common of its implementations is the BCryptPasswordEncoder. Also, we can use an instance of the NoOpPasswordEncoder for our development purposes. It will allow passwords to be stored in plain text. But it is not supposed to be used for production or real-world applications.

<h3 align="center">Spring Security Context</h3>
This is where the details of the currently authenticated user are stored on successful authentication. The authentication object is then available throughout the application for the session. So, if we need the username or any other user details, we need to get the SecurityContext first. This is done with the SecurityContextHolder, a helper class, which provides access to the security context. We can use the setAuthentication() and getAuthentication() methods for storing and retrieving the user details respectively.

Moving on, let’s now discuss the three custom implementations we are going to use for our application.

<h3 align="center">Form Login</h3>
When we add Spring Security to an existing Spring application it adds a login form and sets up a dummy user. This is Spring Security in auto-configuration mode. In this mode, it also sets up the default filters, authentication-managers, authentication-providers, and so on. This setup is an in-memory authentication setup. We can override this auto-configuration to set up our own users and authentication process. We can also set up our custom login method like a custom login form. Spring Security only has to made aware of the details of the login form like – the URI of the login form, the login processing URL, etc.. It will then render our login form for the application and carry out the process of authentication along with the other provided configurations or Spring’s own implementation.
# spring-security
