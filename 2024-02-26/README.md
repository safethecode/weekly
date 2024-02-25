### React Strict Dom (RSD)

```
# 3줄 요약

- 리액트 네이티브를 웹과 네이티브 모두 지원하는 React Strict DOM (RSD)가 제안되었으며, 비표준 API를 줄이고 성능을 향상시키는 것이 목표입니다.

- RSD는 Environment, Elements, Components, Props, Styling API 등 다양한 API를 포함하며, 이를 통해 개발자들은 브라우저와 네이티브 앱에서 더 효율적으로 개발할 수 있습니다.

- React Native의 레이아웃 엔진으로 사용되는 Yoga에 대한 대안으로 Taffy가 제안되었고, 이러한 기술적인 발전이 React Native 생태계를 더욱 발전시킬 것으로 예상됩니다.
```

###### StyleX

최근 페이스북에서 개념 정도로 존재했고, 유저들이 그에 따라서 오픈소스로 만들었던 [StyleX](https://stylexjs.com/)가 세상에 등장하였습니다. 이에 따라서 **꽤 반응이 엇갈리기도** 합니다. 기존 React-Native 에서 사용되었던 [StyleSheet](https://reactnative.dev/docs/stylesheet) 형태와 비슷하여서 뜨겁지 못 한 반응도 있었고, StyleX 문서에 존재하는 [Thinking in StyleX](https://stylexjs.com/docs/learn/thinking-in-stylex) 문서를 정독 후 페이스북에서 어떤 이유에서 만들게 되었는지를 알 수 있는 반응도 있었습니다. 여기서 가장 중요한 것은 지금도 굉장히 많은 스타일링 도구들이 나오고 있고 우리는 이 과정에서 **선택과 집중을 해야 한다는 새로운 과제**가 눈앞에 있다고 생각됩니다.

> 소문으로만 존재했던 라이브러리가 세상에 공개되었지만, 2024년 02월 26일 기준 사용자는 365명 밖에 되지 않는 쓸쓸한 스타일링 라이브러리가 되었습니다.

###### React-Native

본론으로 돌아가서 리액트를 다뤘던 프론트엔드 개발자 및 최신 트렌드를 알고 계신 개발자분들께서는 페이스북에서 만든 [React-Native](https://reactnative.dev/)를 많이 들어보셨을 것입니다. 기업 내에서 앱을 개발해야 한다고 했을 때 항상 나오는 [Flutter](https://flutter.dev/)와 함께 크로스 플랫폼 강자입니다.

###### DOM

오늘 이야기에서는 [DOM](https://developer.mozilla.org/ko/docs/Web/API/Document_Object_Model/Introduction) 개념이 존재합니다. 웹을 다루셨던 분들이라면 DOM이 무엇인지 면접 과정과 개발 과정에서 항상 언급되었던 개념일 것입니다. 이번 이야기를 이해하기 위해서는 기존에 웹 환경에서 DOM 개념을 이해하는 것이 중요합니다. 리액트는 DOM을 통해서 UI를 업데이트하고, 이 과정을 효율적으로 하기 위해 [Virtual DOM](https://ko.legacy.reactjs.org/docs/faq-internals.html) 이라는 개념도 함께 적용됩니다. UI를 업데이트하는 과정에서 제일 중요한 친구입니다.

![DOM<>Virtual DOM](https://miro.medium.com/v2/resize:fit:1400/1*sWfjl94Bnshi1kewFCL0gg.png)

그렇다면 React-Native는 어떤 식으로 UI를 업데이트 할까요? 지금부터는 줄여서 **RN**으로 칭하겠습니다. RN은 네이티브 모듈을 활용하여 네이티브 UI 구성 요소를 조작합니다. 웹 환경에서 DOM을 조작하여서 UI를 업데이트 하는 방식과는 차이가 있습니다.

![React-Native DOM](https://blog.kakaocdn.net/dn/bNcQ9s/btqD6P0q2e0/F16AntILWVGjz71PJ0G6wK/img.png)

결국에는 RN에서는 각 운영체제와 관련된 브릿지를 통해서 네이티브 UI 구성 요소를 조작하게 되는 것입니다. 그렇기 때문에 RN에서 웹 형태로 내보내고자 하는 요구사항이 존재한다면 [React-native-web](https://github.com/necolas/react-native-web)과 같은 RN <> RNWeb <> Web 같은 레이어가 하나가 더 필요하게 됩니다.

이 라이브러리는 현재까지 가장 완벽하고 널리 사용되지만 DX 및 UX 비용이 함께 증가한다는 단점이 있습니다. 이 단점은 RN의 비표준 API와 일치하도록 표준 API 및 이벤트 등을 수정해줘야 합니다. 그렇기 때문에 2022년 [RFC: React DOM for Native (reduce API fragmentation)](https://github.com/react-native-community/discussions-and-proposals/pull/496) 이라는 제목으로 제안이 올라오게 됩니다. 제안자는 React-native-web 라이브러리를 만들었던 메타 소속 개발자
[Nicolas Gallagher](https://github.com/necolas) 입니다.

> 니콜라스가 올린 제안을 보기 전에 [RFC: DOM traversal and layout APIs in React native](https://github.com/react-native-community/discussions-and-proposals/blob/main/proposals/0607-dom-traversal-and-layout-apis.md) 문서를 함께 보면 좋을 것 같습니다.

###### RSD가 왜 나오게 되었을까요?

> React Strict DOM (RSD) is a subset of React DOM, imperative DOM, and CSS that supports web and native targets

니콜라스는 기존에 자신이 제작했던 라이브러리에서 얻은 것들을 기반으로 이번 제안을 작성했습니다.

![RN<>RNWeb](https://user-images.githubusercontent.com/239676/205388174-6fce19f1-8b3a-4c6e-82f1-89ef9c21ab7d.png)

이제부터는 React-Native-Web은 줄여서 **RNWeb**으로 칭하겠습니다. 기존 RN은 Web API 기반으로 작성된 것들이 존재하지만, Web API 표준에 따르지 않는 여러 API가 함께 포함되어 있습니다. 이런 API로 인해서 네트워크 및 런타임 단계에서 성능 문제가 발생되는 문제를 야기할 수 있게 되었습니다.

해당 제안은 결국에는 기존에 성능 문제 등을 일으켰던 비표준 API 등을 해결하고, 브라우저를 타겟으로 작업할 때 더 나은 성능을 제공할 수 있도록 하는 것을 목표로 하고 있습니다.

- minimize the overhead when running in browsers
- reduce developer education required to learn features
- set clear and cohesive API end-state expectations for contributors to aim for
- accelerate the framework's development by avoiding API design costs
- support backwards compatibility
- develop universal codebase

이 제안을 통해서 비표준 API가 모두 표준으로 바뀌면 위와 같은 이점을 얻을 수 있다고 니콜라스는 얘기합니다. 결국에는 앞에서 언급되었던 DX 및 UX를 효과적으로 얻을 수 있다는 뜻입니다.

RSD가 구현되면 아래와 같은 구조로 구현될 것입니다.

![Strict DOM](https://user-images.githubusercontent.com/239676/205388313-e58f8793-51cd-4a6e-86e7-89425fd2d0e1.png)

RSD가 구현되면 현재 RN과는 차원이 다를 것이라고 니콜라스는 얘기합니다. 위에서 RN API, RNWeb 으로 작업되었던 기존 프로젝트와 달리 효율적으로 RSD 안에서 모든 것을 해결할 수 있게 됩니다. 물론, 아직은 모든 것을 다 지원하진 못 한다고 합니다. 점진적으로 더욱 더 지원을 할 것임을 약속하고 있습니다.

결국 해당 제안이 구현되면 우리는 이제 브라우저를 지원하기 위해 여러 트릭과 라이브러리를 사용하는 수고로움은 떠나가고 React DOM에 StyleX 같은 CSS 컴파일러만 결합하면 됩니다. 니콜라스는 이 과정에서 [StyleQ](https://github.com/necolas/styleq)라는 자기 자신이 만든 라이브러리를 사용하고 있는데 일부 StyleX랑 비슷한 부분들이 존재하긴 합니다. 결국에는 이런 것들을 염두한 큰 그림이 아닌가 조심스럽게 예측해봅니다.

여기서 StyleX를 언급하기 위해서 서론에 언급했다면 믿으시겠습니까?

> 우린 이걸 리액트 네이티브 희망편이라고 합니다.

꽤 흥미로운 건 RSD 내에서 현재 RFC에 있는 [useEvent](https://github.com/reactjs/rfcs/blob/useevent/text/0000-useevent.md)가 언급되었다는 것입니다. RFC에 있음에도 RSD와 같은 중요한 변경점에서 언급된다는 건 적용될 확률이 굉장히 높다라는 것이 개인적인 판단입니다. 물론 사용자들 반응은 잘 모르겠습니다. 기존에 만들어서 사용하던 개발자 분들도 꽤 많기에 공식적인 훅으로 나오는 것에 있어서 의미가 있는지는 여러분들 판단에 맡기겠습니다.

###### API

- Environment API.
- Elements API.
- Components API.
- Props API.
- Styling API.

API에서 큰 주제로는 위와 같은 API가 추가/정비 예정입니다. 사실 상 큰 주제로만 보면 어떤 API인지 알 수가 없는 것은 사실입니다. 그래서 아래에 몇 가지 리스트를 가져왔습니다.

- Environment API

  - window.devicePixelRatio
  - window.navigator
  - window.performance
  - window.addEventListener()
  - ResizeObserver
  - ...

- Elements API

  - element.clientWidth
  - node.contains()
  - node.parentNode
  - EventTarget.addEventListener()
  - ...

- Components API

  - html object(button, section., etc.)
  - 우리가 아는 마크업에서 사용되는 것들이 모두 포함됩니다.
  - ...

- Props API

  - aria-\*
    - 웹에서만 사용되었던 접근성이 추가됩니다. [ARIA 1.2](https://www.w3.org/TR/wai-aria-1.2/)를 지원합니다
  - tanIndex(0, -1)
  - role(웹과 동일하게 Button, Menuitem 등 존재합니다.)
  - ...

- Styling API

  - css()

    ```js
    const styles = css.create({
      foo: {
        width: 100
      },
      bar: (color) => ({
        color
      })
    });

    <div style={styles.foo} />
    <div style={styles.bar(props.color)} />
    ```

  - CSS 호환성
    - aspectRatio
    - fontWeight
    - position
    - ...

현재 표기된 것 외에도 너무나 많은 것들이 RSD 내에서 포함되어 구현이 진행되고 있습니다. 이 중에서 일부는 이미 개발된 것들도 있긴 합니다. RN을 점진적으로 웹 표준에 가깝도록 만들고 비표준 API를 줄여나가는 과정은 RN이 만들어진 역사 상 가장 중요한 작업이 되지 않을까 싶습니다.

###### 레이아웃 엔진

제안을 보다가 RN에 레이아웃 엔진에 대해서 알게 되었습니다. 현재 RN은 페이스북에서 만들고 있는 [Yoga](https://github.com/facebook/yoga) 라는 레이아웃 엔진을 사용하고 있습니다. 해당 엔진은 PDF 렌더링 등으로 널리 사용하고 있는 React-PDF 에서도 사용하고 있는 엔진입니다. 물론 RN에서도 사용하고 있습니다.

제안을 보면서 재밌던 부분은 Grid 관련된 부분이 제일 재밌던 것 같기도 합니다. Yoga 엔진은 `display: grid`를 지원하지 않는다고 합니다. 그래서 최고의 웹이라면 그리드를 지원해야 하는데 엔진은 지원하지 않으니 지원하는 것이 어떤지에 대한 제안이였고, 이 과정에서 구축하기가 굉장히 복잡하다는 언급들이 있었습니다. 그런데 그렇지 않다라는 의견이 있었고, 그 의견을 낸 사람은 재밌게도 Rust를 기반으로 레이아웃 엔진을 만들고 있었습니다.

그가 만드는 레이아웃 엔진 이름은 [Taffy](https://github.com/DioxusLabs/taffy) 입니다. 재밌게도 [Yoga vs Taffy](https://github.com/DioxusLabs/taffy?tab=readme-ov-file#benchmarks-vs-yoga) 라는 섹션에서 벤치마크로 대결을 하고 있습니다. 순간 혹했던 건 정말 Yoga 보다 좋았다는 것입니다. 물론 기존 레거시들로 인해서 이런 부분에서는 당연하게도 느릴 수도 있다고 판단되긴 합니다. Flutter 진영에서 바라봤던 Impeller vs Skia 느낌에 대결이긴 할 것 같습니다.

해당 논의가 궁금하시다면 [링크](https://github.com/react-native-community/discussions-and-proposals/pull/496#discussion_r1127188253)를 확인해주시길 바랍니다.

> 재밌게도 이 논의가 이루어진 후 6개월 뒤 Grid가 추가되었습니다.

###### 끝마침

오늘은 React Strict DOM API 관련해서 공유드립니다. RN에서 역사 상 가장 큰 작업이 될 것이고 해당 제안이 성공적으로 RN 내에서 작업된다면, RN 채용부터 **시장 자체가 바뀔 수도 있는 제안**이라고 생각됩니다.

StyleX, Yoga, Taffy 등 세상에는 너무나 다양한 도구와 엔진들이 존재하고 우리는 이것들을 선택할 수 있는 세상에 있다는 것 자체가 굉장히 흥미롭습니다.

이 제안을 통해 RN이 조금 더 브라우저에 친화적으로 바뀌어서 정말 **크로스 플랫폼을 안전하고 더 많은 수고로움이 들어가지 않도록 개발되는 그 날이 오길** 기대해봅니다.

다음 뉴스레터는 **<리액트에서 변화되는 React-Forget은 무엇일까?>** 라는 제목으로 찾아뵙겠습니다.

아래는 제가 오픈소스를 보면서 흥미로웠던 오픈소스입니다.

###### 흥미로운 오픈소스

- [Pkgroll](https://github.com/privatenumber/pkgroll): 타입스크립트와 ESM을 지원하는 새로운 패키지 번들러
  - [tsup](https://github.com/egoist/tsup)과 같은 번들러들이 점차 많이 나오고 있는 것 같습니다.
- [Observable Framework](https://github.com/observablehq/framework): 대시보드 및 보고서를 위한 정적 사이트 생성기
- [not-paid](https://github.com/kleampa/not-paid): 클라이언트가 돈을 내지 않았다면 투명도로 괴롭히세요.
  - 보면서 외주를 진행했던 한 사람으로서 흥미로웠던 라이브러리입니다.
- [Dark-mode-toggle](https://github.com/GoogleChromeLabs/dark-mode-toggle): 크롬랩스에서 만들고 있는 다크 모드 토글입니다.
  - 사실 왜 크롬랩스에서 만들고 있나 의문이 들었던 흥미로운 오픈소스입니다.
- [LLRT](https://github.com/awslabs/llrt): Low Latency RunTime 약자로서 AWS Lambda에서 실행되는 자바스크립트 런타임을 10배 빠른 속도와 2배 더 저렴한 비용으로 제공할 수 있습니다.
  - AWS에서 이런 걸 만든 것이 꽤 흥미롭긴 했습니다. 속도를 빠르게 하려고 했으나, 비용도 저렴해진 케이스가 아닌가 싶습니다. 사용자 입장에선 매우 좋은 것 같습니다!

> 여기에서 가장 끌리는 건 LLRT가 아닌가 싶습니다.

감사합니다.

개발자를 위한 뉴스레터, 핏짜팀 드림
