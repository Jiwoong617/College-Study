# 10장 Bash 쉘 스크립트

**쉘 스크립트** : 명령어 및 유틸리티들을 적절히 사용하여 작성한 쉘에서 동작하는 프로그램

<br>

### Bash 쉘 소개

- 리눅스, 맥 OS X 등의 운영체제의 기본 쉘
- Bash 문법은 본 쉘 문법을 대부분 수용하면서 확장
- . 으로 시작하는 숨겨진 파일 → ~/.bashrc, ~/.bash.profile
    - Bash 시작 과정 : /etc/profile → ~/.bash_profile → ~/.bashrc → 로그인 쉘 프롬포트
    
<br>

### 별명 및 히스토리 기능

- **$ alias 이름 = 문자열** : 문자열의 기존 명령에 대해 새로운 이름을 별명으로 정의
- **$ unalias 이름** : 이미 정의된 별명 해제
- **$ history [-rh] [번호**] : 지금까지 입력된 명령들 리스트
- **재실행**
    - $ !! : 바로 전 명령 재실행
    - $ !n : 이벤트 번호가 n인 명령 재실행
    - $ !시작스트링 : 시작스트링으로 시작하는 최후 명령 재실행
    - $ !? 서브스트링 : 서브스트링을 포함하는 최후 명령 재실행

<br>

### 변수

데이터를 저장하는 장소, 모든 변수가 문자열 타입

**단순 변수 -** 하나의 값만을 저장 할 수 있는 변수

- $ 이름 = 문자열

**리스트 변수** - 여러개의 값을 저장할 수 있는 변수

