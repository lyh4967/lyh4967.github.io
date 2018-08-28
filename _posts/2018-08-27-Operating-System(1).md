---
title: "[운영체제] 1. 병행프로세스와 상호배제"
subtitle: "병행프로세스의 수행과 생호배제를 해결하는 방법을 알아본다. "
date: 2018-08-27 11:30:00
categories:
- OS
tags:
- OS
---
# 병행 프로세스
## 병행 프로세스의 개념
컴퓨터가 프로그램을 사용하는데 필요한 자원 중 메모리 같은 자원은 공유영역을 모든 프로세스가 동시에 공유한다. 즉, 병렬로 사용한다. 운영체제가 프로세서를 빠르게 전환하여 프로세서 시간을 나눠 마치 프로세스 여러개를 동시에 사용하는 것처럼 보이게 하는것을 병행프로세스라고한다.

## 병행 프로세스의 해결 과제
* 공유 자원을 상호베타적으로 사용해야함
* 병행 프로세스간에는 협력이나 동기화가 필요(상호배제)
* 두 프로세스 간에는 데이터를 교환할 수 있도록 통신
* 다른 프로세스의 속도와 관계없이 항상 일정한 싱행결과를 보장 determinacy를 보장
* 교착상태 해결, 병렬처리능력 극대화
* 실행 검증 문제 해결
* 상호배제를 보장해야한다.

## 선행 그래프와 병행 프로그램
### 선행 그래프
선행그래프는 선행제약을 논리적으로 표현한 것이다. 말그대로 어떤 연산을 하기 전 선행할 연산을 그래프의 형태로 나타낸다.
![graph](https://user-images.githubusercontent.com/33022147/44658640-79060f00-aa3c-11e8-856b-d24fcdfb424a.PNG)
이처럼 선행 그래프는 비순환 적이어야 한다.  그렇지 않다면 모순이 발생
### fork와 join구조
fork 명령어는 병행 프로세스를 2개 만든다.
```
	S1;
	fork L;
	S2;
	.
	.
L:S3;
```
S2 와 S3로 분할, 그 다음명령과 지정위치에서(L)에서 실행한다.<br/>
<br/>
join 명령어는 병행 프로세스를 결합한다.
위의 그림의 경우 알고리즘은 다음과 같다.
```
		count :=3;
		S1;
		fork L1;
		S2;
		S4;
		fork L2;
		S5;
		goto L3;
L2: S6;
L1: goto L3;
		S3;
L3: join count;
		S7;
```
### 병행 문장
하나의 프로세스가 여러 병렬 프로세스로 퍼졌다가 다시 하나로 뭉쳐지는 것을 나타내는 고급 언어구조. ex)parbegin S1;s2; ...Sn; parend;
<br/>
parbegin과 parend사이의 모든 문장은 병행 수행할 수 있다.
위의 그림을 병행제어 알고리즘으로 표현한다면<br/>
```
S1;
parbegin
	S3;
	begin
		S2;
		S4;
		parbegin
		S5;
		S6;
		parend
	end
parend
S7;
```
# 상호배제와 동기화
## 상호배제의 개념
병행 프로세스에서 프로세스 하나가 공유 자원을 사용할 때 다른 프로세스들이 동일한일을 할 수 없도록 하는 방법이다.
* 두 프로세스는 동시에 공유자원에 진입할 수 없다.
* 프로세스의 속도나 프로세스의 수에 영향을 받지 않는다.
* 공유 자원을 사용하는 프로세스만 다른프로세스를 차단할 수 있다.
* 프로세스가 공유자원을 사용하려고 너무오래 기다려서는 안된다.
## 임계 영역
```c
do{
	while(turn != i);//turn이 i가 아니면 대기
	//임계 영역
	turn = j;
	//나머지 영역
}while(true);
```
임계영역은 1.상호배제 2. 진행 3. 한정대기 를 만족해야한다.
## 생산자, 소비자문제와 생호배제를 해결하는 초기의 시도
생산자와 소비자의 속도차이를 해결하기위해 버퍼를 사용한다. 유한 버퍼의경우 포인터위치를 순환시킬 필요가 있다. 이때 순환큐와 같이 front와 end포인터를 두고 꽉찼는지 아닌지를 판별, 이때 생산자와 소비자가 동시에 연산을 한다면 counter에 예측 불가의 값이 들어갈 수 있기 때문에 상호배제가 중요하다. 이는 공유변수 counter과 counter을 연산하는 부분을 임계영역으로 설정해 상호배제 할 수 있다.
<br/>
공유데이터
```c++
#define BUFFER_SIZE 10
typedef struct{
	DATA data;
}item;
item buffer[BUFFER_SIZE];
int in=0;
int out=0;
int counter=0;
```
<br/>
생산자 프로세스
```c
item nextProduced;
while(true){
	//버퍼 가득찼는지 확인
	while(counter == BUFFER_SIZE);
	buffer[in]=nextProduced;
	in=(in+1)%BUFFER_SIZE;
}
```
<br/>
소비자 프로세스
```c
item nextConsumed;
while(true){
	//비어있는지 검사
	while(counter==0);
	nextConsumed=buffer[out];
	out=(out+1)%BUFFER_SIZE;
	counter--;
}
```

