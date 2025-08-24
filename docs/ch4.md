
## 說明

在 `ns-3` 目錄中有用來自動執行模擬器 `autorun` 腳本，此腳本在 `ndnSIM/ns-3/src/ndnSIM/examples/analysis` 也保有備份。

## bash function

* `update_config()`
    * 修改 `test-config.txt` 以配置實驗參數。
    * 每兩個參數對應一組實驗參數，例如 `update_config "frequency" "10"` 表示設定請求頻率為每秒10個封包。
* `run_waf()`
    * 執行 `./waf`。
    * 可以給予參數，如 `run_waf "--run=test-inline"`。
* `output_result()`
    * 呼叫 `output` bash 腳本，輸出實驗結果。

## 範例


1. 設定 Log 輸出規則
```
NS_LOG=ndn.Producer:ndn.Consumer
```
2. 以 `update_config()` 設定新實驗的參數配置。
```
# content store size experiment initial config
update_config "frequency" "10" "s" "1.2" "content" "250"

# cs-size=20
update_config "cs-size" "20" "policy" "lru"
```
3. 給予 `waf` 模擬參數。
```
run_waf "--run=test-inline"
```
4. 設定模擬結果存放檔案名稱。
```
output_result "cs20_inline"
```