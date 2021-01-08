# 자바스크립트 Promise

## 콜백함수
- 파라미터로 함수를 전달하는 함수
- 콜백함수(Callback Function)란 파라미터로 함수를 전달받아, 함수의 내부에서 실행하는 함수

```js
let number = [1, 2, 3, 4, 5];

number.forEach(x => {
    console.log(x * 2); // 2, 4,6, 8, 10
});
```
- Ex : forEach 함수의 경우 함수 안에 익명의 함수를 넣어서 forEach 문을 동작 

## 콜백지옥
- 자바스크립트에서는 어떤 작업을 요청하면서 콜백 함수를 등록하면, 작업이 수행되고 나서 결과를 나중에 콜백 함수를 통해 알려주는 패턴이 흔하게 사용된다.
- 하나의 작업을 콜백으로 결과를 받은 뒤 순차적으로 다음 작업을 진행하고자 할 때 콜백 중첩, 이른바 콜백지옥을 만날 수 있다.

- 콜백지옥의 예
```js
function add(x, callback) {
    let sum = x + x;
    console.log(sum);
    callback(sum);
}

add(2, function(result) {
    add(result, function(result) {
        add(result, function(result) {
            console.log('finish!!');
        })
    })
})

<output>
4
8
16
finish!!
```

## Promise
- 위와 같이 콜백지옥을 해결 할 수 있는 패턴이 Promise 패턴이다.
- Promise는 객체이다.
- resolve : 성공했을 때 결과를 전달
- reject : 실패했을 때 에러를 전달

```js
const promise = new Promise((resolve, reject) => {
  try {
    ...비동기 작업
    resolve(결과);
  } catch (err) {
    reject(err);
  }
});
```

- 위와 같은 Promise 객체는 then, catch로 결과를 받는다. resolve(결과)의 결과가 then의 result로 가고, reject(err)의 err이 catch의 err로 간다.
```js
promise.then((result) => {
  // result 처리
}).catch((err) => {
  console.error(err);
});
```
- Promise는 코드를 분리할 수 있다. 또한 then을 여러 번 연속해서 쓸 수 있다.
```js
const promise1 = new Promise((resolve, reject) => { ... });
const promise2 = new Promise((resolve, reject) => { ... });
if (조건문) {
  promise1.then((result) => {...});
} else {
  promise2.then((result) => {...});
}
```

### Promise.all
- 여러 프로미스 객체들을 한번에 모아서 처리 가능
```js
let p1 = Promise.resolve('zero'); // new Promise 없이 성공한 Promise 객체를 만드는 방법
let p2 = Promise.resolve('nero');
let p3 = Promise.reject('error'); // new Promise 없이 실패한 Promise 객체를 만드는 방법
Promise.all([p1, p2, p3]).then((result) => {
  console.log(result); // 만약 p3가 resolve였다면 ['zero', 'nero', 'error']
}).catch((err) => {
  console.error(err); // error (p3가 reject이기 때문)
});
```

## async/await
- Promise 패턴은 콜백지옥의 단점을 보완함에도 불구하고, 여전히 코드가 장황해보일 수 있다.
- 코드를 줄여 Promise 패턴의 단점을 보완하고, 비동기 코드를 동기 형태로 만드는 것이 async/await 이다.
- await 부분이 Promise를 받아 처리한다. 
- await은 반드시 async 함수 바로 안에서만 쓰여야한다.
```js
async function findUser() {
  try {
    let user = await Users.findOne({}).exec();
    user.name = 'zero';
    user = await user.save();
    user = await Users.findOne({ gender: 'm' }).exec();
    ...
  } catch (err) {
    console.error(err);
  }
}
```
- try catch 문으로 감싸서 명시적으로 에러를 처리할 수 있다.
```js
async function another() {
  try {
    let result = await returnPromise();
  } catch (err) {
    console.error(err);
  }
}
```

# 출처

- https://www.zerocho.com/category/ECMAScript/post/5770c27e6a8e09150013f0f7
- https://www.zerocho.com/category/EcmaScript/post/58d142d8e6cda10018195f5a
- https://velog.io/@minidoo/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%BD%9C%EB%B0%B1-%ED%95%A8%EC%88%98Callback-Function
