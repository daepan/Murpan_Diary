## 컴퓨터의 구성

하드웨어 + 소프트웨어로 구성되어 있다

하드웨어의 종류로는 CPU, RAM HDD 마우스 프린터
소프트웨어로는 운영체제 컴파일러 워드프로세서 등등이 있다.

하드웨어는 중앙처리장치(CPU), 기억장치, 입출력장치로 구성되어 있다.

이들은 시스템 버스로 연결되어 있으며, 시스템 버스는 데이터와 명령 제어 신호를 각 장치로 실어나르는 역할을 한다.

  

### 중앙처리장치(CPU)

인간으로 따지면 두뇌에 해당하는 부분

주기억장치에서 프로그램 명령어와 데이터를 읽어와 처리하고 명령어의 수행 순서를 제어함 중앙처리장치는 비교와 연산을 담당하는 산술논리연산장치(ALU)와 명령어의 해석과 실행을 담당하는 제어장치, 속도가 빠른 데이터 기억장소인 **레지스터**로 구성되어있음

개인용 컴퓨터와 같은 소형 컴퓨터에서는 CPU를 마이크로프로세서라고도 부름

  

### 기억장치

프로그램, 데이터, 연산의 중간 결과를 저장하는 장치

주기억장치와 보조기억장치로 나누어지며, RAM과 ROM도 이곳에 해당함. 실행중인 프로그램과 같은 프로그램에 필요한 데이터를 일시적으로 저장한다.

보조기억장치는 하드디스크 등을 말하며, 주기억장치에 비해 속도는 느리지만 많은 자료를 영구적으로 보관할 수 있는 장점이 있다.

  

### 입출력장치

입력과 출력 장치로 나누어짐.

입력 장치는 컴퓨터 내부로 자료를 입력하는 장치 (키보드, 마우스 등)

출력 장치는 컴퓨터에서 외부로 표현하는 장치 (프린터, 모니터, 스피커 등)

  
  

### 시스템 버스

> 하드웨어 구성 요소를 물리적으로 연결하는 선

각 구성요소가 다른 구성요소로 데이터를 보낼 수 있도록 통로가 되어줌

용도에 따라 데이터 버스, 주소 버스, 제어 버스로 나누어짐

  

### 데이터 버스

중앙처리장치와 기타 장치 사이에서 데이터를 전달하는 통로

기억장치와 입출력장치의 명령어와 데이터를 중앙처리장치로 보내거나, 중앙처리장치의 연산 결과를 기억장치와 입출력장치로 보내는 '양방향' 버스임

### 주소 버스

데이터를 정확히 실어나르기 위해서는 기억장치 '주소'를 정해주어야 함.
주소버스는 중앙처리장치가 주기억장치나 입출력장치로 기억장치 주소를 전달하는 통로이기 때문에 '단방향' 버스임

### 제어 버스

주소 버스와 데이터 버스는 모든 장치에 공유되기 때문에 이를 제어할 수단이 필요함
제어 버스는 중앙처리장치가 기억장치나 입출력장치에 제어 신호를 전달하는 통로임
제어 신호 종류 : 기억장치 읽기 및 쓰기, 버스 요청 및 승인, 인터럽트 요청 및 승인, 클락, 리셋 등
제어 버스는 읽기 동작과 쓰기 동작을 모두 수행하기 때문에 '양방향' 버스임

  

컴퓨터는 기본적으로 **읽고 처리한 뒤 저장**하는 과정으로 이루어짐

(READ → PROCESS → WRITE)

이 과정을 진행하면서 끊임없이 주기억장치(RAM)과 소통한다. 이때 운영체제가 64bit라면, CPU는 RAM으로부터 데이터를 한번에 64비트씩 읽어온다.

## 중앙 처리 장치의 작동원리

#### 역할

- 컴퓨터에서 가장 핵심적인 역할을 수행하며, '인간의 두뇌'에 해당합니다.
- 크게 연산장치, 제어장치, 레지스터 3가지로 구성됩니다.

#### 구성 요소

1. **연산장치 (ALU: Arithmetic Logic Unit)**
    
    - 산술연산과 논리연산을 수행합니다.
    - 연산에 필요한 데이터를 레지스터에서 가져오고, 연산 결과를 다시 레지스터로 보냅니다.
2. **제어장치 (Control Unit)**
    
    - 명령어를 순서대로 실행할 수 있도록 제어합니다.
    - 주기억장치에서 프로그램 명령어를 꺼내 해독하고, 그 결과에 따라 명령어 실행에 필요한 제어 신호를 기억장치, 연산장치, 입출력장치로 보냅니다.
    - 각 장치가 보낸 신호를 받아, 다음에 수행할 동작을 결정합니다.
3. **레지스터 (Register)**
    
    - 고속 기억장치로, 명령어 주소, 코드, 연산에 필요한 데이터, 연산 결과 등을 임시로 저장합니다.
    - 용도에 따라 범용 레지스터와 특수목적 레지스터로 구분됩니다.
        - **범용 레지스터**: 연산에 필요한 데이터나 연산 결과를 임시로 저장합니다.
        - **특수목적 레지스터**: 특정 용도로 사용하는 레지스터로, 다음과 같은 주요 레지스터가 있습니다:
            - **MAR (Memory Address Register)**: 읽기와 쓰기 연산을 수행할 주기억장치 주소를 저장합니다.
            - **PC (Program Counter)**: 다음에 수행할 명령어 주소를 저장합니다.
            - **IR (Instruction Register)**: 현재 실행 중인 명령어를 저장합니다.
            - **MBR (Memory Buffer Register)**: 주기억장치에서 읽어온 데이터 또는 저장할 데이터를 임시로 저장합니다.
            - **AC (Accumulator)**: 연산 결과를 임시로 저장합니다.

