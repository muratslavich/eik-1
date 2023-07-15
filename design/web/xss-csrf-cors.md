# xss csrf cors

In a cross-scripting attack (XSS), the attacker can execute malicious  code in the victim’s browser. This code is usually injected by the  attacker when the victim browses a trusted site. There are three types  of XSS — Stored XSS, Reflected XSS, and DOM-based XSS.\
An attacker who exploits XSS will be able to harvest credentials,  redirect victims to phishing pages, and hijack a user session using  cookies.\
достаточно внедрить код в базу или какой-нибудь файл на сервере.фишинговый ссылки.Выполняясь в браузере клиента код может слить твои куки злоумышленнику.\




\
In a Cross-site request forgery (CSRF), the attacker sends a request to the browser that seems like it was made by the user.To do this, the victim is first tricked into clicking a link. This is  followed by sending a seemingly legitimate request to the website. This  request with cookies the victim has associated with the website. A CSRF  attack can work only when the victim is logged in to an account.\
запрос отправляется с уязвимого сервера на неуязвимый с твоими кредами.\




\
**Cross-Origin Resource Sharing** ([CORS](https://developer.mozilla.org/en-US/docs/Glossary/CORS)) is an [HTTP](https://developer.mozilla.org/en-US/docs/Glossary/HTTP)-header based mechanism that allows a server to indicate any other [origin](https://developer.mozilla.org/en-US/docs/Glossary/origin)s (domain, protocol, or port) than its own from which a browser should permit loading of resources.
