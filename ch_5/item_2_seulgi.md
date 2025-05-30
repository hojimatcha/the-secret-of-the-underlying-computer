# 어떻게 캐시 친화적인 프로그램을 작성할까?

최신 컴퓨터 시스템에서는 CPU와 메모리 사이의 속도 차이를 보완하려고<br />
CPU가 메모리에 직접 접근하는 대신 CPU와 메모리 사이에 캐시를 추가했다.

일반적으로 캐시는 L1 - L3 세 계층으로 나뉘고,<br />
L1 -> L3으로 갈수록 용량은 커지지만 접근 속도는 느려진다.

캐시의 특성을 알고 어떻게 해야 이런 캐시 친화적인 프로그램을<br />
작성해 캐시 적중률을 높일지 알아보자!

<br />
<br />

## 1) 프로그램 지역성의 원칙

프로그램 지역성의 원칙의 본질은 프로그램이<br />
"매우 규칙적으로" 메모리에 접근한다는 것이다.

프로그램이 메모리 조각에 접근하고 나서 **이 조각을 여러 번<br />
참조하는 경우** 이를 일컬어 **시간적 지역성**이라고 한다.

### 시간적 지역성이 캐시 친화성이 높은 이유?

데이터가 캐시에 있는 한 메모리에 접근하지 않아도<br />
반복적으로 캐시의 적중이 가능하다는 단순한 이유 때문이다.

### 공간적 지역성?

프로그램이 메모리 조각을 참조할 때는 인접한 메모리도 같이<br />
참조할 수 있는데 이를 공간적 지역성이라고 한다.

일반적으로 요청한 메모리의 인접 데이터도 함께 캐시에 저장되므로<br />
프로그램이 인접 데이터에 접근할 때 캐시가 적중하게 된다.

<br />
<br />

## 2) 메모리 풀 사용

malloc을 사용해서 할당받으면 메모리 조각 N개가 힙 영역의<br />
이곳조곳에 흩어져 있을 가능성이 높아 공간적 지역성이 그다지 좋지 않다.

메모리 풀 기술은 커다란 메모리 조각을 미리 할당받으며,<br />
이후에는 메모리를 요청하거나 해제할 때 더이상 malloc를 거치지 않는다.<br />
(메모리 친화적)

메모리 풀을 초기화할 때 일반적으로 연속적인 메모리 공간을 할당받으며,<br />
우리가 사용하는 데이터 역시 이 연속적인 메모리 공간 내에서 요청된다.<br />
(매우 집중적으로 모여 있는 형태로의 접근이 가능함)

<br />
<br />

## 3 ~ 4) struct 구조체 재배치 / 핫 데이터와 콜드 데이터의 분리

연결 리스트에 특정 조건을 만족하는 노드가 있는지 판단하려고 할 때<br />
연결 리스트 구조체를 정의해보자.

next 포인터와 value값을 함께 배치하게 되면 서로 인접해 있기 때문에<br />
캐시에 next 포인터가 있다면 매우 높은 확률로 value값이 있을 수 있어<br />
공간적 지역성의 원리로 구조체의 형태를 최적화할 수 있다.

프로그래머는 캐시 용량이 제한되어 있다는 점을 반드시 인식해야 하는데,<br />
구조체의 배열 arr는 콜드 데이터이며, next포인터 + value같이 빈번하게 접근하는 데이터인<br />
핫 데이터를 서로 분리하면 더 나은 지역성을 얻을 수 있다.

<br />
<br />

## 5 ~ 6) 캐시 친화적인 데이터 구조 / 다차원 배열 순회

지역성 원칙 관점에서는 배열이 연결 리스트보다 나은데,<br />
배열은 하나의 연속된 메모리 공간에 할당되지만 연결리스트는 일반적으로<br />
이곳저곳에 흩어져 있을 수 있기 때문이다.<br />

다만, 노드의 빈번한 추가/삭제에서는 연결 리스트가 배열보다 우수하다.

최적화를 할 때는 반드시 일종의 분석 도구를 사용하여 캐시의 적중률이<br />
시스템의 성능 병목이 되는지 판단하고, 병목이 되지 않는다면 굳이 최적화를 할 필요가 없다.

프로그램 지역성이 우수하면 할수록 최신 CPU의 캐시를 최대한 활용할 수 있다.

<br />
<br />
