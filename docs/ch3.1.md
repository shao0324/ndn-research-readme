## 說明

本節說明收集實驗數據的 Python 腳本。

目錄 `analysis` 中有許多 python 腳本用於計算、整合模擬數據，此處簡易說明每一腳本的功用。若使用到 ndnSIM 提供的 Tracer 工具，相關使用方法見官方網站說明：[Obtaining metrics](https://ndnsim.net/current/metric.html)

### 腳本存放位置

```
ndnSIM
└── ns-3
    └── src
        └── ndnSIM
            └── examples
                └── analysis
```

## 腳本功能說明

* ```average-delay-time.py```
    * **收集資料：資料平均取得延遲時間**，消費者發出請求至取得內容所需耗費的平均時間。
	* 從 Application-level trace helper 產生的紀錄加總 ```FullDelay``` 欄位中的數值後，除以請求總數，得到請求需耗費的平均時間。
	* 結果另外儲存於 ```average-delay-time.txt```。
* ```average-hops-count.py```
	* **收集資料：平均跳數**，消費者發出請求至保有內容的節點，所需經歷的平均跳數。
	* 從 log 中的 consumer 產生封包時記錄 Interest nonce，此後每次在 log 裡看到 ```onIncomingInterest``` 時都在對應的 Interest nonce +1 跳數，最後將所有請求所需跳數除以請求總數。
	* 結果另外儲存於 ```average-hops-count.txt```。
* ```hit-rate.py```
	* **收集資料：快取命中率**，每次請求由快取滿足的比例。
	* 從 log 中的 consumer 產生封包時記錄 Interest nonce，此後每次在 log 裡看到 ```onContentStoreHit``` 時都在對應的 Interest nonce 記錄為 True，最後將 True 數量除以請求總數。
	* 結果另外儲存於 ```hit-rate.txt```。
* ```network-traffic.py```
	* **收集資料：網路流量**，記錄 InInterest、OutData、Vicinity 三項數值。
	* InInterest、OutData 從 Packet-level trace helpers 記錄中，加總所有 netdev 的對應數值，InInterest、OutData 皆須使用 Type 為 PacketRaw 的數值。
	* 輸出結果至 ```network-traffic.txt```，依序為 InInterest、OutData、Vicinity。
* ```integrate-result.py```
	* 將所有實驗數據統整至 csv 檔中。
	* 輸入值為 csv 檔名：results_{輸入值}.csv
	* rand_seed + policy 為鍵值。

### output bash

由於模擬器必須在 ns-3 目錄下執行，因此在該目錄中加入 output bash 以執行上述所有 python 腳本。bash 檔在此目錄下也保有備份，將其複製到 ns-3 中可方便使用。

指令：

* ```--delaytime```：執行 ```average-delay-time.py```。
* ```--hopscount```：執行 ```average-hops-count.py```。
* ```--hitrate```：執行 ```hit-rate.py```。
* ```--traffic```：執行 ```network-traffic.py```。
* ```--all={arg}```：執行上述全部腳本，```arg``` 為 ```integrate-result.py``` 所需輸入值。