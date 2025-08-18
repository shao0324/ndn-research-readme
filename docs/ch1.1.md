# 系統環境與安裝ndnSIM

## 系統環境

* 虛擬機：VMware Workstation 17 Pro
    * 安裝檔 + 序號：https://github.com/201853910/VMwareWorkstation
    * Netlab NAS 備份路徑：\●SOFTWARE\VMware Workstation
* 作業系統：Ubuntu 20.04.6 LTS 64-bit PC (AMD64) desktop

### 虛擬機配置

* Number of processors: 4
* Number of cores per processor: 1
* Memory: 16 GB (16384 MB)
* Network Type: NAT
* Maximum disk size: 50 GB

## 安裝及編譯 ndnSIM

此處說明如何在 Ubuntu 20.04.6 版本安裝及使用 ndnSIM 2.9，macOS 使用者請自行參照說明文件：[ndnSIM 2.9: Getting Started](https://ndnsim.net/2.9/getting-started.html)。若使用當下有更新的版本，請至[當前版本說明](https://ndnsim.net/current/getting-started.html)。

### 系統相依

以下相依套件不在光碟中，請依指令自行下載。

1. Core dependencies
```
sudo apt install build-essential libsqlite3-dev libboost-all-dev libssl-dev git python3-setuptools castxml
```

2. Dependencies for NS-3 Python bindings
```
sudo apt install gir1.2-goocanvas-2.0 gir1.2-gtk-3.0 libgirepository1.0-dev python3-dev python3-gi python3-gi-cairo python3-pip python3-pygraphviz python3-pygccxml
```
```
sudo pip3 install kiwi
```

### 模擬器原始碼

ndnSIM 有三個主要構成：
1. **ns-3**：依 ndnSIM 需求調整的 ns-3 版本，用以驅動模擬器。
2. **PyBindGen**：可以使用 python 撰寫模擬腳本，以及模擬時產生圖像介面。
3. **ndnSIM**：使用 ndn-cxx 和 NFD 完成 NDN 行為模擬。```--recursive``` 會自動抓取對應的 ndn-cxx 和 NFD 版本。
    * ndn-cxx library (ndnSIM 2.9 指定 ndn-cxx 0.8.0)
    * NDN Forwarding Daemon (NFD) (ndnSIM 2.9 指定 NFD 22.02)

### 下載官方原始碼

```
mkdir ndnSIM
cd ndnSIM
git clone https://github.com/named-data-ndnSIM/ns-3-dev.git ns-3
git clone https://github.com/named-data-ndnSIM/pybindgen.git pybindgen
git clone --recursive https://github.com/named-data-ndnSIM/ndnSIM.git ns-3/src/ndnSIM
```

### 本論文的原始碼

#### 直接使用

將光碟中的 `ndnSIM` 目錄放置在 Ubuntu 作業系統中的任意位置即可編譯並執行，在 `ns-3` 中有包含協助自動執行、收集結果的 `bash`，將在後面如何模擬時說明怎麼使用。

#### 替換官方原始碼

如果你已經下載官方原始碼，要將官方原始碼替換成《結合內容評分與共享機制的命名資料網路快取策略》的模擬器原始碼，請對照以下結構替換 `src` 下的 `ndnSIM` 中全部檔案。

```
ndnSIM
├── ns-3
│   └── src
│       └── ndnSIM // 替換這層目錄
└── pybindgen
```

用於自動執行、收集結果的 `bash` 屬於個人化輔助工具，若有需要再複製過去。

### 編譯 ndnSIM

執行 ```ns-3``` 目錄中的```waf``` 會檢查程式碼變更並編譯模擬器。
```
cd ns-3
./waf configure --enable-examples
./waf
```

首先，需要使用 `./waf configure` 配置模擬器參數，主要使用的配置：

* `--enable-examples`：編譯後產生 scenario 範例，範例位置在 `/ndnSIM/ns-3/build/src/ndnSIM/examples`。<font color=red>建議開啟</font>。
* `--build-profile`：程式編譯設置，這裡只說明 `debug`、`optimized` 兩種選項。
    * `--build-profile=optimized`：最佳化程式碼，官方建議選項，移除不必要的訊息，若未主動設定，預設使用最佳化。
    * `--build-profile=debug`：輸出更多資訊以便 debug，例如開啟 `NS_LOG_*` 功能以記錄資訊。<font color=red>建議使用 debug 以方便收集資訊</font>。
* `--disable-python`：ndnSIM 使用 python 提供視覺化介面但不穩定或可能無法使用。<font color=red>基本上用不到視覺化界面，如果碰到問題就停用</font>。
* `--help`：更多配置選項。

!!! info
    第一次編譯需要花較長的時間，請耐心等候。