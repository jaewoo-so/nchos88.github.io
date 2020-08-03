


## 데이터 
TCGA에서 
샘플 수는 총 10361개로 각 클래스별 샘플 카운트는 다음과 같습니다.

<p align = "center">
<img src = "https://github.com/nchos88/DataContainer/blob/master/RNA_MTL/test.png?raw=true" width = "800" height = "400">
</p>

눈으로 살펴봐도 class imbalnace가 보이기에 ID(Imbalance Degree)를 측정해 보았습니다[1]. Class imbalance 문제는 1) balanced problem ,  2) unbalnaced problem showing multi-majority ,3) a multi-minority unbalanced problem 세가지로 분류되는데[2,3], ID 값으로 어떤 문제에 속하는지 알 수 있습니다. 값은 11.56으로 unbalnaced problem showing multi-majority 와 a multi-minority unbalanced problem 중간정도로 imbalance 한 것을 확인했습니다. 

우리는 이러한 class imbalnce에 의한 작은 샘플수를 가진 클래스 예측율 저하 문제를 해결하기 위해서 data-level methods의 접근방법을 선택했고, sampling이 아닌 여러 타입의 데이터로 feature selection으로 모델의 예측 성능을 개선하는 전략을 취했습니다. 물론 sample이나 algorithm-level methods 또는 hybrid methods 기반의 전략도 좋지만, 이번에는 논외로 합니다. 

## Feature 선택
mRNA는 약 2만개 , 메틸레이션은 약 48만개 정도가 있습니다. 우리는 이중 1000개 에서 5000개 까지 선택을 한 후에,  같은 숫자의 feature 갯수 시 어떤 타입의 데이터가 예측하는데 유리한지 , 그리고 얼만큼의 feature 수 가 최적인지를 알아보려고 했습니다. 

** 피쳐 셀렉션 방법
 선택 기준은 데이터 값 자체의 variance , xgboost 및 lightgbm의 그라디언트 부스팅 모델의 트리 생성시 사용하는 엔트로피값 기준, 그리고 Support Vector Machine에 L1 패널티를 적용하여 피쳐의 선정을 하고, 이것을 weighted voting 방식으로 선택했습니다. 


## 모델
Imbalance 데이터에는 Bagging 및 Boosting 기반의 알고리즘이 잘 동작한다고 알려져 있고 많이 사용하는 방법입니다[5,6,7]. 따라서 Bagging 방식의 Random Forest , Boosting방식의 Catboost , Neural Network 를 가지고 실험 해 보았습니다. 각각의 모델 파라미터는 다음과 같습니다. 


<p align = "center">
<img src = "https://github.com/nchos88/DataContainer/blob/master/RNA_MTL/model_parameter.png?raw=true" width = "500" height = "300">
</p>



## 검증방법 
class imbalance인 데이터이기 때문에 일반적인 accuracy로는 적은 샘플의 클래스에 대한 성능 측정이 어렵기 때문에, 주 성능 지표로는 aucPR( Area Under the Precision-Recall Curve )를 사용했습니다. ( Average 방식은 Micro 사용 ) 그리고 데이터의 샘플 수 가 많지 않기 때문에 5-fold cross validation으로 성능을 측정했습니다. 

---

## 실험 결과 

다음의 테이블은 각 데이터 세트별 , 3가지 모델의 aucPR 평균 값입니다. **com = RNA + 메틸레이션 , rna = RNA , mtl = 메틸레이션** 을 나타내고 , 뒤의 숫자는 피쳐 수를 나타냅니다.


<p align = "center">
<img src = "https://github.com/nchos88/DataContainer/blob/master/RNA_MTL/model_performace_compare.png?raw=true" width = "800" height = "100">
</p>
<p align = "center">
<b> Figure 1: Compare aucPR micro average </b>
</p>
현 실험에서는 딥러닝 모델(Deep Neural Network)이 각 피쳐 수의 데이터세트에 대해서 다른 모델들 보다 높은 성능을 보이는 것을 확인했습니다.

#### 
다음은 딥러닝 모델의 각 암종별 성능을 나타낸 테이블 입니다. 파란색 음영은 각 값을 시각화 한 것이고, 빨간색 숫자는 각 암종에서 aucPR 값이 가장 높은 데이터세트를 나타냅니다. 마지막 열 **Count**는 샘플 수를 나타냅니다. 