# 상호배제 방법

## 데커의 알고리즘

플래그를 사용해 진입허가를 결정한다. 소프트웨어로 해결

```c++
//프로세스가 공유하는 데이터 flag[],turn
flag[0]=false;
flag[1]=false;
turn=0; //공유변수 0 or 1

//프로세스 P0;
flag[0]=true;//P0의 임계영역 진입표시
while(flag[1]==true){//P1의 임계영역 진입여부 표시
	if(turn==1){//P1이 진입할 차례면
		flag[0]=false;//플래그를 재설정, P1에 진입양보
		while(turn==1){//turn을 바꿀때까지 대기
			//바쁜대기
		}
		flag[0]=true; //P0이 임계영역에 재진입
	}
}
//임계역역
turn=1;
flag[0]=false;
//나머지영역

//프로세스 P1
flag[1]=true;//P0의 임계영역 진입표시
while(flag[0]==true){//P1의 임계영역 진입여부 표시
	if(turn==0){//P1이 진입할 차례면
		flag[1]=false;//플래그를 재설정, P1에 진입양보
		while(turn==0){//turn을 바꿀때까지 대기
			//바쁜대기
		}
		flag[1]=true; //P0이 임계영역에 재진입
	}
}
//임계역역
turn=0;
flag[1]=false;
//나머지영역
```

## TestAndSet(테스)명령어
하드웨어에 사용하는 명령어로 알고리즘이 간단,하나의 메모리사이클에서 수행하기 때문에 동시연산이 불가(경쟁상황 해결)
```c++
bool TestAndSet(bool * target){
	bool temp=* target;
	* target=true;
	return temp;
}
/* 프로세스가 2개일때
boolean lock=false;
do{
	while(TestAndSet(&lock))//lock을 검사 true이면대기, false면 진입
	//임계영역
	lock=false;
}while(true);
*/
//프로세스가 여러개
bool waiting[n];
bool lock=false;
int j;
bool key;
do{  //프로세스 Pi의 진입영역
	waiting[i]=true;
	key=true;
	while(waiting[i]&&key)
		key=TestAndSet(&lock); //key가 true가 되어 다른 프로세스는 통과못함
	waiting[i]=false;
	//임계영역
	//탈출영역
	j=(i+1)%n;
	while((j!=i) && !waiting[j])//대기중인 프로세스 찾음
		j=(j+1)%n;
	if(j==i)// 대기중인 프로세스가 없으면
		lock=false; //다른 프로세스 진입
	else//대기 중이 있다면
		waiting[j]=false;//임계영역 진입가능하도록 함\
		//나머지 영역
}while(true);

```
사용자 수준에서 가능하지만 바쁜대기가 발생하면 기아, 교착상태가 발생한다.

