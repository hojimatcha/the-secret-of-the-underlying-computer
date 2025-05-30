# 4.8 CPU 진화론(하): 절체절명의 위기에서 반격

축소 명령어 집합을전면적으로 도입하게 되면 여태까지 팔린 복잡 명령어 집합이 도입된 CPU에서는 최신 프로그램이 사용될 수 없게됩니다.
이를 해결위해서 프로그래머는 CPU 인터페이스를 활용했습니다.

> 인터페이스란?
> 인터페이스라는 단어가 종종 나오곤 하는데 한번도 제대로 이해하본 적이 없는 것 같습니다.
> 그래서 차근차근 이해하면 좋을 것 같은데 먼저 함수 인터페이스의 개념부터 알아봅시다.
> 함수 인터페이스란 입출력을 말합니다. 예를 들어 A와 B라는 숫자 인자를 집어넣었을 때 C라는 숫자를 반환하면
> 저희는 이 함수의 인터페이스가 (A: number B: number) => C: number이란 걸 알 수 있습니다.
> 다시 말해서 내부 로직과 관계없이 인터페이스를 알 수 있다는 말이지요.

## 하이퍼 스레딩이란?

하이퍼스레딩은 인텔에서 개발한 CPU 성능 향상 기술 중 하나로, 하나의 물리적인 CPU 코어를 마치 두 개의 논리적인 코어처럼 동작하게 하는 기술입니다.
하이퍼 스레딩을 사용하면 하나의 물리 CPU는 운영 체제에 환각을 심어 마치 두 개의 CPU 코어가 있는 것처럼 보이기 합니다.

이 비밀은 하이퍼스레딩이 적용된 CPU가 한 번에 스레드 두 개에 속하는 명령어 흐름을 처리할 수 있는 능력에 있습니다.
