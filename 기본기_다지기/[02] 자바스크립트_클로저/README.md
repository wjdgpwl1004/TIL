# 자바스크립트 클로저

## 전역변수와 지역변수

- 전역변수 : 제일 바깥 범위에 만들어지는 변수
- 지역변수 : 함수안에 들어있는 변수

## 스코프

- 변수에 접근할 수 있는 범위

```js
var x = "global";
function ex() {
  var x = "local";
  x = "change";
}
ex(); // x를 바꿔본다.
alert(x); // 여전히 'global'
```

- 함수 안에 var x = 'local' 은 함수 안에서만 스코프를 가짐.
- 그래서 alert(x); 를 했을 때, 전역에 있는 var x = 'global'; 를 찾아서 값이 'global'이 됨.

```js
var x = "global";
function ex() {
  x = "change";
}
ex();
alert(x); // 'change'
```

- 위의 소스에서는 함수 안에 x 선언된 부분이 없기 때문에, 스코프가 전역까지 넓혀짐.
- 전역 스코프 부분에서 x를 찾아 값을 'change'로 바꿔준다.

## 스코프 체인

- 내부 함수에서는 외부 함수의 변수에 접근 가능. 외부함수에서는 내부함수에 접근 불가능
- 아래의 소스에서 inner 함수는 name 변수를 찾기 위해 한단게씩 올라가서 outer 스코프, 전역스코프를 거쳐 'zero' 값을 얻음. 범위를 한단계씩 올라가, 계속 범위를 넓히면서 찾는 이러한 관계를 '스코프 체인' 이라고 부른다.

```js
var name = "zero";
function outer() {
  console.log("외부", name);
  function inner() {
    var enemy = "nero";
    console.log("내부", name);
  }
  inner();
}
outer();
console.log(enemy); //undefined
```

## 렉시컬 스코핑

- 스코프는 함수를 호출할 때가 아니라 선언할때 생김.
- 정적 스코프
- 스코프는 함수를 선언할 때 생기므로 이미 log 안에 name은 'zero' 전역변수를 가리킨다.

```js
var name = "zero";
function log() {
  console.log(name);
}

function wrapper() {
  var name = "nero";
  log();
}
wrapper(); //zero
```

## 실행 컨텍스트

- 실행 가능한 자바스크립트 코드 블록이 실행되는 환경, 문맥
- 브라우저가 스크립트를 로딩해서 실행시키면 '전역 컨텍스트'가 만들어짐. 페이지가 종료될 때까지 유지
- 함수를 호출할 때마다 '함수 컨텍스트'가 하나씩 생김.
- 컨텍스트 생성 시 컨텍스트 안에 변수객체(arguments, variable), scope chain, this가 생성
- 컨텍스트 생성 후 함수가 실행, 사용된느 변수들은 변수 객체 안에서 값을 찾고, 없다면 스코프 체인을 따라 올라가며 찾음.
- 함수 실행이 마무리되면 해당 컨텍스트는 사라짐. (클로저 제외), 페이지가 종료되면 전역 컨텍스트 사라짐.

```js
var name = "zero"; // (1)변수 선언 (6)변수 대입
function wow(word) {
  // (2)변수 선언 (3)변수 대입
  console.log(word + " " + name); // (11)
}
function say() {
  // (4)변수 선언 (5)변수 대입
  var name = "nero"; // (8)
  console.log(name); // (9) nero
  wow("hello"); // (10)
}
say(); // (7) hello nero
```

## 호이스팅

- 변수를 선언하고 초기화 했을 때 선언 부분이 최상단으로 끌어올려지는 현상

```js
console.log(zero); // 에러가 아니라 undefined
sayWow(); // 정상적으로 wow
function sayWow() {
  console.log("wow");
}
var zero = "zero";
```

## 클로저

- 클로저는 독립적인 (자유) 변수를 가리키는 함수이다. 또는, 클로저 안에 정의된 함수는 만들어진 환경을 ‘기억한다’.
- 아래 소스에서 name 변수나, name 변수가 있는 스코프에 대해 클로저라고 부를 수 있다.

```js
var makeClosure = function () {
  var name = "zero";
  return function () {
    console.log(name);
  };
};
var closure = makeClosure(); // function () { console.log(name); }
closure(); // 'zero';
```

### 클로저의 활용

#### 1. 상태 유지

