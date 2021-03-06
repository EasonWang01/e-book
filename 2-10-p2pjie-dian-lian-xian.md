# Bitcoin P2P 節點連線

比特幣的節點之間互相傳遞區塊訊息，使用TCP協定，並遵循一套相同的協定互相交換資料，共同維護一份公開的分散式帳本。

> [https://bitcoin.org/en/developer-guide\#p2p-network](https://bitcoin.org/en/developer-guide#p2p-network)

比特幣的全節點 \( [mainnet](https://www.gitbook.com/book/easonwang01/e/edit#) \) 預設跑在8333的PORT上，而測試鏈 \( [testnet](https://www.gitbook.com/book/easonwang01/e/edit#) \) 預設跑在18333 PORT上。

## Bitcoin Protocol

我們可以使用『 Wireshark 』軟體來監聽節點間的封包，其為一個免費的開源軟體，常用來分析網路封包。

[https://www.wireshark.org/download.html](https://www.wireshark.org/download.html)

開啟程式後，我們在上方的綠色欄位輸入『 Bitcoin 』，讓他只過濾出 Bitcoin Protocol 的封包。

![](/assets/螢幕快照 2017-12-06 下午9.19.50.png)

比特幣的封包傳遞時是使用 TCP 連線，其 Protocol 內容如下

> ![](/assets/螢幕快照 2017-12-06 下午9.11.20.png)
>
> 紅色箭頭部分為每個比特幣節點的網路封包都會包含的訊息頭（Message Headers），而黃色箭頭部分則為根據該封包的Command Name欄位，擁有不同的Payload。

## 節點搜尋

在第一次與其他節點連接之前，會先去搜尋寫在原始碼裡面的 [DNS seeds](https://bitcoin.org/en/glossary/dns-seed)，之後節點啟動後會預設先去找DNS seed Server尋求目前可用的節點來同步資料。

> Bitcoin 訂立成為 DNS Seed Server 的需求要件：![](/assets/234.png)[https://github.com/bitcoin/bitcoin/blob/57b34599b2deb179ff1bd97ffeab91ec9f904d85/doc/dnsseed-policy.md](https://github.com/bitcoin/bitcoin/blob/57b34599b2deb179ff1bd97ffeab91ec9f904d85/doc/dnsseed-policy.md)

目前寫在原始碼的 DNS Seed ![](/assets/9876.png)[https://github.com/bitcoin/bitcoin/blob/3c098a8aa0780009c11b66b1a5d488a928629ebf/src/chainparams.cpp](https://github.com/bitcoin/bitcoin/blob/3c098a8aa0780009c11b66b1a5d488a928629ebf/src/chainparams.cpp)

當節點向 DNS Seed 發出請求後，會返回多個目前可用來同步的節點IP，之後會把可用節點記錄在本地資料庫中，避免每次啟動節點都向 DNS Seed 發送請求。

> 可使用 nslookup 或 dig 發出DNS query請求

1.使用 nslookup 指令

![](/assets/98798.png)

2.使用Dig指令![](/assets/螢幕快照 2017-12-06 下午9.38.59.png)3.使用 Node.js 程式來發出 DNS 請求

```js
const dns = require('dns');
const options = {
  family: 4,
  hints: dns.ADDRCONFIG | dns.V4MAPPED,
};

options.all = true;
dns.lookup('seed.bitcoin.sipa.be', options, (err, addresses) => {
 console.log('addresses: %j', addresses);
 console.log(addresses[0].address); //返回的IP中的第一個
});
```

## 



