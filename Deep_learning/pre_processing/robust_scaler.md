#### 2022.09.07

# Robust scaler

## _Introduction_

난 평소에는 데이터 정규화를 위해 sci-kit learn 에서 제공하는 min-max scaler를 많이 사용한다. 그러나, 연구 중 outlier가 많은 데이터를 다루게 되었는데, min-max scaler를 사용하니 outlier를 제외한 나머지 값들이 매우 작은 수 (1e-4 정도)로 정규화 되는 문제가 있었다.

이를 해결하기 위해 "outlier, data normalization" 이라는 키워드로 구글링 해보니 robust scaler가 검색되었다. 빠르게 훑어보니 다른 정규화 방법에 비해 outlier에 강하기 때문에 안정적으로 정규화가 가능하다고 한다. 애초에 이름 자체가 robust scaler 이니 얼마나 안정적일까!

## _Robust scaler_

robust sacler는 데이터의 Interquartile Range (IQR)을 사용한다. 데이터의 하위 25 % 값에 해당하는 Q1, 중간값에 해당하는 Q2, 상위 25 % 값에 해당하는 Q3를 통해 데이터를 정규화 하는데, 데이터 x에 대해 다음과 같이 나타낼 수 있다.

> norm_x = (x - Q2) / (Q3 - Q1)

이는 중간 값 Q2를 기준으로 하여 50 % 의 데이터가 포함되는 Q1 ~ Q3 으로 데이터를 정규화한다는 의미이다. 이를 통해, 정규화의 기준이 Q1, Q3를 크게 벗어나는 outlier의 영향을 받지 않게 되는 것이다.

## _Is the Robust scaler robust?_

처음 "robust scaler는 outlier로 부터 안정적이다." 라는 문구를 보고 sci-kit learn 라이브러리로 무작정 실험해보았을 때에는 robust scaler를 사용하면 정규화가 진행되면서 outlier들 역시 일정한 수준으로 (일반적인 값들과 비슷하게) 감쇄되리라고 기대했다. 그러나, 실험을 해보자 outlier들이 그대로 남아있었기에 당황했다. 왜 그런지 이해하기 위해 공식을 찾아보니 상술한 식과 같았다. robust scaler는 linear한 정규화 식을 사용하기에 outlier는 당연히 그대로 남아있는 것이다.

그렇다면 outlier를 제거하지 못하는 robust scaler가 무엇이 안정적이라는 걸까? 다음은 sci-kit learn에서 공식 문서[1] 에 나와 있는 실험이다.

#### 원본 데이터

    ![](https://velog.velcdn.com/images/sontneptm/post/3aeafa0b-ea5e-4069-84e9-43fca7b479ed/image.PNG)

## _Conclusion_

#### _Ref._

[1] https://scikit-learn.org/stable/auto_examples/preprocessing/plot_all_scaling.html#sphx-glr-auto-examples-preprocessing-plot-all-scaling-py
