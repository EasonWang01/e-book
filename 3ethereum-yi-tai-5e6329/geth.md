# Geth

為Golang寫成的Ethereum客戶端指令介面。

# 安裝

[https://geth.ethereum.org/downloads/](https://geth.ethereum.org/downloads/)

![](/assets/432.png)或是可參考其他安裝方法，包含使用Docker執行Geth

![](/assets/2.png)

> 如果是自己從原始碼編譯，記得要將bin資料夾加入電腦的環境變數，才能從Terminal之直接執行Geth

# 使用

Geth提供的API endpoint包含RPC HTTP endpoint、Websocket endpoint、unix socket \(Unix\) 或 named pipe \(Windows\)之IPC endpoint可使用。

輸入

```
geth console
```

即可進入 geth 的指令介面

> 也可以先在A terminal輸入geth，然後開啟另一個terminal輸入geth attach，開啟console

![](/assets/234324234.png)

之後再這邊可以輸入指令，指令列表可參考

[https://github.com/ethereum/go-ethereum/wiki/Management-APIs\#list-of-management-apis](https://github.com/ethereum/go-ethereum/wiki/Management-APIs#list-of-management-apis)

![](/assets/435345.png)

# Geth啟動參數

Geth再啟動時可以在指令上加上flag參數，讓Geth使用不同的狀態執行。

e.g.

```
geth.exe --fast --cache 1024

// --fast意思為只下載區塊鏈上的狀態而不下載所有區塊鏈上所有的資料，可以快速地進行同步，但只限於第一次同步時輸入
// --cache=1024 意思為設定電腦上的RAM保存區塊鏈上的資料最大快取為1024MB
```

有關所有指令可參考:

[https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options](https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options)

或是輸入

```
geth help
```

## --testnet、--rinkeby 、--dev

```
--testnet      連線到 Ropsten network，為 proof-of-work 測試網路
--rinkeby      連線到 Rinkeby network，為 proof-of-authority 測試網路
--dev          連線到私有鏈，方便開發者快速開發，並且預設好一個帳號並擁有一定數量之Ether，為 proof-of-authority 測試網路
```

## --syncmode

> 以前的--fast與--light需要改為如下

其影響主要為同步區塊鏈之總大小不同。

```
--syncmode "fast" 
//  節點同步選項，可以選擇如 ("fast", "full", 或是 "light")
```

## --rpc、--ws、--ipc

--rpc

```
--rpc                   啟用 HTTP-RPC server ，預設只會啟用IPC
--rpcaddr <value>       設定 HTTP-RPC server 的監聽IP   
--rpcport <value>       設定 HTTP-RPC server 的監聽PORT 預設為 8545
--rpcapi <value>        設定可用的 HTTP-RPC interface  e.g. personal, admin
--rpccorsdomain <value> 設定可跨域存取的網址
```

-ws

即為Websocket

```
--ws                     啟用 WS-RPC server
--wsaddr <value>         設定WS-RPC server listening interface (預設為: "localhost")
--wsport <value>         設定WS-RPC server listening port (預設為: 8546)
--wsapi <value>          設定可用的 WS-RPC interface  e.g. personal, admin
--wsorigins <value>      設定可跨域存取的網址
```

--ipc

```
--ipcdisable          不啟用 IPC-RPC server ，預設為自動啟用
--ipcpath             IPC socket/pipe 之檔案名稱
```

> --port 可以改變節點監聽之port
>
> Ethereum預設有**listener \(TCP\) **與 **discovery \(UDP**\) 監聽在**30303，可使用--port改變預設**

## --datadir、--keystore

```
--datadir <資料夾路徑>
--keystore <資料夾路徑>
--networkid <數字>
```

--datadir可以指定區塊鏈存放之資料夾位置，如果沒指定的話，預設位置如下

```
Mac: ~/Library/Ethereum
Linux: ~/.ethereum
Windows: %APPDATA%\Ethereum
```

--keystore指定帳戶之金鑰存放位置，預設在datadir資料夾內

# 將metamask的帳號import到Geth

先到metamask將private key取出

![](/assets/螢幕快照 2018-01-12 上午9.17.50.png)

然後進入到Geth指令介面後輸入如下：

```
personal.importRawKey("<Private Key>","<New Password>")
```


