---
isChild: true
title:   Sicurezza nelle applicazioni Web
anchor:  web_application_security
---

## Sicurezza nelle applicazioni Web {#web_application_security_title}

Risulta davvero molto importante che ogni sviluppatore PHP apprenda [le basi sulla sicurezza delle applicazioni web][4]
It is very important for every PHP developer to learn [the basics of web application security][4], which can be broken
down into a handful of broad topics:

1. Code-data separation.
   * When data is executed as code, you get SQL Injection, Cross-Site Scripting, Local/Remote File Inclusion, etc.
   * When code is printed as data, you get information leaks (source code disclosure or, in the case of C programs,
     enough information to bypass [ASLR][5]).
2. Application logic.
   * Missing authentication or authorization controls.
   * Input validation.
3. Operating environment.
   * PHP versions.
   * Third party libraries.
   * The operating system.
4. Cryptography weaknesses.
   * [Weak random numbers][6].
   * [Chosen-ciphertext attacks][7].
   * [Side-channel information leaks][8].

Ci sono cattive persone pronte e desiderose di manipolare la tua applicazione
Web. È importante che prendi le precauzioni necessarie per irrigidire la
sicurezza della tua applicazione Web. Fortunatamente, i ragazzi
dell'[Open Web Application Security Project][1] hanno compilato una lunga lista
di vulnerabilità di sicurezza note e metodi per proteggerti contro di esse.
Questa guida dev'essere necessariamente letta da qualunque sviluppatore che
abbia a cura la sicurezza.

* [Leggi la guida di sicurezza OWASP][2]

[1]: https://www.owasp.org/
[2]: https://www.owasp.org/index.php/Guide_Table_of_Contents
[3]: https://phpsecurity.readthedocs.io/en/latest/index.html
[4]: https://paragonie.com/blog/2015/08/gentle-introduction-application-security
[5]: https://www.techtarget.com/searchsecurity/definition/address-space-layout-randomization-ASLR
[6]: https://paragonie.com/blog/2016/01/on-design-and-implementation-stealth-backdoor-for-web-applications
[7]: https://paragonie.com/blog/2015/05/using-encryption-and-authentication-correctly
[8]: https://blog.ircmaxell.com/2014/11/its-all-about-time.html
