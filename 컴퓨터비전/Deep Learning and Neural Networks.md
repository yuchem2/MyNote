## Perceptrons
하나 이상의 perceptron으로 구성된 구조를 MLP, ANN이라고 한다. Perceptron은 artificial neuron으로, 실제 생물학적 뉴런을 모방해 만들어진 형태이다. 

![|400](Pasted%20image%2020240418111344.png)

입력을 받아 Neuron function에서 연산을 수행하고, 결과를 내보내게 된다. Neuron function은 weight와 bias를 가지고 있는데, 이 값들을 파라미터라고 하며 학습에서 업데이트가 되어 원하는 출력 값을 갖도록 학습된다. 가장 기본적인 형태의 neuron function은 $y=wx+b$의 형태를 가진다. 

![| 400](Pasted%20image%2020240419133050.png)

일반적으로 perceptron의 입력은 vector의 형태를 가지기 때문에 weight도 vector의 형태로 구성된다.
$$y = \sum x_i \times w_i + b$$
그래서 실제로 사용되는 neuron function은 위와 같은 형태를 띄며 코드로는 `y = np.dot(w.T, x) + b`와 같이 작성한다.

weight은 각 특징과 1:1로 대응되어 곱해지게 되는데 weight에 따라 각 특징이 얼마나 결과에 반영될지가 결정된다. 즉, 학습을 진행하면서 weight의 변화에 따라 그 대응되는 특징이 얼마나 객체를 인식하고, 구분하는데 필요한 것인지 확인할 수 있다.

bias는 neuron function가 직선 방정식 형태를 띄기 때문에 다양한 형태의 직선 방정식을 만들어 객체를 더 잘 나눌 수 있도록 하기 위해 추가된 값.

하나 이상의 레이어를 쌓기 위해서는 perceptron 연산에 비선형성이 추가되어야 한다. 모든 perceptron 연산은 선형연산이기 때문에 여러 레이어를 쌓더라도 하나의 레이어로 판단할 수 있다. 그래서 비선형성을 추가하기 위해 activation function이 사용됩니다. 자주 사용되는 activation function은 다음과 같다.

| function                          |                                                      form                                                       | features                                                                                                     | graph                                    |
| --------------------------------- | :-------------------------------------------------------------------------------------------------------------: | ------------------------------------------------------------------------------------------------------------ | ---------------------------------------- |
| Linear transfer function          |                                                   $z = mx+ b$                                                   | identity function이라고도 불리는 이 함수는, 신호를 변경 없이 통과시킨다는 것을 나타냅니다. 즉, 실제로 입력과 출력이 동일하기 때문에 실제로 활성 함수가 없다는 것을 의미합니다. | ![](Pasted%20image%2020240419141201.png) |
| Heaviside step function           | $x = \begin{cases} 0 \quad \text{if } w \cdot x + b \leq 0 \\ 1 \quad \text{if } w \cdot x + b > 0 \end{cases}$ | 0과 1의 출력만 가지는 이 함수는 이진 분류에서 많이 사용되는 활성화 함수입니다.                                                               | ![](Pasted%20image%2020240419141210.png) |
| Sigmoid/logistic function         |                                           $y = \cfrac{1} {1+e^{-z}}$                                            | 가장 많이 사용되는 활성화 함수 중 하나로, 이진 분류에서 클래스의 확률을 예측할 때 사용됩니다.                                                       | ![](Pasted%20image%2020240419141221.png) |
| Softmax function                  |                                 $\sigma(x_j) = \cfrac{e^{e_j}}{\sum_i e^{x_i}}$                                 | 두 개 이상의 클래스가 존재하는 경우 사용되며, 출력 값의 전체 합이 1이 되도록 만들어 주는 함수입니다.                                                  | ![](Pasted%20image%2020240419141332.png) |
| Hyperbolic tangent function(tanh) |                 $\tanh(x) = \cfrac{\sinh(x)} {\cosh(x)} = \cfrac{e^x - e^{-x}} {e^x + e^{-x}}$                  | sigmoid와 유사한 형태로 출력 값의 범위가 조정된 형태입니다. 출력 값의 범위가 [-1, 1]로 한정됩니다.                                              | ![](Pasted%20image%2020240419141236.png) |
| Rectified linear unit (ReLU)      |                                               $f(x) = max(0, x)$                                                | 음수 결과를 모두 0으로 만들어 주는 함수입니다. 양수 값은 그대로 나오게 됩니다. 일반적으로 히든 레이어의 활성화함수로 추천되는 형태입니다.                              | ![](Pasted%20image%2020240419141246.png) |
| Leaky ReLU                        |                                             $f(x) = max(0.01x, x)$                                              | ReLU에서 음수 결과를 모두 0으로 만들어 무시하는 단점을 해결하기 위해 음수 결과를 모두 0으로 만들어주는 대신 0.01를 곱해서 나타냅니다. 양수 값은 ReLU와 동일한 결과를 나타냅니다. | ![](Pasted%20image%2020240419141305.png) |

## Feedforward calculation
 한 개 이상의 perceptron으로 구성된 하나 이상의 레이어를 입력층부터 시작해 출력층까지 차례대로 선형 연산 및 활성화함수를 적용하는 연산을 말한다. 일반적으로 하나 이상의 레이어를 구성하는 경우 계산이 매우 복잡하기 때문에 행렬연산을 통해 나타내고, 연산을 수행한다.
 
![|400](Pasted%20image%2020240419142543.png)

