# Day8 학습정리

# **함수형 프로그래밍**

## **순수함수**

- 같은 입력을 받았을 때, 같은 출력을 반환
    - 파라미터가 아닌 전역 변수를 포함시켜 결과 반환
    - 전역 변수가 변경되면 출력이 변경됨
- side-effect를 가지지 않음
    - 배열을 파라미터로 받아서 pop하거나 push하면 **side-effect 발생**
- 위의 두 가지 경우와 같이 외부 요인의 영향을 받지 않는 것이 참조 투명성

## **일급 함수**

함수를 다른 변수와 동일하게 취급

- **함수를 매개변수로 사용**
    - 고차함수(map,reduce,filter)등에 사용
    - 함수를 파라미터로 받음
- **함수가 함수를 반환**
- **함수가 변수에 할당**
    - 오브젝트에 키-함수 형식으로 맵핑

## **불변성**

- 메모리에 이미 저장된 어떤 값의 상태를 변경하지 않는 것
- 메모이제이션 방식
- 프로퍼티에 직접 접근하지 말고 반환할 것

## **Lazy evalutation 지연평가**

결과값이 필요할 때 까지 계산을 지연하는 방법

```bash
function* infinity(start) {
  while (true) {
    yield start++;
  }
}
const infinite = infinity();
```

- 무한 루프이지만 필요할 때만 next()를 이용하여 값을 불러옴

# 자바스크립트 고차함수

## **first-class citizen(일급 객체)**

대표적인 예가 일급 함수가 존재

- 변수에 할당 가능
- 다른 함수의 파라미터로 전달 가능
- 다른 함수의 반환값이 될 수 있음

→ 함수를 데이터 다루듯이

## 고차함수

- 함수를 담은 변수 또는 함수 자체를 인자로 받음
    - 이때 인자로 전달되는 함수를 콜백함수라 함
- 함수 형태로 반환

## 내장 고차함수

### **Array.filter()**

- **걸러내기 위한 조건을 명시한 함수**를 인자로 받음

```jsx
const isLteFive = function (str) {
  // Lte = less then equal
  return str.length <= 5;
};

arr.filter(isLteFive);
```

### **Array.map()**

- 하나의 데이터를 다른 데이터로 맵핑(mapping) 할 때 사용

```jsx
const findSubtitle = function (cartoon) {
  return cartoon.subtitle;
};

cartoons.map(findSubtitle);
```

### **Array.reduce()**

- reduce는 여러 데이터를, 하나의 데이터로 통합할 때 사용

```jsx
const scoreReducer = function (sum, cartoon) {
  return sum + cartoon.averageScore;
};

cartoons.reduce(scoreReducer, initialValue)
// initialValue가 sum으로 전달되는 듯?
```

```jsx
function joinName(resultStr, user) {
  resultStr = resultStr + user.name + ', ';
  return resultStr;
}

users.reduce(joinName, '');
// join과 같이 사용도 가능

```

# 자바스크립트 불변 객체

## 원시값과 참조값

**원시값**

- Number, String, Boolean, Null, Undefined 등과 같은 기본 자료형
- 변수에 할당할 때 새로운 값이 만들어져 주소값이 재할당
- 따라서 불변함

**참조값**

- 메모리에 저장되는 객체
- 원시값을 제외한 모든 값은 객체(Object)
- 변수에는 객체가 저장되는 메모리 주소인 객체에 대한 참조 값이 저장

## 얕은 복사

**Object.assign**

```jsx
Object.assign(target, ...sources)
```

- 타깃 객체로 소스 객체의 프로퍼티를 복사
- But 객체 안에 객체까지는 복사 불가능

**Spread 연산자**

```jsx
const user2 = {...user1};
```

- 마찬가지로 얕은 복사이므로 내부 객체는 참조형식

## 깊은 복사

**JSON.parse(JSON.stringify(obj))**

```jsx
const user2 = JSON.parse(JSON.stringify(user1));
```

- stringfy 메서드를 통해 직렬화(serialize)한 뒤, parse 메서드를 통해 이를 역직렬화(deserialize)하는 방식
- 얕은 복사의 객체 내부 객체의 참조를 해결하는 법
- But 함수, 빌트인 타입(Date, Map, RegExp) 등이 포함되어있는 복잡한 객체에서는 정상적인 복사 불가능

**structuredClone**

```jsx
let copiedObj = structuredClone(originalObj);
```

- 자바스크립트에서 새로 지원하기 시작한 깊은 복사를 위한 내장 함수