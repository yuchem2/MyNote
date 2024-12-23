## gradlew.properties
```
newArchEnabled=true # react native ^0.68
```

해당 옵션은 react native 0.68부터 제공되는 실험적인 옵션으로 아직 해당 옵션을 지원하지 않는 모듈이 많다. 하지만 여러가지 좋은 기능을 제공하는 것도 맞음..([관련 문서](https://reactnative.dev/architecture/landing-page))

react native 0.76 부터는 모든 프로젝트에서 기본으로 `true`를 가지는데, 여전히 다른 모듈에서 지원하지 않거나, 환경 세팅 문제가 있으면 빌드가 안되는 문제가 발생할 수 있다. 그러므로 새 아키텍쳐를 사용할 수 없는 경우 `false`로 변경해야 한다.

## react-native-config
해당 라이브러리는 react native에서 .env를 편하게 활용할 수 있게 도와주는 라이브러리이다. 숨겨야하는 정보가 있을 수 밖에 없기 때문에 이를 관리하기 위해서 .env는 필수적이다. 

하지만 기본적으로 android, iOS 환경에서는 기본적으로 default config 값들이 존재한다. ex) DEBUG, APPLICATION_ID, BUILD_TYPE 등...

이 이름들과 우리가 만든 .env 파일의 이름이 겹치는 경우 예기치 못하는 build error가 발생할 수 있다. 그러므로 해당 이슈를 해결하기 위해서는 .env 파일에 새로운 속성을 추가할 때 기존에 있는 default config 이름과 같은 이름인지 확인하고 이름을 짓는 것이 필요하다.

