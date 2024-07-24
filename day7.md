# Day7 학습정리

## Testing

### Testing 단위

- Unit test : 의존성X, 하나의 로직 테스트
- Integration test : 다른 함수의 의존성 O
- E2E test : 하나의 기능 테스트

### Jest?

- 페이스북에서 만든 JavaScript testing framework
- config없이 빠르게 테스팅 환경 구성

### Jest 설치

```bash
npm i --save-dev jest
```

### Jest 사용

Jest는 `.test` `.spec`가 포함되거나 `__test__` 폴더 내에 있는 파일을 감지하여 테스트를 실행

Jest의 it()와 test()는 같은 함수,  [jest.It](http://jest.it/).()를 가르키고 있음

```jsx
const { default: test } = require("node:test");
const fn = require("./fn"); //테스트 함수

test("2 더하기 3은 5야.", () => {
  expect(fn.add(2,3)).toBe(5);
});
```

### matchers

- `toBe()` : 해당 값과 일치하면 통과
- `toBeNull()` `toBeUndefined()` `toBeDefined()` : 인 경우 통과
- `toBeTruthy()` `toBeFasly()` : boolean 값 판별
- `toBeGreaterThan` 등… : 이상, 이하, 초과, 미만
- `toMatch(/H/)` : 정규 표현식으로 문자열 판단
- `toEqual()` : 참조값(객체, 배열)을 비교할 때. 해당 값 포함하면 true
- `toStrictEqual()` : 더 엄격한 비교. 완전히 똑같아야 true
- `toContain()` : 배열에서 아이템 포함되어 있는지
    - ex) `expect(["a", "b", "c"]).toContain("a")`

### Error

```jsx
const fn = require("./fn");

test("에러가 발생하나요?", () => {
  expect(()=> fn.throwErr()).toThrow("Error number 1") // 같은 에러메시지어야 true
});
```