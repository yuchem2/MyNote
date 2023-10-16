*Chomsky Hierachy*에 의해 분류된 [[문법(Grammer)]] 중 생성 규칙에서 좌변은 noterminal 심볼이고, 우변은  문법 심볼이 만들어낸 문장 중 하나인 문법.

$$If \; \alpha \rightarrow \beta \in P, \quad A \rightarrow \alpha, where \; A \; is \; nonterminal, \alpha \in V^*$$

이 문법으로 인해 생성된 [[언어(Language)]]를 [[CFL(Context Free Language)]] 라고 함

이를 인식하는 [[인식기(Recognizer)]]를 [[Pushdown Automata]]라고 함

[[프로그래밍 언어]]의 구문 구조를 명시하는데 널리 사용된다. 효율적이고, 잘 정의된 구문 분석 알고리즘을 통해 [[프로그래밍 언어]]를 문법적으로 표현하거나 번역을 수행할 수 있다. 이와 같은 방식으로 [[프로그래밍 언어]]의 구조를 CFG로 표현할 경우 다음과 같은 장점이 존재한다. 
+ 간단하고 이해하기 쉽다.
+ CFG로부터 인식기를 자동으로 구성할 수 있다
+ 프로그램의 구조를 생성규칙에 의해 구분할 수 있으므로 번역시에 유용하다

## **CFG에서의 [[Derivation]]**
---
Sential form에서 어떤 nonterminal symbol을 선택하는 지에 따라 유도를 구분할 수 있다.
+ Leftmost Derivation(좌측 유도): 가장 왼쪽에 있는 nonterminal부터 대치
+ Rightmost Derivation(우측 유도): 가장 오른쪽에 있는 nonterminal부터 대치

위와 같은 방법으로 유도되는 과정에서 나타나는 문장 형태를 각각 left-sential form, right-sential form이라고 한다. 또한, 각각의 방법을 적용해 일련의 생성 규칙 순서를 left parse, right parse라고 하며 이는 [[Syntax Analyzer(Parser)]]의 출력 형태 중 하나이다. 
+ left parse: 좌측 유도에서 적용된 생성 규칙 번호(top-donw parsing)
	+ start symbol로부터 sentence를 생성
+ right parse: 우측 유도에서 적용된 생성 규칙 번호의 **역순**(bottom-up parsing)
	+ [[String(Sentence)]]로부터 nonterminal로  reduce되고, 결국 start symbol로 reduce

### Derivation Tree
---
[[String(Sentence)]]이 유도되는 과정을 [[트리(Tree)]] 형태로 표현한 결과로 [[Ordered Tree]]이다. 이 구조는 다음과 같은 특징을 가진다.
+ Root node: start symbol
+ Internal node: $V_N$
+ External node(leaf node, terminal node): $V_T \cup \{\epsilon\}$
+ If $A \in V_N$, then a node $A$ has at least one descendent.
+ If $A \rightarrow A_1 A_2 ...A_n \in P$, then $A$가 subtree의 root가 되고, 좌로부터 $A_1, A_2, ..., A_n$이 자식 노드가 되도록 간선을 구성

유도 방법(좌측, 우측 유도)에 따라 유도 트리의 모양은 달라지지 않는다. 그러나 하나의 [[String(Sentence)]]을 생성하는 Derivation Tree가 두 개 이상 존재하는 경우도 존재한다. 이러한 경우에 그러한 CFG 무법을 모호하다고 하며, 비결정론적이라고도 한다.
> A context-free grammar G is **ambiguous** iff it produces *more than one derivation trees(nondeterministic)* for some sentence.

결정론적인 Pasing을 위해 CFG 문법을 deterministic하게 구성하거나 nondeterministic한 문법을 deterministic하게 바꿔야 한다.
