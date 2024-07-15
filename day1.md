# 학습 정리

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
    

해결방안

- 스태틱 메소드 내부에서는 this 키워드 사용이 불가능
- 따라서 `handleParseException` 를 스태틱 메소드로 변경 후 생성자를 통해 접근
    
    ```jsx
    StringProcessor.handleParseException(error);
    ```
    
    이후 위와 같이 수정하여 해결함