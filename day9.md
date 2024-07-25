# Day9 학습정리

# **발행-구독 패턴(Publisher-Subscriber Pattern)**

- pub(발행자) 메시지의 수신자가 정해져 있지 않음 → 브로드캐스트 느낌
- 단 미리 지정된 범주의 sub(구독자)에게 메시지가 전달이 되어야함
- pub와 sub는 서로의 정보를 모름

### 발행자

- pub이 sub의 선언 위치나 존재를 알 필요가 없음
- 중간 지점(브로커)에 메시지를 전달하면 브로커가 알아서 처리

### 구독자

- sub 역시 pub의 선언 위치나 존재를 알 필요 없음
- 브로커를 모니터링하면서 작업이 할당되면 작업을 처리

### 브로커

- 발행자와 구독자 사이에 위치
- 주로 메시지큐가 브로커로서의 역할을 수행
- 두 객체 사이에서 구독과 발행 이후의 메시지를 처리
    
    → 들어오는 메시지 필터링 및 구독자에게 배포
    

### 메시지 큐

프로세스나 프로그램 인스턴스가 데이터 상호 교환 시 사용하는 통신 방법

MOM(Message Oriented Middleware : 비동기 메시지를 이용한 데이터 송수신)의 구현 

- 비동기(Asynchronous) : 큐에 넣어서 나중에 처리 가능
- 비동조(Decoupling) : 앱과 분리 가능
- 탄력성(Resilience) : 일부 실패가 전체에 영향x
- 과잉(Redunadancy) : 실패할 경우 재실행 가능
- 보증(Guarantees) : 작업 처리 확인 가능
- 확장성(Scalable) : 다수 프로세스들이 큐에 메시지 보내기 가능

### 옵저버 패턴과 비교

발행-구독 패턴은 옵저버와 옵저버블 사이에 브로커(= 메시지 큐, 이벤트 버스)라고 불리는 중개자가 존재

- 옵저버 패턴은 옵저버와 옵저버블이 서로에 대한 정보를 가지고 있음
- 발행-구독 패턴의 결합도가 더 낮음
- 옵저버 패턴은 동기적, 발행-구독 패턴은 비동기적
- 옵저버 패턴은 단일 도메인, 발행-구독 패턴은 크로스 도메인

# **Node.js Worker threads**

## **Worker threads?**

자바스크립트 코드를 병렬처리 해주는 모듈

```jsx
const { Worker } = require('worker_threads') 
```

위와 같이 import하여 사용 가능

## 자바스크립트의 스레드

자바스크립트는 웹 페이지에서의 동시성 문제 해결을 위해 단일 스레드로 구성이 되어있음

하지만 `worker_threads` 모듈을 이용하여 멀티 스레드 환경 구축 가능

## 사용법

```jsx
// index.js -> 부모 스레드 
import { Worker } from 'worker_threads';

const worker = new Worker("./worker.js");
setTimeout(() => console.log("parent"));
```

```jsx
// worker.js -> 자식 스레드 
console.log('child')
```

### 새로운 스레드(worker) 만들기

```jsx
const worker = new Worker(’./worker.js’)
```

<aside>
💡 new Worker(filename) 또는 new Worker(code, {eval: true})

</aside>

- 파일로 스레드 엔트리포인트 정하기
- 실행하고자 하는 코드를 작성 및 넘기기

```jsx
import { Worker, isMainThread } from 'worker_threads';
if(isMainThread){
	const worker = new Worker(__filename);
	// 메인 스레드 내용
}
else{
	//워커 스레드 내용 
}
```

- `isMainThread` 를 이용하여 단일 파일에서 워커 설정 가능

### 다중 스레드 사용

```jsx
import { Worker, isMainThread } from 'worker_threads';
if (isMainThread) {
	const threads = new Set();
	threads.add(
    new Worker(__filename, {
      workerData: {start: 1},
    })
  );
  threads.add(
    new Worker(__filename, {
      workerData: {start: 2},
    })
	);
}
	
```

- Set에다 Worker 저장 및 사용

### **메세지 전송**

부모 스레드(메인 스레드)와 자식 스레드(worker) 간의 통신

```jsx
// index.js
import { Worker } from "worker_threads";

const worker = new Worker("./worker.js");
worker.on("message", (msg) => {
  console.log(msg);
});
console.log("parent");
```

```jsx
// worker.js
import { parentPort } from "worker_threads";

parentPort.postMessage("hello parent!");
console.log("child");
```

index.js에서 생성한 worker가 parent에게 `‘hello parent!’`라는 메시지를 보낸 뒤, 그 이벤트를 받은 parent가 메시지를 출력하는 코드

- **worker.on(이벤트 타입, 이벤트 타입에 따라 달라짐)**
    - 첫 번째 인자로 이벤트 타입 받음
    - 다음 인자를 활용하여 이벤트 핸들링
    - 이벤트 타입은 `‘message’`, `‘error’`, `‘exit’` 가 존재
- **postMessage(입력 값)**
    - 입력 값을 수신단으로 전송
    - 원시값, Array, Boolean, String, Set, Map 등 여러 타입을 전달 가능
    - 다만 Object, Error는 조건 존재
- **worker.terminate()**
    - 워커 스레드의 모든 실행을 ‘가능한 한’ 빨리 정지
    - Promise를 반환
    - 대신 process.exit()도 사용 가능
- parentPort: parent thread와 communication하는 도구
    
    ```jsx
    parentPort.postMessage('pong');
    ```
    
    parent thread로 메세지를 보내기
    
    ```jsx
    parentPort.on('message',()=>{})
    ```
    
    parent thread로 부터 메세지 받기
    

# EventEmitter

Nodejs의 EventLoop는 다양한 유형의 비동기 이벤트를 특정 순서대로 처리

반면에 EventEmitter는 특정 이벤트에 리스너 함수를 달아서, 이벤트가 발생했을 때 이를 캐치할 수 있도록 만들어진 api