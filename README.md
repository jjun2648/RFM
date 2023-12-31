# RFM
### 요약
쇼핑몰 데이터를 이용해 RFM분석, 매출 추이 분석, 결제 방법에 따른 결제 방법을 분석한다. RFM은 소비자의 구매 패턴을 분석하여 분류함으로써 고객과의 상호작용을 극대화한다. 그럼으로써 효과적으로 개선점을 찾고 고객 유지율을 높일 수 있다. 또한 매출 추이와 결제 방법에 따른 결제 금액 분석을 통해 고객들이 언제 어떤 상품을 찾는지 알아본다. 이를 통해 지속적인 수익 증대를 기대할 수 있을 것이다. 

### 서론
쇼핑몰 데이터를 통해 다음의 3가지를 분석하고자 한다.
1. 소비자 RFM 분석  
  RFM 분석을 통해 RFM이 각각 높고 낮음에 따라 2*2*2 즉 8가지 조합으로 나타낼 수 있다. 이들을 그래프를 이용해 한 눈에 알아볼 수 있게 나타내고 주목할 만한 점과 개선해야 할 점을 찾아본다.<p>
1. 매출 추이 시각화  
  a. 연도별 매출, b. 월별 매출, c. 할부를 고려한 월별 매출을 각각 시각화해 연도별 매출 추이, 월별 매출 추이, 할부 결제 비중이 높은 달을 알아본다.<p>
1. 결제 방법에 따른 결제 금액 분석  
  여러 결제 방법에 따른 결제 금액의 경향성을 알아본다. 이후, 결제 방법을 신용카드, 후불, 정기결제의 신용성 결제와 현금, 포인트, 마일리지 등의 현금성 결제로 나누어 그 둘을 비교해 본다. 신용성 결제가 현금성 결제에 비해 결제 금액이 클 것으로 가정하고 가설 검정을 진행한다.

### 방법
1. 전처리  
   업체명, 상품명, 결제 방법 열의 경우 결측치가 있는 행을 삭제하였다. 업체명 열의 ‘지니 태블릿(후불집행)’은 ‘지니 태블릿’ 업체와 같은 업체라고 판단하여 업체명을 고치고 결제 방법에 ‘후불’을 추가하였다. 또한 주문 수량이나 판매금액 열의 값에 0이 있는 경우 행을 삭제하였다. 제작 문구 내역 열은 대부분이 결측치이고 값이 있더라도 개인정보가 다수 포함되어 열 전체를 삭제하였다. 할부기간 열의 결측치는 일시불 결제로 판단되어 값을 일시불로 채웠다. 결제방법 열은 여러 결제방법이 함께 기재되어 있어 주 결제방법인 첫 번째 글자로 대체하였다. <p>
1. RFM 분석  
   전처리된 데이터 중 처리상태 열의 경우 실제 구매된 건이 아닌 ‘주문취소’, ‘미결제’, ‘환불요청’ 등의 취소 내역이 포함되어 있어 제거해주었다. 다음으로 ‘업체명’ 별로 최신 주문 일자, 총 판매 금액, 주문 빈도를 구해 새로운 데이터프레임으로 만들었다. 이후 최신 주문 일자, 총 판매 금액, 구매 빈도를 평균 이상/이하로 분류하여 각각 R, F, M으로 나타내고 이들을 8가지 조합으로 시각화했다.<p>
1. 매출 시각화  
   1. 월별, 연도별 매출  
     실제 매출만을 가지고 시각화를 해야하기 때문에 이번에도 취소 내역이 제거된 데이터 프레임을 사용하였다. 주문일자 열에서 연-월 데이터만 추출한 뒤 월별, 연도별 매출을 시각화하였다.
   1. 월별 순수익(할부기간 고려)  
     마진 등을 계산하지 않았지만 할부 기간을 고려하여 월별 매출을 조정한 데이터를 만들었다. 이후  3-a의 데이터와 함께 그래프로 시각화하였다.<p>
1. 결제 방법에 따른 결제 금액 분석  
   이번에는 처리상태 열을 따로 분류하지 않고 유지한 상태로 분석을 진행하였다. 결제방법과 판매금액만을 따로 추출하여 scatterplot을 통해 대략의 추이를 알아보았다. 이를 통해 가장 큰 차이를 보이는 신용성 그룹(신용카드, 후불, 정기결제)과 현금성 그룹(가상계좌, 무통장입금, 포인트, 적립금, 웰컴마일, 현금간편결제)을 나눠 정규성 검사, t-test, 시각화를 진행하였다.

