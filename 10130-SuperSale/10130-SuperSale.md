# UVa-10130 SuperSale #

**題目鏈結：https://onlinejudge.org/external/101/10130.pdf**

---

## 題目概要與重點 ##
* 首先於第一行輸入為一個整數T，代表Case的數量。(1 ≦ T ≦ 1000)
* 接著輸入N表示目前有N種商品正在進行特價。(1 ≦ N ≦ 1000)
* 接下來需輸入N行，每行都會包含兩個數字P與W。
* P表示價格(Price)。(1 ≦ P ≦ 100)
* W表示重量(Weight)。(1 ≦ W ≦ 30)
* 然後現在有一個家庭共G個人。(1 ≦ G ≦ 100)
* 每個人都有不同的背包重量上限MW。(1 ≦ MW ≦ 30)
* 最後輸出那一家人總共能拿走多少價值的商品。

## 解題思路與策略 ##
* 此題為演算法題型中背包問題的類似題。
* 在此使用DP與01背包的想法去解題。
* 可以設dp[MW]=k表示重量上限MW的背包裝的物品最多價值k。
* 將每個人視為一個背包，去對所有物品做背包問題，然後將得到的最高價值給總和起來即是答案。

---

## 範例輸入與輸出 ##
### Sample Input ###
```
2
3
72 17
44 23
31 24
1
26
6
64 26
85 22
52 4
99 18
39 13
54 9
4
23
20
20
26
```
### Sample Output ###
```
72
514
```
---

## ACCEPT程式碼 ##

### C++ ###

```c++
#include <iostream>
#include <cstdio>
#include <algorithm>
using namespace std;

int T, N, G, MW;
int price[1005], weight[1005];
int dp[35];

int Knapsack()
{
    if (dp[MW]) return dp[MW];
    int dp[35] = {0};
    for (int i = 0; i < N; ++i) {
        for (int j = MW; j-weight[i] >= 0; --j) {
        //迴圈順序必須從大到小，才能保證考慮 MW 的時候，所有 < MW 的地方仍未掃過
            dp[j] = max(dp[j], dp[j-weight[i]] + price[i]);
        }
    }
    return dp[MW];
}

int main()
{
    scanf("%d", &T);
    while (T--) {
        scanf("%d", &N);
        for (int i = 0; i < N; ++i)
            scanf("%d %d", &price[i], &weight[i]);
        scanf("%d", &G);

        fill(begin(dp),end(dp), 0);
        MW = 30;
        Knapsack();

        int result = 0;
        while (G--) {
            scanf("%d", &MW);
            result += Knapsack();
        }
        printf("%d\n", result);
    }
}
```

---

## 程式執行結果 ##
<img width="338" alt="image" src="https://user-images.githubusercontent.com/100191575/172055608-284a0d4e-a647-4118-8a49-9e991bd4a365.png">
