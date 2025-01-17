# 在 M/D/1 排隊系統下披薩店能賺取之最大收益

411086034 羅廣翔
## 前言
假設你是一家披薩店的負責人，你想讓每位客人收到的披薩盡可能熱騰騰，而且也想讓員工在固定時間做好披薩及運送，這樣在沒有客人時員工可以休息，而忙碌的時候客人還是能盡量快速拿到餐點。
綜合上述，本報告主要在分析在 M/D/1 排隊系統下，披薩從製作完成到送達客人之間的時間間隔，並藉由披薩新鮮度來說明時間對披薩的影響。通過一系列數學推導來確定配送時間的概率上限，從而算出最好的客人下訂率以達最大利益。


## 系統模型

如果披薩放置太久才送出，客人可能會抱怨披薩不夠熱，所以，我們關心每次披薩送達的時候，哪一次經歷了最長的等待時間。假如披薩到達時間比某個門檻（比如 30 分鐘）還長，就會被視為「配送失敗」。

系統模型描述
1. 客人下訂時間 : $A_{i}$：訂單 *i* 下訂的時間，我們假設客人下訂時間間隔是隨機的，且符合 Poisson 分佈，平均到達率為 $λ$（每單位時間的平均訂單到達數量）。
$A_{i}=\sum_{j=1}^{i} T_j$ &nbsp;&nbsp;&nbsp; 其中 $T_{j}$是下訂間隔時間，服從指數分佈，平均值為 $\frac{1}{λ}$。

1. 服務時間 : 製作披薩及配送時間固定為 $T_{s}$(本報告設為10分鐘)。

2. 送達時間 $D_{i}$ : 披薩 $i$ 到達客人的時間，根據隊列邏輯： $D_{i}=max(D_{i-1},A_{i})+T_{s}$ </br>
$D_{0}$設為0，意思是當第 $i$ 個披薩送達會第i個訂單下訂時間(沒有客人在排隊)加上製作運送時間或是前一個訂單送達時間(因為在這單送達前就在排隊，所以是送完這單就要馬上做)再加上製作運送時間。

1. 等待時間 : $W_i$ 代表第i個披薩所需等待時間，可以用以下式子表達 : $W_i=max(0,D_{i-1}-A_i)$ ，意思是當現在訂單較多，我們會出現訂單排隊的現象，這時訂單 $i$ 需要等待訂單 $i-1$ 配達之後才能開始做，這段等候的時間就會用前一個訂單送達減去現在這份訂單下訂時間(0是因為沒有客人排隊，馬上就製作披薩)。

2. 服務人員 : 製作披薩與外送總共1人，而且遵循先到先服務（First-Come-First-Served, FCFS）策略。

3. 我們假設系統的穩定性，意思是客人訂單下訂速度要比製作和運送時間慢，也可以用 $ρ$ (員工忙碌率，定義為 $ρ=λT_s$ ，因為 $λ$ 為每單位時間的平均訂單到達數量，而 $T_s$ 為製作加送的時間，相乘完如果大於1代表訂單訂購速率大於製作加送的時間，而長期來看一定會導致訂單都會配送失敗，所以要保持 $ρ< 1$ )說明。

## 問題構想
我們的目標是確定當送了無限的披薩會獲得最佳利益的接訂單速率為多少，而本報告預設時間 30 分鐘為門檻時間(超過視為失敗)，員工製作與運送時間設為10分鐘，看披薩是否能夠準時送達，送達就會獲得100元利益，而失敗就會損失300元，由以上假設，我們需要計算就會變為在設定門檻下超過門檻的機率，再用接訂單效率去衡量賺取最多的錢。

## 分析
首先，根據上述的假設，可以把問題寫成 $P[A_∞\geq x]=P[W_∞+T_{s}\geq x]$ (長久下來等待時間加上製作、配送時間即為客人總共等待時間)。

再來因為我們預設配送失敗時間 $x$ 一定會小於製作與運送披薩的時間(要是大於的話每次配送皆會是配送失敗，假設預計等待時間為30分鐘，但是製做加運送要35分鐘那就一定失敗)，所以可以改寫 $P[A_∞\geq x]=P[W_∞\geq x-T_{s}]$ 。