- 현재 상태를 기억하고 변경된 최신 상태 유지

```html
<!DOCTYPE html>
<html>
  <body>
    <button class="toggle">toggle</button>
    <div
      class="box"
      style="width: 100px; height: 100px; background: red;"
    ></div>

    <script>
      var box = document.querySelector(".box");
      var toggleBtn = document.querySelector(".toggle");

      var toggle = (function () {
        var isShow = false;

        // ① 클로저를 반환
        return function () {
          box.style.display = isShow ? "block" : "none";
          // ③ 상태 변경
          isShow = !isShow;
        };
      })();

      // ② 이벤트 프로퍼티에 클로저를 할당
      toggleBtn.onclick = toggle;
    </script>
  </body>
</html>
```

① 즉시실행함수는 함수를 반환하고 즉시 소멸한다. 즉시실행함수가 반환한 함수는 자신이 생성됐을 때의 렉시컬 환경(Lexical environment)에 속한 변수 isShow를 기억하는 클로저다. 클로저가 기억하는 변수 isShow는 box 요소의 표시 상태를 나타낸다.

② 클로저를 이벤트 핸들러로서 이벤트 프로퍼티에 할당했다. 이벤트 프로퍼티에서 이벤트 핸들러인 클로저를 제거하지 않는 한 클로저가 기억하는 렉시컬 환경의 변수 isShow는 소멸하지 않는다. 다시 말해 현재 상태를 기억한다.

③ 버튼을 클릭하면 이벤트 프로퍼티에 할당한 이벤트 핸들러인 클로저가 호출된다. 이때 .box 요소의 표시 상태를 나타내는 변수 isShow의 값이 변경된다. 변수 isShow는 클로저에 의해 참조되고 있기 때문에 유효하며 자신의 변경된 최신 상태를 게속해서 유지한다.

#### 2. 전역변수의 사용 억제 및 정보의 은닉

```html
<!DOCTYPE html>
<html>
  <body>
    <p>클로저를 사용한 Counting</p>
    <button id="inclease">+</button>
    <p id="count">0</p>
    <script>
      var incleaseBtn = document.getElementById("inclease");
      var count = document.getElementById("count");

      var increase = (function () {
        // 카운트 상태를 유지하기 위한 자유 변수
        var counter = 0;
        // 클로저를 반환
        return function () {
          return ++counter;
        };
      })();

      incleaseBtn.onclick = function () {
        count.innerHTML = increase();
      };
    </script>
  </body>
</html>
```

- 스크립트가 실행되면 즉시실행함수가 호출되고 변수 increase에는 함수 function () {
  return ++counter;
  }; 가 할당된다.
- 클로저인 이 함수는 자신이 선언됐을 때의 렉시컬 환경인 즉시실행함수의 스코프에 속한 지역변수 counter를 기억한다. 
- counter 라는 변수 값을 외부에서는 조작할 수 없으므로, 의도되지 않은 변경을 걱정할 필요가 없다.

#### 3. 자주 발생하는 실수 방지
- 아래 예제는 클로저를 사용할 때 자주 발생할 수 있는 실수.
- 순차적으로 0, 1, 2, 3, 4를 반환할 것 같지만 그렇지 않다.
- for문에서 사용하는 변수 i 는 지역변수이기 때문!

```js
var arr = [];

for (var i = 0; i < 5; i++) {
  arr[i] = function () {
    return i;
  };
}

for (var j = 0; j < arr.length; j++) {
  console.log(arr[j]());
}
```
```js
var arr = [];

for (var i = 0; i < 5; i++){
  arr[i] = (function (id) { // ②
    return function () {
      return id; // ③
    };
  }(i)); // ①
}

for (var j = 0; j < arr.length; j++) {
  console.log(arr[j]());
}
```
① 배열 arr에는 즉시실행함수에 의해 함수가 반환된다.

② 이때 즉시실행함수는 i를 인자로 전달받고 매개변수 id에 할당한 후 내부 함수를 반환하고 life-cycle이 종료된다. 매개변수 id는 자유변수가 된다.

③ 배열 arr에 할당된 함수는 id를 반환한다. 이때 id는 상위 스코프의 자유변수이므로 그 값이 유지된다.

# 출처

- 제로초 블로그 (https://www.zerocho.com/category/JavaScript/post/5741d96d094da4986bc950a0)
- https://poiemaweb.com/js-closure
