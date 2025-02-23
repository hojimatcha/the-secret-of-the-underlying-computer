## 2.4 프로그래머는 코루틴을 어떻게 이해해야 할까?

병렬성과 동시성에 대해 이야기를 많이 들어봤지만, 코루틴이라는 개념에 대해서는 생소하게 느껴졌습니다. <br>
이번 장을 통해 코루틴이라는 새로운 개념에 대해 배우고, 이것이 우리가 항상 접하는 Promise 의 개념과 매우 밀접하며, 자바스크립트의 generator function으로 코루틴을 구현할 수 있음을 알 수 있었습니다.

- **코루틴(coroutine)이란?** <br>
코루틴은 높은 성능과 동시성을 요구하는 분야에서 자주 등장하는 개념으로, 프로그램의 실행 흐름을 중단하고 재개할 수 있는 함수입니다.

### 2.4.1 일반 함수 & 2.4.2 일반함수에서 코루틴으로

**스레드와 코루틴** <br>
코루틴은 스레드와 유사하게 일시중지와 재개기능이 있습니다.

- 스레드
  - 운영체제 커널(kernel)이 관리하는 실행 단위
  - 프로세스가 여러 개의 스레드를 생성하고 스레드의 일시중지와 재개가 가능함
  - 운영체제가 스케줄링
  - 문맥 전환 시 CPU에서 캐시 메모리를 비우고 레지스터 값을 저장하는 등 오버헤드가 발생
  - 멀티 스레딩을 통해 병렬적(Parallel)으로 여러 작업을 수행할 수 있음

- 코루틴
  - 사용자 모드(User mode)에서 실행되는 경량 실행 단위
  - 함수 실행 컨텍스트 내에서 일시중지와 재개가 가능함
  - 프로그램 자체에서 실행 순서를 관리
  - 문맥 전환 시 사용자 공간에서 문맥전환 되므로 오버헤드가 적음
  - 병렬 실행(Parallel execution)이 아닌, 동시성(Concurrency)을 제공

<br>

**일반함수와 코루틴** <br>
코루틴은 그저 함수에 불과합니다. 다만, 실행을 제어할 수 있는 특별한 함수입니다. <br>
코루틴은 자신의 이전 실행 상태를 기억하고 있다가, 다시 실행될 때 일시 중지 되었던 시점에서 계속 실행이 가능한 함수입니다.

- 일반 함수
  - return 명령어를 통해 값을 반환하고 실행이 종료됩니다.
  - return 명령어 이후 코드를 실행할 수 있는 방법이 없습니다.
  - 반환된 후 프로세스 주소 공간의 스택영역에서 사라지므로, 아무런 정보도 저장하지 않습니다.

- 코루틴
  - yield 명령어를 통해 값을 반환하고 함수의 실행을 일시중지합니다.
  - 코루틴은 자신의 실행상태를 저장할 수 있습니다.
  - 코루틴은 반환된 후에도 코드를 이어서 실행할 수 있습니다.
  - 다시 실행될 때 이전 실행 위치부터 재개됩니다.


<br>

**JS에서 코루틴을 구현해보기**

generator function을 통해 코루틴을 직접 지원하지 않는 Javascript에서도 코루틴을 구현할 수 있습니다.<br>
generator function으로 간략히 코루틴을 구현해보면 다음과 같습니다.


```js
function* coroutine() {
  console.log("🟢 A");
  yield "A";

  console.log("🟠 B");
  yield "B";

  console.log("🔴 C");
  yield "C";
}

const co = coroutine();

console.log(co.next()); //첫번째 yield 의 반환값이 value로, done프로퍼티는 false
console.log(co.next()); //두번째 yield 의 반환값이 value로, done프로퍼티는 false
console.log(co.next()); //세번째 yield 의 반환값이 value로, done프로퍼티는 false
console.log(co.next()); //네번째 yield 는 없으므로 value는 undefined, done프로퍼티는 true
```

```text
//실행결과

"🟢 A"
[object Object] {
  done: false,
  value: "A"
}
"🟠 B"
[object Object] {
  done: false,
  value: "B"
}
"🔴 C"
[object Object] {
  done: false,
  value: "C"
}
[object Object] {
  done: true,
  value: undefined
}
```
제너레이터 함수는 [Generator Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator)를 반환합니다. <br>
Generator Object 프로토타입에 정의된 next 메서드를 호출하면, 제너레이터 함수를 재개할 수 있습니다.<br>
이때 next 메서드는 done, value 를 프로퍼티로 갖는 객체를 반환합니다. <br>
yield 키워드로 넘겨준 값은 value에, 제너레이터 함수가 끝났는지 여부는 done에 저장됩니다. <br>
이를 통해 제너레이터 함수의 실행 흐름을 제어할 수 있습니다. <br>


### 2.4.3 직관적인 코루틴 설명
코루틴은 일반적인 함수를 나누어서 실행할 수 있습니다.<br>
책 예시에서는 그림이 그려져 있으니, 저는 제너레이터로 비동기 실행을 제어하는 예시를 보겠습니다.<br>

**[예제1]** async/await는 코드가 자동으로 실행되지만, 제너레이터는 원하는 시점에 실행을 재개할 수 있습니다.

