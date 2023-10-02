
# 오픈소스

### Replicate 기반 슬랙 이모지 생성 도구

**Replicate**는 클라우드 환경에서 **자신만의 머신러닝 모델을 구동할 수 있고, API 형태로 사용이 가능한** 플랫폼입니다.
아래와 같은 파이썬 코드로 형태로 간단하게 머신러닝 모델을 구동할 수 있습니다. 현재는 7개 환경에서 구동할 수 있으며,
[링크](https://replicate.com/docs)에서 확인하실 수 있습니다.

```python
import replicate

output = replicate.run( "stability-ai/sdxl:2b017d9b67edd2ee1401238df49d75da53c523f36e363881e057f5dc3ed3c5b2",
input={"prompt": "an astronaut riding a rainbow unicorn"}, )

```

돌아와서 Replicate 기반으로 슬랙 이모지를 만들 수 있는 제품이 런칭되었습니다.
[오픈소스](https://github.com/cbh123/emoji)로 공개되었고, 실제 웹 환경에서도 [구동](https://emoji.fly.dev/)할 수 있습니다.

누구나 한 번 정도는 팀과 그리고 커뮤니티에서 슬랙을 통해 이모지를 사용하고, 추가한 경험이 있으실 겁니다. 이 과정에서 직접 디자인을 해서 추가하거나, [슬랙이모지](https://slackmojis.com/) 페이지를 이용하셨을 겁니다.

해당 오픈소스는 직접 디자인 하는 시간을 **생성형 인공지능에게 맡겨서 시간을 단축**했습니다. 결과물도 프롬프트만 잘 입력한다면 수준 높은 결과물을 받아볼 수 있는 것 같습니다!

추가적으로 해당 오픈소스는 함수형 프로그래밍 언어 [엘릭서](https://elixir-lang.org/)와 함께 [피닉스](https://phoenixframework.org/) 프레임워크가 사용되었습니다.
처음 해당 오픈소스를 보았을 때는 PQS, 아파치에서 출시한 쿼리 서버를 사용한 것으로 이해하였고 해당 서비스를 만드는데 있어서 SQL Layer 모델이 필요한가 고민했으나, **엘릭서 기반에 피닉스 프레임워크**였습니다.

Elixir는 카카오엔터프라이즈 및 한국축산데이터 등에서 서버 개발 언어로 채택되었습니다. 카카오엔터프라이즈에서 카카오워크를 개발하는 과정에서 사용되는 엘릭서 경험은 [유튜브](https://youtu.be/NotXpRovDoA?si=3rwZKCcmQyADy2bo)에서 확인하실 수 있습니다. 하지만, Elixir는 2023년 스택오버플로우 서베이 통계에서 2~2.5% 정도로 꽤 낮은 통계치를 보유하고 있습니다. **낮은 수치에도 불구하고 잘 성장하고 있는 언어 중에 하나라고 보고 있습니다.** (2022년 기준은 2-3등을 차지할 정도로 꽤 높은 통계치를 가지고 있었지만, 2023년 대폭 감소하였습니다.)

아래는 간단한 **Elixir** 코드입니다.

복권 번호를 생성하는 코드로서 파이프 연산자(|>)를 통해서 읽기 쉽게 개발할 수 있는 매력점들이 포함되어 있습니다.
기존에 복권 번호를 생성하는 코드를 타 언어로 구성한다면 중첩되는 코드가 늘어날 수도 있지만, **Elixir는 파이프 연산자를 통해서 가독성과 실행 순서를 확보하였습니다.**

```elixir
1..45
|> Enum.to_list
|> Enum.shuffle
|> Enum.take(6)
|> IO.inspect
```

이와 같은 매력점들은 타입스크립트에도 존재합니다. 타입스크립트 패턴 매칭 라이브러리로서 **가독성과 타입 안정성을 동시에 잡은 [라이브러리](https://github.com/gvergnaud/ts-pattern)가 있습니다.** 해당 라이브러리는 아래와 같은 예시로 단 번에 도입을 결정할 수 있습니다.

<img src="https://user-images.githubusercontent.com/9265418/231688650-7cd957a9-8edc-4db8-a5fe-61e1c2179d91.gif" width="600">

기존 웹 환경에서는 상태에 따른 UI 상태를 위와 같이 관리하였습니다.
기본적으로 우리는 로딩/성공/실패 3가지를 기본적인 상태로 나눌 수 있지만, **이것보다 더 많은 상태가 있다면** 어떨까요?

인터넷 은행을 만든다고 가정 후 송금 과정에서 발생될 예외 사항을 고민해봅시다.

- 송금 중
- 송금 성공
- 송금 실패
- 잔액 부족
- 송금액 초과
- 은행 점검
- 서비스 오류

간단하게 생각해도 7개 정도 예외 사항이 존재합니다. 이 예외 사항을 모두 인터페이스에 반영하면 어떨까요?
**굉장히 난해하고 읽기 어려운 코드**가 나올 것입니다.

해당 라이브러리를 도입하는 과정에서 한 가지 고민 포인트가 존재할 수도 있습니다. 최근에 나온 라이브러리 중 Zero-Runtime 환경 타입 매칭 라이브러리 [Pattycake](https://github.com/aidenybai/pattycake)도 존재합니다. 위에 설명되었던 라이브러리에 **속도 이슈를 적게는 36배, 많게는 66배 정도에 빠른 속도로 타입 매칭을 제공**하고 있습니다.

현재는 모든 기능은 지원하진 않지만, 추후에는 **Ts-Pattern**에서 제공되는 기능 모두를 제공할 계획을 가지고 있습니다.

### 고품질 이메일을 위한 이메일 작성 도구

고품질 이메일 작성을 위해서 만들어진 오픈소스 [ReactEmail](https://github.com/resendlabs/react-email) 소개드립니다.

해당 오픈소스 소개 이전에 개발사인 **Resend**를 소개할 필요가 있는 것 같습니다.
[Resend](https://resend.com/)는 Y-Combinator에서 졸업한 스타트업으로서 개발자를 위한 이메일이라는 슬로건으로 서비스를 만들어가고 있습니다. 발송되는 메일이 스팸으로 가지 않고 고객에게 정상적으로 도착할 수 있도록 하고 만들고 있습니다.

현재는 **큰 주제로는 10개 플랫폼을 지원하고 있고, 그 안에서 세부적으로는 28개 이상 환경을 지원**하고 있습니다.
핏짜에서도 현재 Resend, ReactEmail으로 뉴스레터를 전송하고 있습니다.

Vercel Edge Functions 환경에서 실제로 구동한 결과 굉장히 좋은 경험을 받았습니다.

돌아와서 ReactEmail은 Resend Labs에서 개발한 차세대 이메일 작성 도구입니다.

웹 환경에서 라이브러리 및 프레임워크는 굉장히 많은 발전을 이루었지만,
**이메일에 작성 부분에선 아직 속도를 내지 못 하고** 있습니다. 

그래서 나온 것이 **ReactEmail** 입니다.

```bash
yarn add @react-email/components -E
```

위와 같이 설치할 수 있습니다.

현 세대에게 필수적인 다크모드를 지원하고 있으며, **GMail과 Outlook 에 대한 불일치도 처리해주고** 있습니다.
이에 따라 각 이메일 클라이언트 간에 동일한 경험을 줄 수 있다는 장점이 존재합니다.

```js
import { Button } from '@react-email/button';

const Email = () => {
  return (
    <Button href="https://example.com" style={{ color: '#61dafb' }}>
      Click me
    </Button>
  );
};
```

다음과 같은 예시 코드를 가지고 있습니다. 일반적으로 리액트에서 컴포넌트를 불러서 사용하는 형태입니다.
해당 형태로 각 이메일 클라이언트에게 전송할 수 있습니다.


### 자바스크립트 `in`과 `instanceof` 개선 제안

약 2주 전 자바스크립트 내 `in`과 `instanceof`에 대한 개선을 요구하는  [제안](https://github.com/gorosgobe/proposal-negated-in-instanceof)이 올라왔습니다.
우리가 이것을 이해하기 위해서는 **프로토타입과 프로토타입 체인**을 이해해야 합니다.

자바스크립트는 프로토타입 기반의 언어입니다. 예전 임성묵님께서 작성해주신 [자바스크립트는 왜 프로토타입을 선택했을까](https://medium.com/@limsungmook/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%8A%94-%EC%99%9C-%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85%EC%9D%84-%EC%84%A0%ED%83%9D%ED%96%88%EC%9D%84%EA%B9%8C-997f985adb42) 라는 글을 보면 더욱 더 철학적으로 이해할 수 있을 것입니다. 물론 자바스크립트의 디자인 철학 등을 더욱 더 딥하기 위해서 [클래스는 ES6의 최대 실수다](https://www.youtube.com/watch?v=PSGEjv3Tqo0&t=300s) 영상도 참고하면 좋을 것 같습니다.

**프로토타입 객체는 무엇인가?**

다시 돌아와서 프로토타입 객체는 자바스크립트에서 꽤 중요한 역할을 하고 있습니다.
자바스크립트에서 모든 객체는 자신의 부모 역할을 하는 최상위 객체와 연결되어 있고, 이 부모 객체를 프로토타입 객체 또는 프로토타입이라고도 칭할 수 있습니다.

**프로토타입 체인은 무엇인가?**

프로토타입 체인은 특정 객체의 프로퍼티나 메소드 접근하였을때, 현재 객체의 해당 프로퍼티가 존재하지 않는다면 `__proto__`가 가리킴을 따라 부모 역할을 하는 프로토타입 객체의 프로퍼티나 메소드를 차례로 검색하는 것을 우리는 프로토타입 체인이라고 할 수 있습니다.

```js
a in obj;
a instanceof C;
```

결론적으로 위와 같은 코드를 작성했을 떄 프로토타입 체인이나 오브젝트를 알 수 있습니다.

```js
!(a in obj);
!(a instanceof C);
```

이를 표현식의 결과를 부정하려면 위와 같이 논리 NOT `(!)`를 사용하여서 래핑할 수 있습니다.
다만, 이 방식으로 진행하였을 때 몇 가지 문제가 발생하고 **이 문제가 해당 제안에 핵심**입니다.

- [Bugs in negated `in` expressions](https://github.com/oven-sh/bun/issues/5800) Bun에서 발생한 `in` 연산자 관련 버그
- [Bugs in negated `in` and `instanceof` expressions](https://bugs.webkit.org/show_bug.cgi?id=261815) WebKit에서도 발생된 `in` 과 `instanceof` 관련 버그

깃헙 내 포함된 모든 저장소에서 빈번하게 발생되는 이슈라고 제안에서는 언급되고 있습니다.

이러한 경우 타입스크립트 및 린트를 사용할 것을 `Bloomberg` 에서는 권장하였지만, 인체공학적인 설계가 되지 않은 `in` 과 `instanceof` 는 개선될 여지가 충분히 필요하다고 제안자는 설명하고 있습니다.

이것을 제외하고도 혼란, 가독성, DX 등 다양한 문제를 제기하고 있습니다.

```js
a !in obj;
a !instanceof obj;
```

제안자는 기존 형태를 위와 같이 변경할 것을 제안하고 있습니다.
이 과정에서 추가적으로 그룹화하지 않아 발생되는 문제가 없어져 더욱 더 안전하며, 그룹화가 필요하지 않기 때문에 가독성 측면에서 더욱 더 증가한 것으로 제안자는 설명했습니다.

또한 타입스크립트 TC39에서도 올라온 제안 중에 하나인 [패턴 패칭]([https://github.com/tc39/proposal-pattern-matching#is-expression](https://github.com/tc39/proposal-pattern-matching#is-expression)) 관련 제안에서 ``a instanceof b is ~`` 를 자바스크립트 내에서도 지원하기를 원하고 있습니다.

꽤 흥미로운 제안이라서 독자 여러분에게도 전달하고자 해당 제안을 가져오게 되었습니다.


### Deno 1.37 Jupyter용 커널 제공

[Deno 1.37 업데이트](https://deno.com/blog/v1.37)에서는 Jupyter Notebook용 [커널](https://deno.com/blog/v1.37#jupyter-notebook-integration)을 제공함으로서 자바스크립트와 타입스크립트를 사용할 수 있게 되었습니다. 기존에 Jupyter 환경에서 타입스크립트 사용하는 것이 굉장히 불편하고 고통스러웠지만 Deno에서 커널을 제공함으로서 더욱 더 편하게 사용할 수 있다는 장점이 생겼습니다.

<img src="https://fitzza.xyz/_static/assets/jupyter_notebook.png" width="600">

Deno의 모든 API를 Jupyter에서 접근할 수 있으며, 흔히 설치하는 npm 모듈도 접근할 수 있습니다.
데이터 시각화에 필요한 D3 라이브러리도 가져와서 그래프를 시각화 할 수 있다는 것을 업데이트 노트에서 보여주고 있습니다.

### 그 외 좋은 오픈소스 도구

- [ts-remove-unused](https://github.com/line/ts-remove-unused#ts-remove-unused) 라인에서 출시한 오픈소스로 사용되지 않는 타입스크립트 코드를 제거해주는 CLI 도구
- [ni](https://github.com/antfu/ni) 프로젝트에 맞는 패키지 매니저로 설치해주는 CLI 도구 _`npm i` in a yarn project, again? F..k!_
- [ts-reset](https://github.com/total-typescript/ts-reset) css-reset처럼 타입스크립트용 재설정 도구, 이에 대한 TS 메인테이너 반응 → [링크](https://youtu.be/_-RyV65J4kE?si=Fyr_nQ_s1wAh7HKy)
- [Wasmnizer-ts](https://github.com/intel/Wasmnizer-ts) 인텔에서 나온 타입스크립트를 WasmGC로 컴파일 도와주는 툴체인