- $ 이름 = ( 문자열 리스트 )
    - $ {name[i]} : 리스트 변수 name의 i번째 원소
    - $ {name[*]} : 리스트 변수 name의 모든 원소 (* 말고 @도 됨)
    - $ {#name[*]} : 리스트 변수 name 내의 원소 개수 (* 말고 @도 됨)

<br>

### 쉘 변수 - 지역변수와 환경변수

쉘은 필요할 때마다 자식 쉘을 생성한다. 스크립트 수행, 후면 작업 실행, 명령어 실행 시 자식 쉘을 생성하여 자식 쉘로 하여금 해당 명령어를 실행하게 함.

![Untitled](https://user-images.githubusercontent.com/101644572/174438348-65d19957-f0b6-45a7-8339-6c86649b4e06.png)


- 환경 변수는 값이 자식 프로세스에게 상속 / 지역 변수는 상속 불가
- 사용자가 쉘 변수를 생성하면 기본적으로 모두 지역변수
- $ export 변수 : 지역 변수를 환경 변수로 만들어줌

<br>

**사전 정의 환경변수 :** 의미가 미리 지정된 환경변수들

![Untitled 1](https://user-images.githubusercontent.com/101644572/174438352-559f664f-bc26-413d-9570-99fd117171db.png)

<br>

**사전 정의 지역변수**

![Untitled 2](https://user-images.githubusercontent.com/101644572/174438354-b19057e6-9ef6-4512-a278-d46e6d0e2d85.png)

<br>

## Bash 쉘 스크립트

- 쉘 스크립트는 명령어 및 유틸리티들을 적절히 사용하여 작성한 프로그램
- 조건, 스위지, 반복 등을 위한 제어구조 if, case, for ,while 등의 문장 제공
- 비교 연산, 파일 관련 연산, 산술 연산 등 가능
- 첫 줄에 사용할 쉘 지정 : # !/bin/bash
- 부울 연산자 : &&,  ||,  ! 가능
- $ **read 변수** : 변수의 값을 키보드로 읽어 들임
- $ **let 변수 = 수식 :** 수식 값을 계산하고 변수에 저장
- **$#** : 명령줄 인수의 개수, **$변수** : 변수의 값 의미
- **조건식의 형태**는 **[ 조건식 ]** 또는 **(( 조건식 ))** 형태로 해야됨
- 리커전으로 자기 자신도 호출 가능

<br>

### 변수 타입 선언

![Untitled 3](https://user-images.githubusercontent.com/101644572/174438368-bf4966bb-72b9-487f-a130-f3b058c0fd21.png)

<br>

### 비교 연산

![Untitled 4](https://user-images.githubusercontent.com/101644572/174438372-a2881c46-ad77-40f1-936c-643dcd3e2024.png)

<br>

### 문자열 비교 연산

![Untitled 5](https://user-images.githubusercontent.com/101644572/174438379-d4674d17-caa0-4a35-8427-f66ab5db8d6e.png)

<br>

### 파일 관련 연산

![Untitled 6](https://user-images.githubusercontent.com/101644572/174438381-a7f6f969-4efd-45ed-95c0-686b02bf4ebc.png)

<br>

### 산술 연산자

![Untitled 7](https://user-images.githubusercontent.com/101644572/174438387-df8d0e30-90ae-45e7-8332-dae12874aeed.png)

<br>

### 조건문

**if 문**

```bash
if 조건식
then 
	명령어 리스트
fi
```

```bash
if 조건식
then
	명령어 리스트
elif 조건식
then
	명령어 리스트
else
	명령어 리스트
fi
```

ex)

```bash
#!/bin/bash

echo -n '점수 입력: '
read score
if (( $score >= 90 ))
then
	echo A
elif (( $score >= 80 ))
then
	echo B
elif (( $score >= 70 ))
then
	echo C
else
	echo 노력 요함
fi
```

<br>

**case 문**

```bash
case $변수 in
	패턴1) 명령어 리스트;;
	패턴2) 명령어 리스트;;
	*) 명령어 리스트;;
esac
```

ex)

```bash
#!/bin/bash

echo -n '점수 입력: '
read score
let grade=$score/10
case $grade in
	"10" | "9") echo A;;
	"8") echo B;;
	"7") echo C;;
	*) echo 노력 요함;;
esac
```

<br>

### 반복문

**for 문**

- 단어 리스트에 $* 이 들어가면 - 모든 명령줄 인수 처리
- for file in * : 디렉터리 내의 모든 파일 처리

```bash
for 이름 in 단어리스트
do
	명령어 리스트
done
```

ex)

```bash
#!/bin/bash

if [ $# -eq 0 ]
then
	echo 사용법: $0 파일*
	exit 1
fi
	echo " 사용권한 파일"

for file in $*
do
	if [ -f $file ]
	then
		fileinfo=`ls -l $file`
		perm=`echo "$fileinfo"|cut -d' ' -f1`
		echo "$perm $file"
	fi
done
```

ex) for file in *

```bash
#!/bin/bash

if [ $# -eq 0 ]
then
	dir="."
else
	dir=$1
fi

if [ ! -d $dir ]
then
	echo $0\: $dir 디렉터리 아님
	exit 1
fi

let fcount=0
let dcount=0
let others=0
echo $dir\:
cd $dir
for file in *
do
	if [ -f $file ]
	then
		let fcount++
	elif [ -d $file ]
	then
		let dcount++
	else
		let others++
	fi
done
echo 파일: $fcount 디렉터리: $dcount 기타: $others
```

<br>

**while 문**

```bash
while 조건식
do
	명령어리스트
done
```

ex)

```bash
#!/bin/bash

echo 명령어 메뉴
cat << MENU
	d : 날짜 시간
	l : 현재 디렉터리 내용
	w : 사용자 보기
	q : 끝냄
MENU

stop=0
while (($stop == 0))
do
	echo -n '? ‘
	read reply
	case $reply in
		"d") date;;
		"l") ls;;
		"w") who;;
		"q") stop=1;;
		*) echo 잘못된 선택;;
	esac
done
```

<br>

### 함수 정의

**함수 정의**

```bash
함수이름()
{
	명령어리스트
}
```

<br>

**함수 호출**

```bash
함수이름 [매개변수]
```

ex)

```bash
#!/bin/bash

lshead() {
	echo "함수 시작, 매개변수 $1"
	date
	echo "디렉터리 $1 내의 처음 3개 파일만 리스트"
	ls -l $1 | head -4
}

echo "안녕하세요"
lshead /tmp
exit 0
```
