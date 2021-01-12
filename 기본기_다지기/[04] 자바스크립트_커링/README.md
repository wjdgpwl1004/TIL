# 자바스크립트 커링

## 일급함수
- 함수를 값으로 다루는 개념
- 변수에 함수가 값으로서 담길 수 있다.

```js
var f1 = function(a) { return a * a; };
```

## 커링(currying)
- 커링은 인자를 여러개 받는 함수를 분리하여, 인자를 하나씩만 받는 함수의 체인으로 만드는 방법
- 함수와 인자를 다루는 기법, 함수에 인자를 하나씩 적용하다가, 필요한 인자가 채워지면 함수본체를 실행하는 기법
- 자바스크립트는 일급함수를 지원하고, 평가시점 다룰 수 있어서 커링기법을 구현할 수 있다.
- 함수를 인자로 받고, 커리함수를 실행하는 즉시 함수를 리턴, 그리고 또 함수를 실행, 미리받았던 함수 본체를 안쪽에서 평가
- 본체함수를 값으로 들고있다가, 원하는시점까지 미뤘다가 최종적으로 평가하는 기법
```js
    function curry(fn) {
        return function(a) {
            return function(b) {
                return fn(a, b);
            }
        }
    }

    var add = _curry(function(a, b) {
  return a + b;
});

var add10 = add(10);
var add5 = add(5); // 여러가지 add함수를 만들 수 있다.
console.log( add10(5) );
console.log( add(5)(3) );
console.log( add5(3) );
console.log( add(10)(3) );

```

```js
function curry(fn) {
    return function(a, b) {
        if(arguments.length == 2) return fn(a, b); // 인자가 2개 들어오면 한번에 즉시 평가, 하나만 들어오면 한 번 더 실행 미룸
        return function(b) {
            return fn(a, b);
        }
    }
}

```

```js
// 리팩토링
function curry(fn) {
    return function(a, b) {
        return arguments.length == 2 ? fn(a, b) : function(b) {return fn(a, b); };
    }
}
console.log( add(1, 2) ); // 인자가 두개 들어오면 바로 함수 실행하게 하면 동작 가능
```
```js
// 보통 커리함수는 왼쪽 인자부터.., 근데 오른쪽 인자부터 받는 함수 curryr 만들기
function _curryr(fn) {
    return function(a, b) {
        return arguments.length == 2 ? fn(a, b) : function(b) {return fn(b, a); };
    }
}

var sub = _curryr(function(a, b) {
  return a - b;
});


console.log( sub(10, 5) );

var sub10 = sub(10);
console.log(sub10(5));
```

# 고차함수
- 함수를 값으로 다루는 함수
- 함수를 인자로 받아서 실행해주는 함수
- 함수를 만들어서 리턴하는 함수
- 이것이 가능한 이유는 자바스크립트에서는 함수를 일급객체로 취급
- 일급객체 : 컴퓨터 프로그래밍 언어에서 일반적으로 다른 객체들에 적용 가능한 연산을 모두 지원하는 객체

```js
const apply1 = f => f(1);
  const add2 = a => a + 2;
  log(apply1(add2));
  log(apply1(a => a - 1));
```

```js
const addMaker = a => b => a + b;
  const add10 = addMaker(10);
  log(add10(5));
  log(add10(10));
```

# 출처
- 인프런 함수형 프로그래밍 강의(유인동)
