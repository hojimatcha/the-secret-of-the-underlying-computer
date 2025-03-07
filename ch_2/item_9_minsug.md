# 2.9 컴퓨터 시스템 여행: 데이터, 코드, 콜백, 클로저에서 컨테이너, 가상 머신까지

## 2.9.1 코드, 데이터, 변수, 포인터

우리는 자주 사용하는 코드를 변수와 함수로 지정해서 사용합니다. 이렇게하면 DRY 법칙을 지키기도 쉽고 작업 피로도나 효율성 측면에서도 도움이 되기 때문이죠!

> 👀 DRY 법칙이란 <br>
> Don't repeat yourself의 축약어로 같은 코드를 반복해서 쓰지 말라는 뜻이다.
> 개인적인 견해지만 요즘 DRY를 꼭 지키는게 필요한지 의문이 든다.

변수와 함수로 지정된 코드는 메모리에 저장되는데 이를 참조할 수 있게 됩니다. 자바스크립트 세계에서는 이름 참조라 하지만 C++과 같은 세계에서는 포인터라고 합니다.

## 2.9.2 콜백 함수와 클로저

혹시 일급 객체라는 말을 들어보셨나요? 프로그래밍을 처음 공부할 때 매우 생소하게 들렸을 이 개념은 특정 코드가 할당, 매개변수로 전달 및 반환값으로 사용되는 객체를 지칭합니다.

예를 들어 자바스크립트에서는 함수는 일급 객체인데 이는 함수가 매개변수로 주어질 수도 있고 다른 함수의 반환 값이 될 수도 있다는 것을 의미합니다. 그리고 다른 함수의 매개변수로 주어질 때 해당 함수를 콜백 함수라고 말합니다.

콜백 함수는 책의 앞 부분에서 이야기한 것처럼 선언 환경과 실행 환경을 분리할 수 있기 때문에 라이브러리를 만들 때 잘 사용됩니다.

그런데 만약 선언된 환경에서의 변수를 참조해야하는 상황이 온다면 어떡할까요?

우리는 이 때 클로저라는 개념을 사용해서 해결할 수 있습니다. 클로저란 선언된 환경의 컨텍스트를 기억하는 것을 말합니다.
코드로 보면 다음과 같습니다.

```js
function foo() {
  const name = "minsug";

  return () => {
    console.log(name);
  };
}

const whatIsYourName = foo();

whatIsYourName(); // 출력 값: minsug
```

`foo`가 실행됐을 떄 사실상 name 변수는 메모리에서 삭제되어야 합니다. 그러나 반환값에서 계속 참조하고 있기 때문에 메모리에서 삭제되지않고 whatIsYourName이 실행될 떄마다 name을 참조해 값을 출력하는 것이지요.

# 2.9.3 컨테이너와 가상 머신 기술

우리는 방금 환경이 달라져도 메모리를 잃지 않는 법을 배웠습니다. 그러나 이는 매우 지엽적입니다. 그렇다면 프로세스 혹은 스레드의 범위에서도 메모리를 유지하는 법은 없을까요? 혹은 어떤 환경에서도 프로그램이 지니고 있는 고유한 특성을 유지하는 것은요?

우리는 이를 컨테이너라는 방식을 통해서 구현합니다. 일전에 책에서 언급한 것처럼 CPU의 언어 해석 방법이 모두 다르기 때문에 우리는 모두 같은 언어로 바꾸는 컴파일러를 사용했어야 합니다. 이처럼 사용자마다 실행 환경이 상이하기 때문에 우리가 작성한 프로그램이 의도치 않게 실행될 수도 있습니다.

그리고 이런 상이한 실행 환경을 프로그램이 의도한 환경으로 세팅하기 위해서 나타난 것이 컨테이너입니다. 컨테이너는 운영 체제와 관계없이 프로그램이 설계한 환경을 그대로 가져가서 그 내부에서 프로그램이 실행될 수 있게 합니다. 그래서 어떤 환경이든 개발자가 의도한대로 기능이 작동할 수 있게 합니다.

> 이 중 대표적인 프로그램이 도커입니다.

이런 일련의 과정을 **가상화**라고 하는데 이는 소프트웨어에만 국한된 것은 아닙니다. 하드웨어도 충분히 가상화할 수 있으며 이를 가상 머신 감시자라고 합니다. 가상 머신 감시자에서 실행되는 운영 체제는 하드웨어 리소스를 독점한다고 간주할 수도 있습니다.
