# 자바스크립트 제너레이터

## 제너레이터
- 제너레이터(generator)를 사용하면 여러 개의 값을 필요에 따라 하나씩 반환(yield)할 수 있다.
- 제너레이터를 만들려면 '제너레이터 함수’라 불리는 특별한 문법 구조, function*이 필요
- 제너레이터 함수를 호출하면 코드가 실행되지 않고, 대신 실행을 처리하는 특별 객체, '제너레이터 객체’가 반환된다.

```js
function* generateSequence() {
  yield 1;
  yield 2;
  return 3;
}

let generator = generateSequence();
alert(generator); // [object Generator]

```
- generateSequence()의 함수 본문 코드는 아직 실행되지 않은 상태이다.

### next()
- next()를 호출하면 가장 가까운 yield문을 만날 때까지 실행이 지속됨. 이후, yield문을 만나면 실행이 멈추고 산출하고자 하는 값인 value가 바깥 코드에 반환됨.
- value: 산출 값
- done: 함수 코드 실행이 끝났으면 true, 아니라면 false

```js
function* generateSequence() {
  yield 1;
  yield 2;
  return 3;
}

let generator = generateSequence();

let one = generator.next();

alert(JSON.stringify(one)); // {value: 1, done: false}
```
- 현재로서는 첫 번째 값만 받았으므로 함수 실행은 두 번째 줄에서 멈춤.

```js
let two = generator.next();

alert(JSON.stringify(two)); // {value: 2, done: false}

let three = generator.next();

alert(JSON.stringify(three)); // {value: 3, done: true}
```
- 끝까지 호출하면 실행은 return문에 다다르고 제너레이터 함수가 종료됨.


# 출처
- https://ko.javascript.info/generators
- https://jeonghwan-kim.github.io/2016/12/15/coroutine.html