接下來分析 $P[W_∞\geq x-T_{s}]$ ，我們先令 $x-T_{s}$ 為$w$即可改寫為 $P[W_∞\geq w]$ ，接著我們可以套用排隊理論中的 Pollaczek-Khinchine 公式 : 對於M/G/1系統，平均排隊等待時間 $E[W_q]= \frac{λ⋅𝐸[S^2]}{2(1-ρ)}$ ，其中， $𝐸[S^2]$ 是服務時間的平方的期望值， $𝜌=𝜆/𝜇$ 是系統的利用率，𝜇是服務率，接者將我們模型代入( $S$ 為常數 $T_{s}$ )可得 $E[W_∞]= \frac{λ⋅T_{s}^2}{2(1-ρ)}$ 而又 $λ⋅T_{s}=ρ$ 所以化簡為 $E[W_∞]= \frac{ρ⋅T_{s}}{2(1-ρ)}$ ，這時我們已經算出等待時間的平均值了，接著又因為 M/D/1 模型下每個訂單到來時間( $W_i$ )皆為Poisson分佈所以其 $\mu$ 會為期望值的倒數: $\mu=\frac{2(1-ρ)}{ρ⋅T_{s}}$ ，最後，我們的目標是算出 $P[W_∞\geq w]$ ，由Poisson隨機變數的CDF即可知道: $P[W_∞\le w]=1-e^{-\mu w}$ ，用1去扣掉即可得: $P[W_∞\ge w]=1-(1-e^{-\mu w})=e^{-\mu w}$ ，就可以得到超過門檻的機率了。

接者，我們代入設定好的數字 $T_{s}=10,x=30$ ， $e^{-\mu w}=e^{-\frac{2(1-λ⋅T_{s})}{λ⋅T_{s}^2}⋅w}=e^{-\frac{2(1-λ⋅10)}{λ⋅10^2}⋅20}=e^{-\frac{2-20λ}{5λ}}$ 

</br>
</br>
</br>
</br>

## 模擬
### 圖一:可以看到模擬與理論相彷
![圖1](https://github.com/Jeff5870/RP_final/blob/dd21b7919d041c76b4e182a301c95b90f2a5068f/%E8%9E%A2%E5%B9%95%E6%93%B7%E5%8F%96%E7%95%AB%E9%9D%A2%202024-12-20%20203427.png)


</br>
</br>
</br>
</br>
</br>
</br>
</br>
</br>

### 圖二:可以看到最好的顧客下訂頻率為0.0531次/分鐘，而且平均每分鐘賺4.69元
![圖2](https://github.com/Jeff5870/RP_final/blob/dd21b7919d041c76b4e182a301c95b90f2a5068f/%E8%9E%A2%E5%B9%95%E6%93%B7%E5%8F%96%E7%95%AB%E9%9D%A2%202024-12-20%20203443.png)


## 結論
其實最佳來客率在給定的條件下，即製作與配送時間固定為 10 分鐘，失敗門檻為 30 分鐘，我們計算出可以最大化披薩店收益的下訂率 
𝜆(店鋪能夠最大化收益，平衡客戶滿意度與員工工作量)，我們通過數學推導得出，成功配送的機率可以表示為  $𝑒^{−𝜇𝑤}$，其中 𝜇 代表了系統的衰減率，接著再用Pollaczek-Khinchine 公式計算出平均等待時間，然後用模擬得出最佳結果。
此次報告收穫最多的是一開始以為 $𝑒^{−𝜇𝑤}$ 的 $𝜇$ 就是一開始所設的 $λ$ ，代入才發現好像不大對，接者一步一步慢慢了解，才知道錯誤在哪裡。

## 參考文獻
指數分布- 維基百科，自由的百科全書 <https://zh.wikipedia.org/zh-tw/%E6%8C%87%E6%95%B0%E5%88%86%E5%B8%83>
M/D/1 queue<https://en.wikipedia.org/wiki/M/D/1_queue>
Large deviations theory<https://en.wikipedia.org/wiki/Large_deviations_theory>
Pollaczek–Khinchine formula<https://en.wikipedia.org/wiki/Pollaczek%E2%80%93Khinchine_formula>
Real-Time Status: How Often Should One Update?<https://www.winlab.rutgers.edu/~gruteser/papers/sanjitnew.pdf>
Age-of-Information (AoI)<https://webpages.charlotte.edu/aarafa/age-of-information.html>
On the Outage Probability of Peak Age-of-Information for D/G/1 Queuing Systems<https://www.researchgate.net/publication/333254194_On_the_Outage_Probability_of_Peak_Age-of-Information_for_DG1_Queuing_Systems>

</br>
</br>
</br>
</br>

無法正常觀看:<https://github.com/Jeff5870/RP_final/edit/main/README.md>
