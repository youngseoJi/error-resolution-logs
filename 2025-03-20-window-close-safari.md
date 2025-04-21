# 새 창 닫기 문제: `<a target="_blank">` vs `window.open()`

## 배경

기존에는 `window.open()` 방식으로 새 창을 열고 `window.close()`로 닫는 구조였다.  
하지만 새 창을 `<a target="_blank">` 방식으로 변경 요청이 있어 적용하였다.  
이때 Safari에서는 이를 팝업으로 인식하여 차단하는 문제가 발생하였다.

- Safari에서는 팝업 차단 해제를 하지 않으면 새 창이 열리지 않음
- 새 창이 바로 뜨도록 하기 위해 `<a target="_blank">` 방식으로 변경
- 문제는 `window.close()`가 더 이상 작동하지 않음
- 이에 따른 해결 방안 및 브라우저별 동작 차이를 정리함

---

## 문제 요약

- `<a target="_blank">` 방식으로 새 창을 열면 `window.close()`가 작동하지 않음
- `window.opener`가 null이기 때문에 보안 정책에 따라 닫기가 차단됨
- 일부 브라우저에서 `window.opener = window.self` 방식으로 닫는 것이 가능함

---

## 새 창 여는 두 가지 방식 정리

### 1. `window.open()` 방식

```js
const newWin = window.open("testClose.html", "_blank", "width=600,height=400");

if (newWin && !newWin.closed) {
  newWin.close(); // 정상적으로 닫힘
}

```

### 2. `<a target="_blank">` 방식
```js
// 새 창 내부에서 실행

if (!window.opener) {
  window.opener = window.self;
}
window.close(); // 일부 브라우저에서는 닫힘
```
window.opener는 null 상태이며 rel="noopener" 자동 적용된다.
보안 정책으로 인해 window.close() 호출이 차단될 수 있음
opener = self로 우회하여 닫는 방법이 일부 브라우저에서 가능함

