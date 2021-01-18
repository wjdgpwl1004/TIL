# 쓰로틀링과 디바운싱

## 쓰로틀링
-  마지막 함수가 호출된 후 일정 시간이 지나기 전에 다시 호출되지 않도록 하는 것
- 성능개선을 위해, 실행횟수에 제한을 둘 때 사용
- 예를 들어 스크롤 이벤트 등에서 몇 초에 한 번, 몇 밀리초에 한 번씩만 실행되도록 제한을 두어, 과부하를 예방한다.

```js
var timer;
document.querySelector('#input').addEventListener('input', function (e) {
  if (!timer) {
    timer = setTimeout(function() {
      timer = null;
      console.log('여기에 ajax 요청', e.target.value);
    }, 200);
  }
});

```

## 디바운싱
- 연이어 호출되는 함수들 중 마지막 함수(또는 제일 처음)만 호출하도록 하는 것
- 예를 들어 검색창에 단어를 쳐서 검색을 할 때, 검색을 하는 도중에도 계속 API요청이 실행된다. 검색을 하는 도중에 일일히 API를 요청하면 비효율적이고 비용적인 문제가 발생할 수 있다. 
- 그래서 보통 검색어를 모두 입력했을 때 등의 시간을 가정하여 일정시간 뒤에 API요청을 실행하여 이러한 문제를 해결할 수 있다. (EX. 200ms 후에 API요청 등..)

```js
var timer;
document.querySelector('#input').addEventListener('input', function(e) {
  if (timer) {
    clearTimeout(timer);
  }
  timer = setTimeout(function() {
    console.log('여기에 ajax 요청', e.target.value);
  }, 200);
});
```


# 출처
- https://www.zerocho.com/category/JavaScript/post/59a8e9cb15ac0000182794fa
