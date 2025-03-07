# 1. 여러분이 프로그래밍 언어를 발명한다면?

간단한 스위치를 조합하면 복잡한 불 논리를 표현할 수 있다는 사실을 발견하고 이를 기반으로 개발된 것이 CPU.<br />
CPU는 간단한 개폐(on/off)만 이해할 수 있으며 이를 숫자로 표현하면 0과 1이 된다.

<br />

## 1.1.1 창세기 : CPU는 똑똑한 바보

CPU는 고난이도 동작을 하지 않지만 가장 큰 장점이 있다. 엄청나게 빠르다는 것.
<br />아무리 똑똑한 인간도 연산 속도로는 CPU를 이기지 못한다.

최초에 프로그래머가 CPU와 의사소통하기 위해서는 CPU 맞춤 언어를 사용해야 했었는데<br />
이 CPU 맞춤으로 이야기하기 위해 "천공 카드(punched card)"를 이용해 컴퓨터 작업을 제어했다.

> CPU 의지에 따라 0과 1로 구성된 명령어를 직접 작성한 것! <br />
> 이게 바로 코드(code)이고, 소스(source).

<br />

## 1.1.2 어셈블리어 등장

CPU는 가산 명령어, 점프 명령어 등 몇 가지 명령어만 실행할 수 있다는 사실이 있었고,<br />
따라서 기계어와 해당 특정 작업을 간단하게 대응시킬 수 있게 했다.

기계어를 인간이 읽고 이해할 수 있는 단어와 대응시켰는데(예를 들어 add, sub, mov 등의 단어)<br />
이를 위해 **인간이 인식할 수 있는 기계 명령어를 CPU가 인식할 수 있는<br />
0과 1로 구성된 바이너리로 변환하는 프로그램을 사용**한다.

이게 바로 어셈블리어(assembly language)!<br />
인간이 직접 인식할 수 있는 프로그래밍 언어가 탄생한 것이다.

<br />

## 1.1.3 저수준 계층의 세부 사항 대 고소준 계층의 추상화

사실 이 어셈블리어 자체도 여전히 저수준 언어이기 때문에<br />
프로그래머는 여전히 모든 세부 사항에 대해 신경을 써야 한다.

(CPU는 매우 원시적이라 데이터를 옮기고 간단히 연산하고 옮기는 작업밖에 하지 못한다.)

### "저에게 물 한잔 주세요" 라는 상황을 어셈블리어 같은 저수준 언어로 표현하려면?

오른쪽 다리를 내딛는다 -> 멈춘다 -> 왼쪽 다리를 내딛는다와 같이 모든 상황들을 CPU에게 설명해야 할 것이다.<br />
결국 더 인간과 기계의 거리를 좁힐 수 있는 방법이 필요해지고,

> 인간의 추상적인 표현을 CPU가 이해할 수 있는 구체적인 표현으로<br /> 자동으로 변환할 수 있는 방법이 있다면 프로그래머의 생산성이 높아질 것!

<br />

## 1.1.4 가득한 규칙: 고급 프로그래밍 언어의 시작

추상적인 표현을 CPU가 이해할 수 있게 하려는 노력 중에 세부 사항이 규칙 또는 패턴으로 가득하며<br />
CPU가 실행하는 명령어는 대부분 단도진입적이기 때문에 이 **단도진입적인 명령어에 문(statement) 또는 문장이라는 이름을 붙였다.**

### 1) 특정 상황에 따라 어떤 명령어를 실행하려면?

특정 상황에 따라 어떤 명령어를 실행할 지 결정해야 하는 선택이 필요하고 이 규정을 인간의 방식으로 표현하면 "만약 ~라면 ~하고, 그렇지 않으면 ~한다"가 된다.

```javascript
if ("조건") {
  ("명령어 내용");
} else {
  ("명령어 내용");
}
```

### 2) 일정한 명령어를 계속 반복하려면?

```javascript
while ("조건") {
  ("명령어 내용");
}
```

### 3) 거의 같은 명령어가 많을 땐?

개별적인 세부 사항만 다소 차이가 있을 뿐 계속 반복되며 이런 차이를 **매개변수(parameter)** 라고 한다.<br />
이를 별도로 분리하고 <u>매개변수를 제외한 나머지 명령어를 모아 하나의 코드로 지정하면 함수가 탄생</u>한다!

```javascript
function abc() {
  "명령어 내용";
}
```

이런 코드처럼 인간이 인식할 수 있는 문자열을 어떻게 CPU가 인식할 수 있는 기계 명령어로 변환할 수 있을까?