<p align = "center">
<img src = "https://github.com/nchos88/DataContainer/blob/master/RNA_MTL/nn_result_table_best_each_dataset.png?raw=true" width = "900" height = "600">
</p>
<p align = "center">
<b> Figure 2: Compare each class aucPR across all dataset </b>
</p>
피쳐수가 1000개 일때는 RNA + 메틸레이션 조합이, 2000,3000개는 RNA+메틸레이션 데이터와 RNA데이터의 성능 이 비슷한 것을 볼 수 있었습니다. 모든 피쳐수에 대해서 메틸레이션 데이터는 다른 데이터들 보다 낮은 성능이 나왔습니다. 

#

<p align = "center">
<b> Figure 3: Compare each class aucPR in same feature count </b>
</p>
<p align = "center">
<img src = "https://github.com/nchos88/DataContainer/blob/master/RNA_MTL/nn_result_table_best_each_dataset_bycount.png?raw=true" width = "900" height = "550">
</p>

평균적인 성능은 RNA + 메틸레이션 조합의 데이터세트가 모든 피쳐수 구간에서 가장 높은 aucPR이 나왔다. 특히 피쳐수가 가장 적은 1000개시 RNA 와 메틸레이션을 같이 구성시 각 암종별 패턴이 더욱더 잘 표현되는 것을 알 수 있었습니다. 

#

다음 결과는 Micro Precision-Recall Curve , 샘플수가 가장 많은 순위 top2 BRCA , LUAD_LUAC , 샘플 수 가 가장 적은 순위 top2 UCS , DLBC를 시각화한 결과입니다. 


<p align = "center">
<b> Figure 4: Compare aucPR Value on Average</b>
</p>
<p align = "center">
<img src = "https://github.com/nchos88/DataContainer/blob/master/RNA_MTL/cat_rf_nn_compare.png?raw=true" width = "800" height = "400">
</p>

성능의 평균치를 보았을떄는 피쳐수가 많을수록 성능이 올라가는것을 확인 할 수 있었다. 

#

<p align = "center">
<img src = "https://github.com/nchos88/DataContainer/blob/master/RNA_MTL/pr_curve_nn_5000_com.png?raw=true" width = "900" height = "900" border = '1'>
</p>
<p align = "center">
<b>Figure 5: Deep Neural Network PR-Curve on RNA + Methylation , 5000 feature count</b>
</p>


---

## 결론 
TCGA의 24종 암종 데이터의 Class imbalance problem에서 작은 샘플 사이즈를 가진 클래스에 대한 예측율을 올리기 위해 RNA와 메틸레이션 데이터를 같이 쓰면 어떠한 효과가 있는지 확인해 보았습니다. 이번 실험을 통해 당므과 같은 결론은 낼 수 있었습니다. 


1. 현 실험에서는 같은 종류의 데이터를 사용시 전반적으로 딥러닝(Deep Neural Network) 모델이 성능이 가장 좋았다. 

2. 데이터의 종류로 비교를 해보면 RNA +메틸레이션 구성의 데이터가 조금더 암종 구분을 위한 패턴이 들어있다고 볼 수 있고, 암종에 따라서 RNA 또는 메틸레이션 한종류로 구성한 데이터에서 더 높은 예측률을 보일 떄도 있었습니다. 따라서 같은 피쳐 수 인 경우 모델의 전반적인 예측율을 높이는데 가장 좋은 방법은 RNA와 메틸레이션 둘다 사용하는 것이라고 볼 수 있겠습니다. 현 실험에서는 RNA와 메틸레이션의 비율은 5대5로 했지만, 최적화된 비율를 찾아낸다면 성능은 더 개선될 것입니다. 

3. 피쳐수는 많을수록 평균적인 성능이 증가했습니다. 

4.여러가지 feature selection method을 비교 및 최적화 하는 작업도 도움이 될 것입니다. 특히 이번에는 머신러닝 모델 기반의 선택 방법을 사용했지만, 도메인 지식 기반의 선택 방법도 같이 사용한다면, 좋은 결과를 이끌어 낼 수 있을 것 입니다. 



