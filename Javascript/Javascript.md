# 2022-04-17
## toggle
toggle을 사용하면 이전 class를 유지하면서 class를 추가/삭제할 수 있다.
```
function handleTitleClick() {
    h1.classList.toggle("clicked");
}
```
***

# 2022-04-19
## preventDefault()
필요하다면 eventHandler에서 event를 받아 브라우저의 기본 동작을 막을 수 있다.  
ex) form이 submit하면 브라우저가 자동으로 새로고침하는 것을 막는다.

```javascript
function eventHandler(event) {
    event.preventDefault();
}
loginForm.addEventListener("submit", eveneHandler);
```

## alert
alert() blocks javascript working. So usually don't use it.

# 2022-04-21
## parentNode vs. parentElement
Almost same.
- If a node's parent is not an element, parentElement returns null.
- For Internet Explorer, parentNode is defined for SVG elements, but parentElement is undefined.

```javascript
document.body.parentNode; // the <html> element
document.body.parentElement; // the <html> element

document.querySelector('html').parentNode; // the document node
document.querySelector('html').parentElement; // null
```

일반적으로 둘중 무엇을 써도 상관없을 것이다.
