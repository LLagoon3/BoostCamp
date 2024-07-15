# 학습 정리

# 자바스크립트의 빌드

V8은 Google Chrome 및 Node Js에서 사용하는 자바스크립트 엔진이다.

V8엔진은 자바스크립트 코드 처리를 위해 3단계의 과정을 거친다.

## 파싱

- 코드가 토큰으로 분해  AST로 변환되는 과정
- 이를 바탕으로 스코프도 생성됨

## 컴파일

**JLT(Just-In-Time)**

- V8엔진이 사용
- 동적 번역이며 프로그램이 실행될 때, 바이트 코드(중간언어 IR)를 기계어로 번역
- 일반적으로 다양한 경우의 수가 존재하면 slow case로 코드를 넘기지만 모두 최적화하지는 못함

**AJITC(Adaptive JIT Compilation)**

- 반복수행되는 정도에 따라 유동적으로 다른 최적화를 적용
- 모든 코드가 처음에 인터프리터로 순차적으로 실행
- 자주 반복되는 부분(hotspot)이 보이면 그 부분만 JIT를 이용해 네이티브 코드로 변환

# Git 동작

## 1. git add

`git add` 명령어는 작업 디렉터리에서 변경된 파일을 스테이징 영역으로 이동시키는 역할

- **작업 디렉터리(Working Directory)**: 로컬에서 실제로 파일을 수정하는 곳
- **스테이징 영역(Staging Area)**: 커밋할 파일과 변경 사항을 임시로 저장하는 곳
- **리포지토리(Repository)**: 커밋된 변경 사항이 영구적으로 저장되는 곳

### 내부 동작

1. **파일 스냅샷 생성**: `git add` 명령어는 작업 디렉터리의 지정된 파일에 대한 스냅샷을 생성한 뒤, 이를 스테이징 영역에 저장한다.
2. **스테이징 영역 업데이트**: 이 스냅샷이 스테이징 영역에 추가되며 이는 다음 커밋에 포함될 파일과 그 파일의 변경 사항이다.

## 2. git commit

`git commit` 은 스테이징 영역에 있는 모든 파일 스냅샷을 하나의 커밋으로 **리포지토리**에 저장한다.

### 내부 동작

1. **새 커밋 객체 생성**: Git은 현재 스테이징 영역에 있는 모든 파일 스냅샷을 기반으로 새 커밋 객체를 만든다.
    - 아래는 커밋 객체에 포함되는 정보이다.
    - 트리 객체에 대한 포인터
    - 부모 커밋에 대한 포인터
    - 커밋 메시지
    - 커밋하는 사람의 메타데이터
2. **트리 객체 생성**: Git은 커밋할 파일들의 스냅샷을 저장하는 트리 객체를 생성하며, 이 트리 객체는 커밋된 파일들의 디렉터리 구조이다.
3. **커밋 객체 업데이트 :** 위에서 생성된 커밋 객체를 업데이트 한다.
4. **HEAD 포인터 업데이트**: 현재 브랜치의 HEAD 포인터가 새 커밋 객체를 가리키도록 업데이트한다.

# 디버그 콘솔과 터미널

## 파일 실행

- **터미널에서 Node.js 파일 실행**
    
    ```bash
    node yourfile.js
    ```
    
    단순 실행과 빠른 테스트가 가능
    
- **디버그 콘솔에서 Node.js 파일 실행**
    
    디버깅 도구가 지원됨으로 단계별 디버깅이 가능
    

## 디버그 콘솔과 터미널의 출력 방식

`가나다 A`와 `가나다라`를 각각 콘솔과 터미널에서 출력해보면, 콘솔에서는 `가다나 A`가 더 길게 출력되고 터미널에서는 두 단어의 길이가 동일하게 출력된다. 이러한 차이에 대해 조사해보았다.

### 1. 폰트의 차이

- **터미널:** 대부분의 터미널은 고정 폭 폰트를 사용한다. 고정 폭 폰트는 모든 문자가 동일한 너비를 가지므로, 문자열의 길이가 항상 정확하게 맞춰진다.
- **디버그 콘솔:**  IDE 내 디버그 콘솔은 가변 폭 폰트를 사용한다. 가변 폭 폰트에서는 문자마다 너비가 다르다. 추가적으로 한국어 문자는 특히 이러한 가변 폭 폰트에서 너비 차이가 두드러질 수 있다.

### 2. 텍스트 렌더링 방식

- **터미널:** 텍스트는 간단하게 렌더링되며, 주로 문자 단위로 동일한 크기를 유지한다. 따라서 특수 문자나 넓은 문자를 포함한 텍스트라도 고정된 폭으로 처리된다.
- **디버그 콘솔:** 텍스트 렌더링이 더 복잡합니다. 문자의 너비, 폰트 스타일, 글꼴 설정 등에 따라 다르게 표시될 수 있다. 유니코드 문자나 다양한 언어의 문자를 포함할 경우 터미널과 다르게 표현될 가능성이 더 높다.

# TroubleShooting

## 트러블 슈팅이란?

- 문제가 발생했을때 원인을 규명하고 해결하는 작업
- 가장 단순하고 빈도 높은 원인에서 가능성을 지워가는 것
- 문제 정의 → 사실 수집 → 원인 추론 →문제 조사 → 결과 관찰 → 문서화

## gist push 문제

- gist commit 후 push 할 경우 다음과 같은 에러 발생
    
    ```
    git push ‘https://gist.github.com/{gist_ID}.git/’
    
    remote: Repository not found.
    fatal: repository ‘https://gist.github.com/{gist_ID}.git/’ not found
    ```
    
- github 토큰 권한 문제로 파악
    
    ```
    Settings → Developer Settings → Tokens(classic) → Edit personal access token (classic) → gist권한 체크
    ```
    
    하지만 동일한 증상 발생
    

### 해결방안(SSH 사용)

- SSH를 이용해 git clone
    
    ```
    git clone git@gist.github.com:{gist_ID}.git
    
    git@gist.github.com: Permission denied (publickey).
    fatal: 리모트 저장소에서 읽을 수 없습니다
    올바른 접근 권한이 있는지, 그리고 저장소가 있는지
    확인하십시오.
    ```
    
    올바른 SSH 키가 등록되어 있지 않아 에러 발생
    
- SSH 키 생성 및 등록
    
    키 생성
    
    ```
    ssh-keygen -t rsa -C “{email 주소}"
    
    cat ~/.ssh/id_rsa.pub
    ```
    
    github에 생성한 키 등록
    
    ```
    Settings/SSH and GPG keys → NEW SSH key
    ```
    
    git의 모든 기능 정상적으로 작동
    

## JavaScript의 Static 메소드 문제

### 클래스 메소드 호출 문제

- 스태틱 메소드 호출 시 아래의 에러 발생
    
    ```
    Uncaught TypeError TypeError: this.handleParseException is not a function
    
    this.handleParseException(error);
    ```
    
    디버깅 결과 문제는 아래 부분에서 발생
    
    ```jsx
    static parseString(string) {		
    	~~
    	this.handleParseException(error);
    }
    ```
    
    `handleParseException` 의 경우 일반 메소드, `parseString` 은 스태틱 메소드
    

### 해결방안

- 스태틱 메소드 내부에서는 this 키워드 사용이 불가능
- 따라서 `handleParseException` 를 스태틱 메소드로 변경 후 생성자를 통해 접근
    
    ```jsx
    StringProcessor.handleParseException(error);
    ```
    
    이후 위와 같이 수정하여 해결함