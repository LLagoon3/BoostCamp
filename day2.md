# Day2 학습 정리

# Crontab

crontab 작성

```bash
crontab -e
```

crontab 내용 확인

```bash
crontab -l
```

주기

```bash
*　　　　　   *　　　　　 　  *　　　　   *　　　　　　  *
분(0-59)　　시간(0-23)　　일(1-31)　　월(1-12)　　　요일(0-7)
```

ex)

```bash
# 매주 금요일 오전 5시 45분에 test.sh 를 실행
45 5 * * 5 /home/script/test.sh

# 매일 1시 0분부터 30분까지 매분 tesh.sh 를 실행
0-30 1 * * * /home/script/test.sh

# 매 10분마다 test.sh 를 실행
*/10 * * * * /home/script/test.sh
```

# Stress Tool

### 설치

```bash
apt-get install stress
```

### **CPU 사용**

```bash
stress -c <코어수>
```

### **Memory 사용**

```bash
stress --vm <프로세스 개수> --vm-bytes<사용할 크기>
```

### 동시에 사용

```bash
stress -c 4 --vm 3 --vm-bytes 2048m --timeout 60s
```

# 쉘 스크립트 문법

쉘 스크립트 파일의 가장 첫 라인은 `#!/bin/bash`로 시작

코드를 작성한 후에는 실행 권한 필요

`'파일이름.sh'` 형태

주석은 `#내용`

### 출력 : `echo`

- echo "출력 내용”

### 변수

- 선언 : 변수명=데이터
- 사용 : $변수명
- ex)
    
    ```bash
    echo $변수명1 $변수명2 …
    ```
    

### 리스트

- 선언 : 변수명=(데이터1데이터2데이터3...)
- ex)
    
    ```bash
    daemons=("httpd" "mysqld" "vsftpd")
    ```
    
- 사용 : ${변수명[인덱스번호]}

### 연산자 : `expr`

- 역 작은 따옴표 (`) 사용
- 연산자 * 와 괄호 ( ) 앞에는 역 슬래시를 같이 사용
- 연산자와 숫자, 변수, 기호 사이에는 space
- ex)
    
    ```bash
    num=`expr \( 3 \* 5 \) / 4 + 7`
    ```
    

### 조건문

```bash
if [ 조건 ] 
then
     명령문
fi
```

```bash
if [ 조건 ]; then 명령문; fi
```

- if [ 뒤와 ] 앞에는 반드시 공백필요
- &&, ||, <, > 연산자들이 에러가 나는 경우는 [ [ ] ] 를 사용
- 조건문 인자
    - **`-eq` (equal to)**: 두 숫자가 같은지 비교합니다. 즉, 왼쪽과 오른쪽의 값이 같은 경우 참을 반환
    - **`-gt` (greater than)**: 왼쪽 숫자와 오른쪽 숫자 비교 → 왼쪽 값이 오른쪽 값보다 큰 경우 참을 반환

### 무한 반복문

```bash
while :; do
	~~
done
```

### `awk`

- 문자열의 숫자 연산을 위해 사용

```bash
$(awk 'BEGIN { if ('"$memory_usage"' > 50) print 1; else print 0 }')
```

- `BEGIN` 내부의 수식이 먼저 실행되고, 이 결과가 문자열로 반환되게 됨
- 여기서 `memory_usage` 는 숫자 문자열

```bash
[문자열] | awk '{print $1}'
```

- 위의 경우 `[문자열]` 을 공백단위로 split 한 뒤 1 ~ n 까지의 인덱스를 붙임
- `{print $1}` 과 같이 가져올 수 있음

# TroubleShooting

### 패키지 관리자 문제

- 패키지 관리자가 다른 곳에서 사용 중인 문제 발생
    
    ```bash
    Waiting for cache lock: Could not get lock /var/lib/dpkg/lock-frontend. It is held by process 18906 (unattended-upgr)
    ```
    
- 프로세스 종료 시도
    
    ```bash
    ps -ef | grep dkpg
    ```
    
    패키지 관리자 프로세스를 확인 → 하지만 프로세스 ID가 계속 변경되어 kill이 불가능
    
- 해결방안
    
    재부팅 이후 정상 작동