## 세마포
무한 while문을 돌지않고 상태확인을 통해 프로세서 사이클낭비를 줄인다.

### 세마포 개념과 동작
세마포는 값이 음이 아닌 정수인 플래그 변수이다.<br/>
세마포 정의
```
P(S):wait(S){
	while S <=0
			;
	S--;
}

V(S):signal(S){
	S++;
}

do{
	wait(mutex);
	//임계영역
	signal(mutex);
	//쓰레기영역
}
```
### 세마포의 종류
* 계수세마포: 상호배제와 조건부 동기화 해결
* 이진세마포: 임계영역처럼 특별히 상호배제 해결

#### 이진세마포

세마포S를 1로 초기화하면 P연산을 실행, 0으로 초기화하면 v연산을 먼저실행한다.

#### 계수세마포

초기 count가 0이면 사용불가, 양수이면 사용가능, 이떄 사용가능 세마포수는 count와 동일하다.

### 세마포의 구현

```c++
struct semaphore{
	int count;
	queue que;
}; semaphore S;

//wait 연산
wait(S){
	S->count--;
	if(S->count<0){
		add this process to S->que;
		block();
	}
}

//signal 연산
signal(S){
	S->count++;
	if(S->count<=0){
		remove a process P from S->que;
		wakeup(P);
	}
}
```
세마포를 통해 신호를 줘 프로세스가 lock을 걸었을 경우, 준비큐에 프로세서를 넣고 대기상태에 들어가게 한다. 이는 무한정 while을 수행하는 바쁜대기 상태를 해소할 수 있다. 세마포에서는 wait나 signal연산을 생략하면 상호배제 문제가 발생할 수 있다.
## 모니터
세마포는 모든 프로세스가 1로초기화한 세마포를 공유, 프로세서가 임계영역에 진입시에 signal연산을 실행 후 wait를 해야한다. 그렇지 않다면 상호배제를 위반하거나 교착상태가 발생한다. 모니터는 이러한 단점을 극복하려한다.

### 모니터의 개념과 구조
임계영역을 관리하는 하나의 모듈로 내부에는 프로시저로만 프로세스들의 접근을 허용하며 통제한다. 이는 한순간에 하나의 프로세스만 모니터 안에서 실행하도록 보장할 수 있다.**프로세스가 프로시저 호출-> 호출프로세스를 모니터에서 실행** 이때 모니터에는 조건변수를 주어 통제의 강력함을 더할 수 있다.

### 모니터와 세마포 비교
1. P와 V연산은 계수세마포와 비슷하다. 하지만 계수 세마포의 경우 P연산은 계수 세마포가 0보다 클 수 있더 해당 프로세스를 반드시 차단하지 않는다. 반면 wait연산을 실행하면 항상 프로세스를 차단. V연산은 일단 counter을 증가시키고 작업의 유무를 확인, 하지만 모니터는 signal을 수행했을때 대기 중인 작업이 없으면 signal호출은 아무런 효과를 발생하지않는다.
2. 세마포는 V연산으로 사용자가 지연 없이 실행을 재개할 수 있다. 하지만 모니터는 signal연산으로 모니터의 잠금을 해제할때만 다시 시작한다.

### 모니터의 구현
* 1로 초기화된 세마포 mutex사용
* 프로세스는 진입전 P연산을 수행, 떠날때는 V연산 수행

```c++
//x.wait연산
x_count++;
if(next_count>0)
	signal(next);
else
	signal(nutex);
	wait(x_sem);
x_count--;

//x.signal연산
if(x_count>0){
	next_count++;
	signal(x_sem);
	wait(next);
	next_count--;
}
```

모니터 안에서 프로세스를 다시 시작할때
```c
monitor resource_allocator{
	condition is_free;
	int in_use=0;

	get_resource(){
		if(in_use)
			wait(is_free);//프로세스 중단(일시정지)
		in_use=1;
	}
	return_resource(){
		in_use=0;
		signal(is_free); //대기중인 프로세스에 할당 허용(신호)
	}
}
```
