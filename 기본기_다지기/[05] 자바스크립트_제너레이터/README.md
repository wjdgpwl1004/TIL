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

# 코루틴

## 서브루틴
- 소프트웨어에서 특정 동작을 수행하는 일정 코드 부분을 의미                      

## 코루틴
- 루틴의 일종으로서, 협동 루틴이라 할 수 있다
- 상호 연계 프로그램을 일컫는다고도 표현가능
- 루틴과 서브 루틴은 서로 비대칭적인 관계이지만, 코루틴들은 완전히 대칭적인, 즉 서로가 서로를 호출하는 관계
- 코루틴은 suspend/resume가 가능하며, 함수가 call/return되는 것과 비교하면 더 일반화된 형태라 할 수 있다.

```js
const getId = () =>
  new Promise(resolve => {
    setTimeout(() => resolve(1), 1)
  })
const getNameById = id =>
  new Promise(resolve => {
    setTimeout(() => resolve("chris"), 1)
  })

function* gen() {
  const id = yield getId()
  const name = yield getNameById(id)
  console.log({ id, name })
}

const g = gen()
g.next().value.then(id => {
  g.next(id).value.then(name => {
    g.next(name)
  })
})
```
- next(main) → promise(gen) → yield(gen) → then(main) →  // id 획득
- next(main) → promise(gen) → yield(gen) → then(main) → // name 획득
- 메인과 sub루틴 사이에서 제어권을 핑퐁하듯이 주고 받고 있는 모습이다. 이것을 코루틴(coroutine)이라고 부른다.

# 출처
- https://ko.javascript.info/generators
- https://jeonghwan-kim.github.io/2016/12/15/coroutine.html
