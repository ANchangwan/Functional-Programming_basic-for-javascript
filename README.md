# 😀 이터러블/이터레이터 프로토콜

- 이터러블: 이터레이터를 리턴하는 [Symbol.iterator]() 를 가진 값
- 이터레이터: { value, done } 객체를 리턴하는 next() 를 가진 값
- 이터러블/이터레이터 프로토콜: 이터러블을 for...of, 전개 연산자 등과 함께 동작하도록한 규약

```javascript
const iterable = {
  [Symbol.iterator]() {
    let i = 3;
    return {
      next() {
        return i == 0 ? { done: true } : { value: i--, done: false };
      },
      [Symbole.iterator]() {
        return this;
      },
    };
  },
};

const iterator = iterable[Symbol.iterator]();

console.log(iterator.next()); // {value : 3, done : false}
console.log(iterator.next()); // {value : 2, done : false}
console.log(iterator.next()); // {value : 1, done : false}
console.log(iterator.next()); // {done : true}

for (const iter of iterable) console.log(iter); // 3 2 1
```

## ✍유사 배열 vs 이터러블

- 이터러블(iterable) : 메서드 Symbol.iterator가 구현된 객체
- 유사 배열 : 인덱스와 length 프로퍼티가 있어서 배열처럼 보이는 객체

# 😀 제너레이터/이터레이터

- 제너레이터: 이터레이터이자 이터러블을 생성하는 함수

```javascript
function* gen() {
  yield 1;
  yield 2;
  yield 3;
}

for (const a of gen) console.log(a);
```

# 😀 Map

```javascript
const map = (f, iter) => {
  let res = [];
  for (const a of iter) {
    res.push(f(a));
  }
  return res;
};
```

# 😀 filter

```javascript
const filter = (f, iter) => {
  let res = [];
  for (const a of iter) {
    if (f(a)) res.push(a);
  }
  return res;
};
```

# 😀 reduce

```javascript
const reduce = (f, acc, iter) => {
  if (!iter) {
    iter = acc[Symbol.iterator]();
    acc = iter.next().value;
  }
  for (const a of iter) {
    acc = f(acc, a);
  }
  return acc;
};
```

## 😀 map + filter + reduce 중첩 사용과 함수형 사고

```javascript
const user = [
  { name: "Tom", age: 26 },
  { name: "jessy", age: 21 },
  { name: "jack", age: 36 },
];

const add = (a, b) => a + b;

console.log(
  reduce(
    add,
    map(
      (u) => u.name,
      filter((u) => u.age < 30, user)
    )
  )
);

console.log(
  reduce(
    add,
    filter(
      (u) => u < 30,
      map((u) => u.age, user)
    )
  )
);
```