## Appendix A : Feature Importance Result
- 모든 값은 0~1 사이로 스케일링 했다. 
<p align = "center">
<img src = "https://github.com/nchos88/DataContainer/blob/master/RNA_MTL/lgb_compare_pie.png?raw=true" width = "800" height = "230" border = '1'>
</p>
<p align = "center">
<b>Figure A.1: Top7 Feature from lightgbm</b>
</p>

##

<p align = "center">
<img src = "https://github.com/nchos88/DataContainer/blob/master/RNA_MTL/xgb_compare_pie.png?raw=true" width = "800" height = "230" border = '1'>
</p>
<p align = "center">
<b>Figure A.2: Top7 Feature from xgboost</b>
</p>

##

<p align = "center">
<img src = "https://github.com/nchos88/DataContainer/blob/master/RNA_MTL/lgb_compare.png?raw=true" width = "800" height = "800" border = '1'>
</p>
<p align = "center">
<b>Figure A.3: Top50 Feature of RNA+Methylation , RNA , Methylation from lightgbm</b>
</p>

##

<p align = "center">
<img src = "https://github.com/nchos88/DataContainer/blob/master/RNA_MTL/xgb_compare.png?raw=true" width = "800" height = "800" border = '1'>
</p>
<p align = "center">
<b>Figure A.4: Top50 Feature of RNA+Methylation , RNA , Methylation from xgboost</b>
</p>
---

<p align = "center">
<b>Top50 Feature of mRNA from weighted voting</b>
</p>
<p align = "center">
<img src = "https://github.com/nchos88/DataContainer/blob/master/RNA_MTL/rna_mean_bar_top50.png?raw=true" width = "800" height = "800" border = '1'>
</p>
##
<p align = "center">
<b>Top50 Feature of Methylation from weighted voting</b>
</p>
<p align = "center">
<img src = "https://github.com/nchos88/DataContainer/blob/master/RNA_MTL/mtl_mean_bar_top50.png?raw=true" width = "800" height = "800" border = '1'>
</p>







## 앞으로 ???
추후 각 암종을 구분하는데 있어서 중요하게 작용한 피쳐들을 분석을 해보려고 합니다. 
이러한 예측에 중요하게 기여하는 피쳐를 분석할떄는 피쳐들간의 관계를 분석이 불가능 하다는 한계가 있습니다. 


#### 작업 미완 부분 
- XGboost 결과 넣기 : 약 >4.5 시간 예상됨 
- 중요도 분석 관련 의미 있으면 같이 써주기?
- 결론 방향 

- TCF21 : 암에 관련 ( In humans, TCF21 has been identified as a candidate tumor suppressor gene and is frequently epigenetically silenced in various human cancers.[7])

- 







--------

[1]Ortigosa-Hernandez, J., Inza, I., Lozano, J.A., 2017. Measuring the class- ´
imbalance extent of multi-class problems. Pattern Recognition Letters 98,
32–38.
[2] He, H., and Garcia, E. A. Learning from imbalanced data. IEEE Trans. on
Knowledge and Data Engineering 21, 9 (Sep. 2009), 1263–1284.
[3] Wang, S., and Yao, X. Multi-class imbalance problems: Analysis and potential
solutions. IEEE Trans. on Systems, Man and Cybernetics - Part B 42, 4 (Aug.
2012), 1119–1130.
[4]Blaszczynski, J., Stefanowski, J.: Neighbourhood sampling in bagging for imbalanced data. Neurocomputing 150, 529–542 (2015)
[5]Galar, M., Fernández, A., Barrenechea, E., Bustince, H., Herrera, F.: A review on ensembles for the class imbalance problem:
bagging-, boosting-, and hybrid-based approaches. IEEE Trans.
Syst. Man Cybern. Part C 42(4), 463–484 (2012)
[6]Krawczyk, B., Wo´ zniak, M., Schaefer, G.: Cost-sensitive decision
tree ensembles for effective imbalanced classification. Appl. Soft
Comput. 14, 554–562 (2014)
[7] https://en.wikipedia.org/wiki/TCF21_(gene)#Role_in_development



