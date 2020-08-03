## 

그림1 


배경 : 

따라서 Class Imbalnace Problem이 심한 상태이다. 

우리는 다른 타입의 유전체 데이터를 종합해서 사용시, 적은 샘플 사이즈의 클래스에 대해서 

### 방법 



따라서 


imbalnace 문제를 해결하기 위해서는 크게 3가지의 전략이 사용된다. 1번쨰는 Data-level methods , 2번재는 Algorithm-level methods ,  3번째는 Hybrid methods. 우리는 이중 Data-level methods의 관점에서 접근해 보려고 한다. 

## 문제정의 

TCGA의 33가지 암종 데이터의 mRNA , Methylation , mRNA + Methylation 
Featrue engineering , Sampling , Ensemble-Method 의 접근방법이 많이 쓰인다. 
그중에서 



## 데이터 살펴보기 
TCGA의 RNA와 메틸레이션이 같이있는 데이터의 클래스는 24종류의 Multi-class 문제이기 때문에, 클래스분포의 Skew정도를 보기위해 ID(Imbalance Degree)를 측정해 보았다. 


Class imbalance 문제는 1) balanced problem ,  2) unbalnaced problem showing multi-majority ,3) a multi-minority unbalanced problem 세가지로 분류됩니다. 어떤 분류에 속하는지에 따라서 검증 방법이나 문제점 극복을 위한 방법이 달라집니다. 
그리고 데이터세트는 3)에 속하는것을 ID 값으로부터 확인 했다.  

우리는 





현재 사용하는 데이터세트의 



값은 0.1 정도로 나왔고, 불균형 정도가 심한것을 알 수 있다. 

각 클래스별 샘플 수의 히스토그램은 아래와 같다.  

데이터에 대한 Imbalnce의 정도를 파악하기 위해 Shannon entropy , IR 을 계산해 보았다. 



---



## 데이터 
TCGA에서 
샘플 수는 총 10361개로 각 클래스별 샘플 카운트는 다음과 같습니다.

<p align = "center">
<img src = "https://github.com/nchos88/DataContainer/blob/master/RNA_MTL/test.png?raw=true" width = "800" height = "400">
</p>

눈으로 살펴봐도 class imbalnace가 보이기에 ID(Imbalance Degree)를 측정해 보았습니다. Class imbalance 문제는 1) balanced problem ,  2) unbalnaced problem showing multi-majority ,3) a multi-minority unbalanced problem 세가지로 분류되는데, ID 값으로 어떤 문제에 속하는지 알 수 있습니다. 값은 11.56으로 unbalnaced problem showing multi-majority 와 a multi-minority unbalanced problem 중간정도로 imbalance 한 것을 확인했습니다. 

우리는 이러한 class imbalnce 문제를 해결하기 위해서 data-level methods의 접근방법을 선택했고, sampling이 아닌 여러 타입의 데이터로 feature selection으로 모델의 예측 성능을 개선하는 전략을 취했습니다. 물론 sample이나 algorithm-level methods 또는 hybrid methods 기반의 전략도 좋지만, 이번에는 논외로 합니다. 

## Feature 선택
mRNA는 약 2만개 , 메틸레이션은 약 48만개 정도가 있습니다. 우리는 이중 1000개 에서 5000개 까지 선택을 한 후에,  같은 숫자의 feature 갯수 시 어떤 타입의 데이터가 예측하는데 유리한지 , 그리고 얼만큼의 feature 수 가 최적인지를 알아보려고 했습니다. 

이때 선택 기준은 데이터 값 자체의 variance , xgboost 및 lightgbm의 그라디언트 부스팅 모델의 트리 생성시 사용하는 엔트로피값 기준, 그리고 Support Vector Machine에 L1 패널티를 적용하여 피쳐의 선정을 하고, 이것을 weighted voting 방식으로 선택했습니다. 


## 모델
Imbalance 데이터에는 Bagging 및 Boosting 기반의 알고리즘이 잘 동작한다고 알려져 있습니다. [1] 따라서 Bagging 방식의 Random Forest , Boosting방식의 LightGBM , Neural Net 을 가지고 실험 해 보았습니다.  
각각의 모델 파라미터는 다음과 같습니다. 


## 검증방법 
모델 검증 방법으로는 Micro Precision-Recall Curve를 사용해서 비교를 하였습니다. 
결과는 다음과 같습니다 .


---


## 실험 결과 



## 결론 



--------


[1] He, H., and Garcia, E. A. Learning from imbalanced data. IEEE Trans. on
Knowledge and Data Engineering 21, 9 (Sep. 2009), 1263–1284.
[2] Wang, S., and Yao, X. Multi-class imbalance problems: Analysis and potential
solutions. IEEE Trans. on Systems, Man and Cybernetics - Part B 42, 4 (Aug.
2012), 1119–1130.

