
주어진 [[문법(Grammer)]]이 나타내는 [[언어(Language)]]가 무엇인지 구하기 위해 [[문법(Grammer)]]의 생성 규칙으로부터 유추해나가는 과정을 말함

1. $\Rightarrow$ 는 직접적인 유도를 통해 나온 결과를 말한다$if \; \alpha \rightarrow \beta \in P \; and \; \gamma, \delta \in V^* \; then \; \gamma\alpha\delta \Rightarrow \gamma\beta\delta$
2. $\Rightarrow^*$ 는 0번 이상의 유도 과정을 통해 유도된 결과를 말한다
3. $\Rightarrow^+$ 는 1번 이상의 유도 과정을 통해 유도된 결과를 말한다

유도과정에서  $\omega$ 를 다음과 같이 정의한다
1. $\omega \; is \; a \; sential \; form \; of \; G \; if \; S \Rightarrow^* \omega \; and \; \omega \in V^*$
2. $\omega \; is \; a \; sentence \; of \; G \; if \; S \Rightarrow^* \omega \; and \; \omega \in V^*_T$