### CPU의 동작 과정

1. **명령어 인출**
    
    - 주기억장치는 입력장치에서 입력받은 데이터 또는 보조기억장치에 저장된 프로그램을 읽어옵니다.
    - CPU는 프로그램을 실행하기 위해 주기억장치에 저장된 프로그램 명령어와 데이터를 읽어와 처리하고, 결과를 다시 주기억장치에 저장합니다.
    - 제어장치는 명령어가 순서대로 실행되도록 각 장치를 제어합니다.
2. **명령어 사이클**
    
    - CPU가 주기억장치에서 명령어를 순차적으로 인출하여 해독하고 실행하는 과정을 반복합니다.
    - 명령어 사이클은 인출, 실행, 간접, 인터럽트 사이클로 나누어집니다.
        - **인출 사이클**: 주기억장치에서 명령어를 가져오는 과정
        - **실행 사이클**: 명령어를 실행하는 과정

### 명령어 처리 과정

#### 인출 사이클

1. **PC 값을 MAR로 전달**:
    - `T0 : MAR ← PC`
2. **명령어 인출 및 PC 값 증가**:
    - `T1 : MBR ← M[MAR], PC ← PC + 1`
3. **IR로 명령어 전달**:
    - `T2 : IR ← MBR`

#### 실행 사이클 (예: ADD addr 명령어)

1. **주소를 MAR로 전달**:
    - `T0 : MAR ← IR(Addr)`
2. **데이터 인출**:
    - `T1 : MBR ← M[MAR]`
3. **연산 수행**:
    - `T2 : AC ← AC + MBR`

### 명령어 세트

- **명령어 세트**: CPU가 실행할 명령어의 집합으로, 연산 코드와 피연산자로 이루어집니다.
    - **연산 코드 (Opcode)**: 실행할 연산을 정의합니다.
    - **피연산자 (Operand)**: 필요한 데이터 또는 저장 위치를 정의합니다.


## 캐시 메모리

- **캐시 메모리**는 속도가 빠른 장치와 느린 장치 사이의 병목 현상을 줄이기 위한 메모리입니다.
- 예시:
    - CPU 코어와 메모리 사이의 병목 현상 완화
    - 웹 브라우저 캐시 파일은, 하드디스크와 웹페이지 사이의 병목 현상을 완화

#### 기능

- **CPU가 주기억장치에서 데이터를 읽어올 때**, 자주 사용하는 데이터를 캐시 메모리에 저장한 뒤, 다음에 이용할 때 주기억장치가 아닌 캐시 메모리에서 먼저 가져오면서 속도를 향상시킵니다.
- 캐시 메모리는 속도가 빠르지만, 용량이 적고 비용이 비쌉니다.

#### 캐시 메모리의 종류

- **L1 캐시**: CPU 내부에 존재, 가장 빠른 접근 속도를 가집니다.
- **L2 캐시**: CPU와 RAM 사이에 존재, L1 캐시 다음으로 빠릅니다.
- **L3 캐시**: 보통 메인보드에 존재, 가장 느리지만 여전히 빠른 접근 속도를 가집니다.

#### 캐시 메모리의 작동 원리

- **시간 지역성**: 한번 참조된 데이터는 잠시 후 다시 참조될 가능성이 높습니다.
- **공간 지역성**: 연속된 메모리 위치에 있는 데이터가 잠시 후 다시 사용될 가능성이 높습니다.
- 캐시는 데이터와 함께 인접한 데이터도 저장하여 참조 지역성을 최대한 활용합니다.

#### 캐시 히트와 캐시 미스

- **캐시 히트 (Cache Hit)**: CPU가 요청한 데이터가 캐시에 있는 경우
- **캐시 미스 (Cache Miss)**: CPU가 요청한 데이터가 캐시에 없는 경우

#### 캐시 미스의 종류

1. **Cold Miss**: 해당 메모리 주소를 처음 불러서 발생하는 미스
2. **Conflict Miss**: 두 데이터가 같은 캐시 주소에 할당되어 발생하는 미스 (direct mapped cache에서 발생)
3. **Capacity Miss**: 캐시 메모리의 공간이 부족해서 발생하는 미스

### 캐시 메모리의 구조 및 작동 방식

#### Direct Mapped Cache

- DRAM의 여러 주소가 캐시 메모리의 한 주소에 대응되는 다대일 방식
- 인덱스 필드 + 태그 필드 + 데이터 필드로 구성
- 간단하고 빠르지만 Conflict Miss가 발생할 수 있습니다.

#### Fully Associative Cache

- 비어있는 캐시 메모리에 자유롭게 주소를 저장하는 방식
- 저장은 간단하지만 검색이 복잡하여 CAM이라는 특수 메모리 구조를 사용해야 합니다.

#### Set Associative Cache
* Direct Mapped Cache와 Fully Associative Cache의 장점을 결합한 방식입니다.
* 특정 행(Set)을 지정하고, 그 행 내의 비어있는 열(Way)에 데이터를 저장하여 효율성을 높입니다. Conflict Miss를 줄이면서도 비교적 빠른 검색을 가능하게 하여 캐시 메모리의 성능을 향상시킵니다.


## 고정 소수점과 부동 소수점

컴퓨터에서 실수를 표현하는 방법은 `고정 소수점`과 `부동 소수점` 두가지 방식이 존재한다.