```js
function* asyncController() {
  console.log('🦷🪥 양치를 먼저하고 싶어요.')
  yield new Promise((resolve) => {
    setTimeout(() => {
      resolve('🙋✅ 양치 완료!');
    }, 1000);
  });

  console.log('🧼🫧 양치를 한 후 씻고 싶어요.');
  yield new Promise((resolve) => {
    setTimeout(() => {
      resolve('🙋✅ 씻기 완료!');
    }, 2000);
  });

  console.log('💆🛏️💤 다 씻었으니 침대에 누워서 쉬고 싶어요.');
  yield new Promise((resolve) => {
    setTimeout(() => {
      resolve('🙋✅ 눈떠 출근이야');
    }, 3000);
  });
}

async function main() {
  console.log('집에 도착');

  const controller = asyncController();

  const brushTeeth = await controller.next().value;
  console.log(brushTeeth);
  console.log('양치 하고 잠깐 누워있기');

  const wash = await controller.next().value;
  console.log(wash);
  console.log('씻고 잠깐 누워있기');

  const sleep = await controller.next().value;
  console.log(sleep);
  console.log('다시 하루 시작!');
}

main();
```

**[예제2]** 여러개의 작업을 번갈아 실행하는 멀티태스킹 구현에 유용합니다.
```js
function* meeting() {
  console.log('[회의1] 아침 출근이 너무 피곤한 건에 대하여');
  yield '☕️';

  console.log('[회의2] 점심메뉴 선정 건에 대하여');
  yield '🍔';

  console.log('[회의3] 퇴근이 늦어지는 건에 대하여');
  yield '💰';
}

function* work() {
  console.log('열심히 했더니 새싹이 자랐다.');
  yield '🌱';

  console.log('열심히 했더니 잎이 자랐다.');
  yield '🍀';

  console.log('열심히 했더니 꽃이 피었다.');
  yield '🌷';

  console.log('시간이 다 되어 낙엽이 떨어진다. 퇴근하자.');
  yield '🍂';
}

function multiTasking() {
  const todayMeeting = meeting();
  const todayWork = work();
  let meetingDone = false;
  let workDone = false;

  while (!meetingDone || !workDone) {
    if (!meetingDone) {
      const { value, done } = todayMeeting.next();
      meetingDone = done;
      if (!done) console.log('📌 회의 결과:', value);
    }

    if (!workDone) {
      const { value, done } = todayWork.next();
      workDone = done;
      if (!done) console.log('💼 업무 결과:', value);
    }
  }
}

multiTasking();
```

### 2.4.4 함수는 그저 코루틴의 특별한 예에 불과하다

코루틴이 일반 함수와 다른 점은 자신이 이전에 마지막으로 실행된 위치를 알 수 있다는 것입니다.<br>
이는 마치 운영체제가 실행하던 스레드를 멈춘 후, 다시 실행할 때 이전 위치에서 재개되는 것과 같습니다.<br>
컴퓨터 시스템은 주기적으로 타이머 인터럽트를 생성하고, 인터럽트가 처리될 때 마다 운영체제가 현재 스레드의 일시 중지 여부를 결정합니다.<br>
스레드는 운영체제가 알아서 스케줄링 해주기 때문에, 우리는 이를 신경쓰지 않아도 됩니다.<br>
반면 코루틴은 사용자 상태에서 타이머 인터럽트를 위한 작동 방식이 없으므로 yield와 같은 예약어를 통해 어디에서 CPU 리소스를 내어 줄 것인지 명시적으로 지정해야 합니다.<br>
코루틴은 온전히 사용자 상태 내에서 구현된 것이므로, 우리가 코루틴을 몇 개 생성하든 운영체제는 이를 알지 못합니다.<br>
즉 코루틴의 스케줄링 제어권은 프로그래머에게 있습니다.<br>

**CPU의 2가지 실행모드**
1. User Mode
  - 어플리케이션 실행 모드(웹 브라우저 등)
  - 하드웨어나 운영체제에 접근할 수 없음
  - 직접 파일을 열거나 네트워크 소켓 관리를 할 수 없으며 커널에게 요청
  - 커널에게 요청하는 것을 시스템 콜(system call)이라고 함 (`sys_open`)
2. Kernel Mode
  - OS의 커널이 실행되는 모드
  - 하드웨어와 메모리를 직접 제어
  - 파일 관리, 프로세스 및 메모리 관리


### 2.4.5 코루틴의 역사
코루틴은 새로운 기술일까요? 그렇지 않습니다. 사실은 스레드보다 오래 된 개념입니다.<br>
스레드가 없었던 시절, 동시성을 갖는 프로그램을 작성하기 위해서는 어쩔 수 없이 코루틴 함수를 작성할 수 밖에 없었습니다.<br>
스레드가 등장하고 운영체제가 프로그램의 동시 실행을 지원하기 시작하며 코루틴은 잊혀져 가다가, <br>
모바일 인터넷 시대가 되며 많은 요청을 처리하는 데 필요한 기술로 다시 주목받기 시작했습니다.<br>

### 2.4.6 코루틴은 어떻게 구현될까?

코루틴의 구현은 스레드 구현과 유사합니다.

- 코루틴의 상태정보
  - **CPU의 레지스터 정보**와 **함수 실행 시 상태 정보**가 포함됩니다.
  - 이 정보들은 함수의 스택 프레임에 저장됩니다.
  - 스택 프레임은 스레드를 위한 공간인 프로세스 주소 공간을 침범하지 않고, 힙 영역에 메모리를 요청합니다.

- 코루틴 동시성의 장점
  - 코루틴은 메모리 공간이 충분하다면 코루틴 개수에 제한이 없습니다 - 힙 영역 메모리가 충분
  - 코루틴 간 전환이나 스케줄링은 운영체제가 개입할 필요가 없어 문맥 전환이 비용이 낮고, 오버헤드가 적으며, 성능이 좋습니다. - 전적으로 사용자 모드에서 발생<br>
  - 코루틴은 동기방식으로 비동기 프로그래밍을 가능하게 합니다.