<br />

## 1.1.5 <인셉션>과 재귀: 코드 본질

명령어 내용들도 간단하지 않을 수 있다.<br />
문장이 될 수도 있고, 조건에 따른 이동일 수도 있으며(if else 등)<br />
순환(while) 혹은 함수 호출이 될 수 있다.

하나의 꿈속에 또 다른 꿈이 있고 또 그 꿈속에 다른 꿈이 반복되는 영화 인셉션처럼<br />
단계 안에 단계과 포함되어 있고 자식과 손자 관계과 끝없이 이어진다.

### 이 문제를 해결하려면?

```javascript
f(x) = f(x-1) + f(x-2)
```

해당 수열식은 `f(x)`값은 `f(x-1)`값에 의존하고, `f(x-1)` 값은 `f(x-2)`와 `f(x-3)`에 의존하며<br />
`f(x-2)`값은 `f(x-3)`, `f(x-4)`에 의존하는 것처럼 수열이 자식 수열에 의존하는 것을 표현한다.

단계 안에 단계과 중첩되는 것.<br />
끝없이 중첩된 것처럼 보이는 것도 **재귀** 로 표현 가능하다.<br />

세상의 모든 코드는 아무리 복잡하더라도 모두 구문으로 귀결되는데
이것이 가능한 이유가 <u>모든 코드가 구문에 기초하여 작성되기 때문</u>이다.

<br />

## 1.1.6 컴퓨터가 재귀를 이해하도록 만들기

프로그래밍 언어를 컴퓨터가 인식할 수 있는 기계 명령어로 변환하려면
단계 안에 단계가 중첩되는 <u>재귀 구문에 따라 작성된 코드를 트리(tree) 구조로 표현</u>할 수 있다.(구문 트리, syntax tree)

<br />

## 1.1.7 우수한 번역가: 컴파일러

컴퓨터는 프로그래밍 언어를 처리할 때 구문 정의에 따라 트리 형태로 코드를 구상할 수 있다.

이것이 구문 트리!

### 구문 트리가 적용하는 방법?

리프 노드를 기계 명령어로 번역하면 그 결과를 리프 노드의 부모 노드에 적용할 수 있다.

이렇게 번역 결과를 차례대로 부모 노드에 적용하는 방식을 보면 결국 전체 트리를 구체적인 기계 명령어로 번역할 수 있다.

이 작업을 담당하는 프로그램이 바로 "컴파일러(compiler)".<br />
컴파일러를 통해 고급 프로그래밍 언어를 발명하고, <u>인간이 인식할 수 있는 언어로 코드를 작성할 수 있고 컴파일러는 이를 CPU가 인식하는 기계 명령어로 번역하는 역할</u>을 한다.

<br />

## 1.1.8 해석형 언어의 탄생

형식이 다른 CPU에서는 다른 형식에서 생성된 기계 명령어를 CPU에서 실행할 수 없었다.<br />
(각각의 고유한 언어를 사용하고 있었기 때문)

### 이런 문제는 어떻게 해결했을까?

영어를 우리가 국제 통영어로 사용하는 것처럼 CPU에는 각각의 유형이 있어 바꿀 수 없지만<br />
CPU는 기계 명령어를 실행하는 존재기 때문에

직접 표준 명령어 집합을 정의해 CPU 기계 명령어 실행 과정을 모방하는 프로그램을 작성해 사용할 수 있다.

> 한 번의 코드 작성으로 어디에서나 그 코드를 실행하는 방법으로<br /> CPU 시뮬레이션 프로그램인 가상 머신(virtual machine)이자 인터프리터(interpreter)을 활용하는 것!

<br />

## 요약

세상의 모든 프로그래밍 언어는 특정 구문에 따라 작성된다.<br />

컴파일러는 <u>언어 구문에 따라 코드 구문을 분석하여 구문 트리</u>로 만들고,<br />
이 구문 트리를 C/C++언어처럼 <u>기계 명령어로 번역해 CPU로 직접</u> 넘기거나<br /> 자바처럼 <u>바이트 코드(byte code)로 변환 후 가상 머신</u>으로 넘겨 실행한다.

고급 언어는 추상적인 표현이 뛰어나 사용이 쉽지만
저수준 계층에 대한 제어 능력이 떨어져<br />
직접 저수준 계층의 세부 사항을 제어할 수 있어야 하는 운영 체제 중 일부분은 어셈블리어로 작성된다.

<br />