| ![workflow](https://github.com/jjun2648/RFM/assets/50532905/8a0a29c8-569f-413f-be88-09e0f5cab8e8) |
|:--:|
| <b> Workflow </b> |

### 결과
| <img src="https://github.com/jjun2648/RFM/blob/main/images/01_table.png"> |
|:--:|
| <b> [Table 1] 업체명 기준 RFM 데이터 프레임 </b> |
업체명을 기준으로 최신 주문 일자, 총 판매 금액, 구매 빈도를 평균 이상/이하로 분류하여 각각 R, F, M 열에 True/False로 나타냈다. 다만 총 판매금액과 빈도는 이상치로 인해 평균값이 너무 커져서 로그 스케일로 변환한 뒤 평균값을 냈다.

| <img src="https://github.com/jjun2648/RFM/blob/main/images/01_bar.png"> |
|:--:|
| <b> [Figure 1] 고객 분류 그래프 </b> |
표 1의 RFM을 각각 T/F에 따라 8가지로 분류했다. Best(R↑F↑M↑), First time(R↑F↓M↓), Uncertain(R↓F↓M↓), Churn(R↓F↑M↑), Spenders(R↓F↓M↑), Shopper(R↑F↑M↓), Valuable(R↑F↓M↑), Frequent(R↓F↑M↓) 순으로 나타났다.

| <img src="https://github.com/jjun2648/RFM/blob/main/images/02_pie.png"> |
|:--:|
| <b> [Figure 2] 고객 분류 파이 차트 </b> |
고객 분류 데이터를 파이 차트와 비율로 나타냈다. 이탈 고객 그룹이 10%에 달하는 것에 주목해야 한다.

| <img src="https://github.com/jjun2648/RFM/blob/main/images/03_year_bar.png"> |
|:--:|
| <b> [Figure 3] 연도별 매출 추이 </b> |
한 눈에 보아도 증가하는 추세를 확인할 수 있다. 2019년은 12월 중순부터 보름 정도의 기간만 집계되었기에 1년치로 보정해주었다.

| <img src="https://github.com/jjun2648/RFM/blob/main/images/04_bar_month.png"> |
|:--:|
| <b> [Figure 4]  월별 매출 추이 </b> |
대체로 평이하지만 3, 5, 10월이 높은 것을 확인할 수 있다.

| <img src="https://github.com/jjun2648/RFM/blob/main/images/05_bar_installment.png"> |
|:--:|
| <b> [Figure 5] 월별 매출 추이(할부 고려) </b> |
할부를 고려한 월별 매출 추이를 함께 나타냈다. 10월 매출이 가장 큰 폭으로 떨어진 것으로 보아 10월에 가장 많은 할부 결제가 있었음을 알 수 있다.

| <img src="https://github.com/jjun2648/RFM/blob/main/images/06.png"> |
|:--:|
| <b> [Figure 6] 결제 방법에 따른 판매 금액 </b> |
결제 방법에 따른 건 별 판매 금액을 점을 찍어 나타냈다. 5백만원 이상의 고액은 신용카드, 후불, 정기 결제로만 결제된 것을 확인할 수 있다.

| <img src="https://github.com/jjun2648/RFM/blob/main/images/07_density.png"> |
|:--:|
| <b> [Figure 7] 판매 금액에 따른 밀도 그래프 </b> |
판매 금액의 분포를 정규화하기 위해 판매 금액을 로그 스케일로 변환시킨 그래프이다. 정규 분포로 보이지 않고 표본이 많아 shapiro-wilk 정규성 검정을 진행할 수 없다.

| <img src="https://github.com/jjun2648/RFM/blob/main/images/08_density.png"> |
|:--:|
| <b> [Figure 8] 1% 표본에 대한 밀도 그래프 및 정규성 검정 결과 </b> |
 그래프는 여전히 정규분포로 보이지 않고 shapiro-wilk 정규성 검정도 p-value가 0.05 이하로 나와 정규분포를 만족하지 않음을 알 수 있다.

| <img src="https://github.com/jjun2648/RFM/blob/main/images/09_boxplot.png"> |
|:--:|
| <b> [Figure 9] 신용성 거래와 현금성 거래 간의 판매 금액 차이에 대한 box plot 및 scatter plot </b> |
1% 표본의 결제 방법을 신용성 거래(신용카드, 후불, 정기 결제)와 현금성 거래(그 외)로 나눠 두 그룹 간의 판매 금액 차이를 확인해보았다. 두 그룹의 건 당 판매금액으로 box plot과 scatter plot을 그리고 mann-whitney test를 진행했다. P-value=9.121e-07로 두 그룹 간의 판매 금액 차이를 확인할 수 있다.

### 논의
1. RFM 분석  
   [그림 1]과 [그림 2]를 보면 8가지 고객 분류 중 Best(R↑F↑M↑), First time(R↑F↓M↓), Uncertain(R↓F↓M↓), Churn(R↓F↑M↑) 순으로 높은 것을 볼 수 있다. Best와 First time이 높은 것은 긍정적인 신호지만 로그를 씌워서 평균을 내는 자의적인 방법으로 분류를 했기 때문에 결과가 낙관적으로 나왔을 가능성이 있다. 둘 중 더 주목해야 할 것은 First time인데 이 신규 고객들 바로 떠나지 않고 빈도와 구매 금액을 높일 수 있도록 만드는 것이 쇼핑몰의 매출 증가에 큰 도움이 될 것이다.  
반면 Uncertain과 Churn이 높은 것은 부정적인데, 특히 Churn같은 경우에는 빈도와 구매 금액이 모두 높았던, 즉 구매력이 있는 고객들이었기 때문에 이들의 이탈은 매우 뼈아프다. 경쟁사의 데이터가 없기 때문에 상대적으로 높은지 낮은지 알 수는 없지만 10%에 가까운 고객이 이탈하는 일이 반복된다면 매출에 상당한 악영향을 주게 될 것이다. 그렇기 때문에 기존의 충성 고객, 즉 가장 많은 비중을 차지하는 Best 그룹에게 혜택을 주어야 할 근거로 활용될 수 있다.<p>
1. 매출 시각화  
   [그림 3]을 보면 연도별 매출이 증가하고 있는 것을 볼 수 있다. 2020년에서 2021년, 2021년에서 2022년으로 갈 때 증가하는 절대치는 유지되고 있지만 비율로 보면 줄어들고 있는 것이기 때문에 성장이 정체될 수도 있다는 점을 염두에 두어야 한다.  
[그림 4]를 보면 월별 매출은 3, 5, 10월에 가장 높은 것을 볼 수 있다. 그런데 [그림 5]를 보면 할부를 고려한 월별 매출은 조금 달라진다. 10월의 매출은 할부결제의 비중이 매우 높아 할부를 고려하기 전에는 3위였던 매출이 9위까지 내려앉은 것을 볼 수 있다. 그 이유를 생각해 보면 [표 2]의 상위 매출 상품에 태블릿이 많은 것과 고액 결제를 할 때 할부 결제를 이용한다는 점을 떠올릴 수 있다. 10월부터 새로운 학년을 위해 학습 프로그램과 결합된 학습용 태블릿을 구매한다고 예상해볼 수 있을 것이다.<p>
1. 결제 방법에 따른 결제 금액 분석  
   먼저 신용을 이용한 결제는 당장 값을 치루지 않고 나중에 낼 수 있는 만큼 고액 결제를 위해 이용할 것이라는 가설을 세워볼 수 있다. [그림 6]을 보면 실제로 신용카드, 후불, 정기 결제에서만 500만원 이상의 고액 결제가 이루어지고 있는 것을 볼 수 있다. [그림9]를 보면 P-value가 9.121e-07로 매우 낮게 나와 이 가설이 실제로 채택할 수 있는 가설이라는 것을 알 수 있다. <p>
1. 한계 및 개선점  
   RFM 분석에서 고객을 분류하는 기준을 세우는 데 있어서 BIRANT, Derya.(2011)의 연구를 참고하여 K-means clustering algorithm 을 통해 기준을 세워볼 수 있을 것이다. 그렇게 된다면 지나치게 낙관적인 결과가 나오지 않는 것 뿐만 아니라, 논문 속의 데이터(터키의 스포츠 매장 및 온라인 스토어)와도 고객 분류 비율을 비교하여 쇼핑몰의 상황이 긍정적인지 부정적인지 파악하는데 도움이 될 수 있다.   
고객 식별 번호가 없어 주문 번호 혹은  업체명으로 대신 분석해야했기 때문에 RFM 분석이 정확하지 않다는 한계가 있다. 주문번호를 통한 분석은 한 고객이 여러 번 주문한 것을 알 수 없기 때문에 RFM 중 F(빈도)를 알 수 없어 분석이 불가능했고, 업체명을 통한 분석은 사실상 그 입점 업체에 대한 고객 집단 분석이었다. 또한 ‘지니 태블릿’과 같이 F, M은 높지만 R(최근성)이 낮은 경우 고객들이 외면한 것인지 입점 업체에서 빠진 것인지 정확히 알 수가 없다.   
또한 고객의 방문 횟수 데이터가 없어 ‘방문 빈도’ 대신 ‘구매 빈도’로 대신해야 했다는 한계도 있다. 고객 방문 횟수로 RFM 분석을 진행했다면 R과 F는 높지만 M이 낮은 고객, 즉 자주 방문하지만 구매를 망설이는 고객의 수를 분석해낼 수 있기 때문에 가격 조절이나 프로모션 진행 등에 대한 근거로 활용할 수 있었을 것이다. 