![|400](Pasted%20image%2020240419142601.png)
## Error Functions(Loss function)
모델을 학습을 진행하기 위해 자신의 출력 결과(예측 값)과 실제 입력의 출력값(실제 값)을 비교하는 과정을 거치게 된다. 이를 통해 예측한 결과가 실제 결과가 얼마나 차이를 나는지 확인할 수 있다. 

Error function은 neural network의 예측이 실제 결과와 얼마나 다른지를 측정하는 함수로, 다양한 형태가 존재한다. ML, DL 모두 정확한 값을 찾는 문제가 아니라 최적화 문제이기 때문에 error function의 역할이 매우 중요하다. Error function으로 사용될 수 있는 함수들은 다음과 같다.

| function                 | form                                                     | features                                                                     |
| ------------------------ | -------------------------------------------------------- | ---------------------------------------------------------------------------- |
| Mean Square Error(MSE)   | $E(W, b) = \frac {1} {N} \sum_{i=1}^N (\hat{y_i}-y_i)^2$ | 단순히 실제 값과 예측 값의 차이를 제곱한 뒤 평균을 낸 오류 함수. 매우 작거나 매우 큰 값이 있으면 오차가 커진다는 문제가 존재한다. |
| Mean Absolute Error(MAE) | $E(W, b) = \frac {1} {N} \sum_{i=1}^N \|\hat{y_i}-y_i\|$ | MSE와 같으나 제곱 대신 절대값을 씌어 평균을 내는 함수                                             |
| Cross-entropy            | $E(W, b) = -\sum_{i=1}^m \hat{y_i} \log p_i$             | 분류 문제에서는 일반적으로 결과가 각 객체에 대한 확률 분포로 나오기 때문에 확률 분포의 오차를 확인하기 위해 가장 많이 사용되는 함수. |
## Optimization Algorithms
Error function으로부터 측정된 오류를 줄일 수 있도록 weight와 bias를 조정해 나가며 학습을 진행한다. 모델의 궁극적인 목표는 오차를 최소화하는 것이기 때문에 어떤 optimization algorithm을 선택하고, 어떤 방식으로 적용할 것 인지가 매우 중요하다. 

최적화는 현재의 오차가 오차함수의 global minima가 되도록 weight과 bias를 조정해 나가는 과정을 말한다. 모든 최적화 알고리즘의 기본 아이디어는 gradient descent(경사 하강법)이다. 
### Batch gradient descent(BGD)
가장 기본적인 형태는 Batch gradient descent로, 모든 입력 데이터에 대해 오차를 계산한 후 그 오차를 통해 경사 하강법을 진행하는 것이다. 가장 기본적이고, 초기 형태의 최적화 방법이다. 여기서 최적화를 할 때 중요한 요소는 step direction(gradient)와 step size(learning rate)이다. gradient는 기울기를 측정함으로써 결정되고, learning rate는 hyper-parameter로 연구자가 결정하는 값이다. learning rate는 클 수록 한번에 이동하는 정도가 커지고, 작을수록 이동하는 정도가 달라진다. 이를 적당하게 결정하는 것이 중요하다.

Batch gradient descent를 사용하는 경우 각 학습이 끝난 뒤 weight을 업데이트하는 과정은 다음과 같다.$$\Delta w_i = -\alpha \frac{dE}{dw_i}$$Batch gradient descent는 다음과 같은 문제점이 존재한다.
1. 모든 오차 함수가 간단한 형태가 아님으로, 여러 개의 local minima가 존재할 수 있다. 이로 인해 global minima를 찾는 것이 매우 오래 걸리거나 불가능할 수 있다.
2. 각 학습 단계 마다 모든 입력 데이터를 사용해 오차를 적용하므로, 계산 속도가 느리고, 비용도 비싸다.

![|400](Pasted%20image%2020240419151033.png)
### Stochastic gradient descent(SGD)
이를 해결하기 위해 등장한 것이 Stochastic gradient descent이다. 이 방법은 전체 데이터에 대해서 오차를 측정하는 것이 아니라 특정 데이터를 선택해 오차를 계산해 경사 하강법을 진행하는 것이다. 이 방법을 사용하는 경우 local minima에 빠졌을 때 다시 나올 수 있지만, global minima을 못 찾을 수도 있다. 하지만 항상 SGD는 batch GD보다 빠르고 좋은 성능을 나타내는 것이 특징이다.

![|400](Pasted%20image%2020240419151058.png)
### Mini-batch gradient descent(MB-GD)
BGD와 SGD의 사이에 존재하는 방법으로, 한 학습에 사용되는 데이터를 mini-batch 크기로 나눈 뒤 각 mini-batch가 종료될 때마다 오류를 측정해 최적화를 진행하는 방법이다. MB-GD를 사용하는 경우 벡터 연산을 사용할 수 있어 일반적으로 SGD에 비해 계산 성능이 향상됩니다. 
## Backpropagation
Backpropagation은 각 학습의 마지막 단계로, 실제로 weight과 bias를 업데이트하는 과정이다. 출력 레이어부터 시작해 입력레이어까지 차례대로 수행되어 backpropagtion이라는 이름이 붙여졌다. 각 perceptron에서 다음과 같은 연산이 수행된다. $$W_{\text{new}} = W_{\text{old}} - \alpha(\frac{\partial Error}{\partial W_x}) = W_{\text{old}} - \Delta w$$
매 변화량 계산마다 편미분이 사용되기 때문에 연산이 복잡하고, 결과를 얻기 힘든데, chain rule을 이용해 계산하면 쉽게 계산할 수 있다.

![|400](Pasted%20image%2020240419152247.png)
