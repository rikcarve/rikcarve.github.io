---
layout: post
date: 2020-05-14
title: Quarkus Security
desc: How to secure your Quarkus app
---
# How to secure your Quarkus app
In my last blog post I experimented with [Quarkus and Qute](https://rikcarve.github.io/2020/02/28/Qute-Quarkus.html).
While only an experiment in the beginning it became soon a side project for my home automation. Some context: I use a 
[Homewizard](https://www.homewizard.com/) gateway for controlling and monitoring different "gadgets" and I like it. But
it has one thing I really found stupid: the homewizard gateway API has the password in URL! No comment.

Of course, I don't want to make that accessible from outside my WLAN ;-) So here's my side project: Make a small webapp
to control and monitor the most important parts of my home-automation. And it should be more secure than the original :-P

Based on Quarkus and Qute I wrote this little webapp, but then the hard part came: security. I'm a backend developer and
architect and whenever it comes to security I quickly put the invisibility cloak around me ;-) But here I am
and nobody else can do it for me.

### Code
First thing: only allow authorized users to access the resource. Thanks to Jakarta EE, this is quite simple:
```java
    @GET
    @RolesAllowed("user")
    public TemplateInstance get() {
``` 
Basically @RolesAllowed is all you need here.

### Credentials
Next thing, manage credentials. Here, Quarkus provides a security-properties [extension](https://quarkus.io/guides/security-properties) 
based on Elytron:
```xml
        <dependency>
            <groupId>io.quarkus</groupId>
            <artifactId>quarkus-elytron-security-properties-file</artifactId>
        </dependency>
```

Configuration can be done in application.proprties:
```properties
quarkus.security.users.file.enabled=true
quarkus.security.users.file.users=users.properties
quarkus.security.users.file.roles=roles.properties
quarkus.security.users.file.realm-name=MyRealm
```

Hash your password to put it in the users.properties file:
```bash
echo -n Foo:MyRealm:s3cr3t | md5sum
```
Yes, I'm aware of the fact that storing passwords (even hashed) in a file is maybe not the way to go. But hey, I'm not dealing
with credit cards here, just some light switches :-P

### Login
Ah well, I should use form-based authentication, but I was too lazy, so I just added basic auth:
```properties
quarkus.http.auth.basic=true
```

### HTTPS
There's no sense using authentication without securing the transport layer, so I had to switch to HTTPS. And that means certificates
and I hate certificates ;-) But as I own a Synology NAS (which is reachable through a dynamic DNS provider), getting a certificate
from Let's encrypt is just a few clicks away. Once there, I could export them. Then I was left with 3 .pem files which
look like a chinese newspaper to me :-(
All I knew was, I need to get them in a Java keystore. My only option: [KeyStore Explorer](https://keystore-explorer.org/):
* create a new keystore (JKS)
* import key pair (OpenSSL format)
* select the privkey.pem and the cert.pem
* save it (e.g. as publickeystore.jks)
And your done! 

### Put it all together
All that's left is enabling HTTPS in Quarkus application.properties:
```properties
%dev.quarkus.http.port=8080
quarkus.http.ssl-port=8443
quarkus.http.ssl.certificate.key-store-file=publickeystore.jks
```

### Conclusion
Security does not have to be hard! And Quarkus (once again) helps you a lot.
Full code on [github](https://github.com/rikcarve/homewizard)

![screenshot](https://rikcarve.github.io/images/lights.png){:height="50%" width="50%"}
