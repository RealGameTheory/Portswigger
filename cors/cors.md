#### What is CORS (cross-origin resource sharing)?

Cross-origin resource sharing (CORS) is a browser mechanism which enables controlled access to resources located outside of a given domain. It extends and adds flexibility to the same-origin policy (SOP). However, it also provides potential for cross-domain attacks, if a website's CORS policy is poorly configured and implemented.

**Same-origin policy**

The same-origin policy is a restrictive cross-origin specification that limits the ability for a website to interact with resources outside of the source domain.

The same-origin policy is very restrictive and consequently various approaches have been devised to circumvent the constraints. Many websites interact with subdomains or third-party sites in a way that requires full cross-origin access. A controlled relaxation of the same-origin policy is possible using cross-origin resource sharing (CORS).

#### Vulnerabilities arising from CORS configuration issues

Many modern websites use CORS to allow access from subdomains and trusted third parties. Their implementation of CORS may contain mistakes or be overly lenient to ensure that everything works, and this can result in exploitable vulnerabilities.

#### Lab: CORS vulnerability with basic origin reflection

In this you can see origin can be changed.

<br>
<img src="corslab1_1.png" alt="Alt text" width="1000" height="900">
</br>

In this I put the html code [exploit]{https://github.com/RealGameTheory/Portswigger/blob/main/cors/corsexplab1.html} into the server exploit and then check the access logs.

<br>
<img src="corslab1_2.png" alt="Alt text" width="1000" height="900">
</br>

Decoding the data I got (api visible here too).

<br>
<img src="corslab1_3.png" alt="Alt text" width="1000" height="900">
</br>

Replacing with the session key to login as admin (api visible here too).

<br>
<img src="corslab1_4.png" alt="Alt text" width="1000" height="900">
</br>
