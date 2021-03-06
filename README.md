# π μ΄ν°λ¬λΈ/μ΄ν°λ μ΄ν° νλ‘ν μ½

- μ΄ν°λ¬λΈ: μ΄ν°λ μ΄ν°λ₯Ό λ¦¬ν΄νλ [Symbol.iterator]() λ₯Ό κ°μ§ κ°
- μ΄ν°λ μ΄ν°: { value, done } κ°μ²΄λ₯Ό λ¦¬ν΄νλ next() λ₯Ό κ°μ§ κ°
- μ΄ν°λ¬λΈ/μ΄ν°λ μ΄ν° νλ‘ν μ½: μ΄ν°λ¬λΈμ for...of, μ κ° μ°μ°μ λ±κ³Ό ν¨κ» λμνλλ‘ν κ·μ½

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

## βμ μ¬ λ°°μ΄ vs μ΄ν°λ¬λΈ

- μ΄ν°λ¬λΈ(iterable) : λ©μλ Symbol.iteratorκ° κ΅¬νλ κ°μ²΄
- μ μ¬ λ°°μ΄ : μΈλ±μ€μ length νλ‘νΌν°κ° μμ΄μ λ°°μ΄μ²λΌ λ³΄μ΄λ κ°μ²΄

# π μ λλ μ΄ν°/μ΄ν°λ μ΄ν°

- μ λλ μ΄ν°: μ΄ν°λ μ΄ν°μ΄μ μ΄ν°λ¬λΈμ μμ±νλ ν¨μ

```javascript
function* gen() {
  yield 1;
  yield 2;
  yield 3;
}

for (const a of gen) console.log(a);
```

# π Map

```javascript
const map = (f, iter) => {
  let res = [];
  for (const a of iter) {
    res.push(f(a));
  }
  return res;
};
```

# π filter

```javascript
const filter = (f, iter) => {
  let res = [];
  for (const a of iter) {
    if (f(a)) res.push(a);
  }
  return res;
};
```

# π reduce

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

## π map + filter + reduce μ€μ²© μ¬μ©κ³Ό ν¨μν μ¬κ³ 

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
