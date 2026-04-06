## WebSockets
WebSockets are widely used in modern web applications. They are initiated over HTTP and provide long-lived connections with asynchronous communication in both directions.

WebSockets are used for all kinds of purposes, including performing user actions and transmitting sensitive information. Virtually any web security vulnerability that arises with regular HTTP can also arise in relation to WebSockets communications.

> Note: Browse to the application function that uses WebSockets. You can determine that WebSockets are being used by using the application and looking for entries appearing in the WebSockets history tab within Burp Proxy.<br>

> Note: We can configure whether client-to-server or server-to-client messages are intercepted in Burp Proxy. Do this in the Settings dialog, in the WebSocket interception rules settings.

## WebSockets security vulnerabilities

In principle, practically any web security vulnerability might arise in relation to WebSockets:

* User-supplied input transmitted to the server might be processed in unsafe ways, leading to vulnerabilities such as SQL injection or XML external entity injection.
* Some blind vulnerabilities reached via WebSockets might only be detectable using out-of-band (OAST) techniques.
* If attacker-controlled data is transmitted via WebSockets to other application users, then it might lead to XSS or other client-side vulnerabilities.

## Manipulating WebSocket messages to exploit vulnerabilities

The majority of input-based vulnerabilities affecting WebSockets can be found and exploited by tampering with the contents of WebSocket messages.

For example, suppose a chat application uses WebSockets to send chat messages between the browser and the server. When a user types a chat message, a WebSocket message like the following is sent to the server:<br>
`{"message":"Hello Carlos"}`<br>

The contents of the message are transmitted (again via WebSockets) to another chat user, and rendered in the user's browser as follows:<br>
`<td>Hello Carlos</td>`<br>

In this situation, provided no other input processing or defenses are in play, an attacker can perform a proof-of-concept XSS attack by submitting the following WebSocket message:<br>
`{"message":"<img src=1 onerror='alert(1)'>"}`<br>

## Lab: Manipulating WebSocket messages to exploit vulnerabilities
First use intercept and get the message.

<br>
<img src="websocket_lab_1_1.png" alt="Alt text" width="1000" height="900">
</br>

## Manipulating the WebSocket handshake to exploit vulnerabilities

Some WebSockets vulnerabilities can only be found and exploited by manipulating the WebSocket handshake. These vulnerabilities tend to involve design flaws, such as:

* Misplaced trust in HTTP headers to perform security decisions, such as the `X-Forwarded-For` header.
* Flaws in session handling mechanisms, since the session context in which WebSocket messages are processed is generally determined by the session context of the handshake message.
* Attack surface introduced by custom HTTP headers used by the application.

## Lab: Manipulating the WebSocket handshake to exploit vulnerabilities
First I do this: <br>

<br>
<img src="websocket_lab_2_1.png" alt="Alt text" width="1000" height="900">
</br>

Due to which my IP gets blacklisted. Hence we use `X-Forwarded-For` to bypass the blacklisting

<br>
<img src="websocket_lab_2_2.png" alt="Alt text" width="1000" height="900">
</br>

Then finally I execute the same payload as the first but by changing the letter casings which bypass the XSS checker:

<br>
<img src="websocket_lab_2_3.png" alt="Alt text" width="1000" height="900">
</br>
