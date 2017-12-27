# Bloom Filter

上一章節提到的SPV節點可以減少在節點上所需要下載的區塊鏈大小，而 Bitcoin在 BIP37 引入了Bloom Filter 機制，可以讓節點利用此機制發出過濾請求，得到想要的資訊。

> BIP37可參考如下連結：
>
> [https://github.com/bitcoin/bips/blob/master/bip-0037.mediawiki\#filter-matching-algorithm](https://github.com/bitcoin/bips/blob/master/bip-0037.mediawiki#filter-matching-algorithm)

### Bloom Filter 概念

Bloom Filter 很早之前即開始使用在各種場景，1970年由 『  Burton Howard Bloom 』提出。

當我們想判斷一個元素是不是在一個集合裡時，可以通過Array、Tree、Hash Table等資料結構實現，但他們的時間複雜度分別為  
 O\(n\) 、  O\(log n\) 與  O\(n/k\)，且在效率最好的Hash Table中又必須儲存鍵值\(Key與Value\)，而Bloom Filter不需要在資料中多存鍵值並且其時間複雜度可以達到O\(k\)。

### Bloom Filter 實現原理

> ![](/assets/螢幕快照 2017-12-27 下午9.16.png)[https://en.wikipedia.org/wiki/Bloom\_filter](https://en.wikipedia.org/wiki/Bloom_filter)

看到上圖，一個Bloom filter初始化時是一個固定大小長度的Array，含有m個欄位，每個欄位的值均為0，以及k個hash function

上述的k和m的數量會根據目標的False Positive \(註1\) 機率而改變與調整。

#### 步驟一：

假設今天有一個元素要加入Bloom filter，我們會先讓他先分別經過k個hash function，之後會產生k個值，把這些hash後的數字對照array上對應的位置，將該位置的值從0改為1。

以上圖範例為例，x經過hash function後會產生三個值，分別對應到圖中第二，第六和倒數第五的位置，所以將著些位置的值改為1

之後y和z也是一樣步驟，經過同樣的三個hash function後把對應的位置數值改為一。

#### 步驟二：

接著我們看到圖中，發現x,y,z分別將一些位置重複的改為一，這時就埋下了未來產生False Positive的機率。

所以我們可以想到將Array長度拉長即可減少這種事情發生，但也會增加資料的基本儲存空間。

#### 步驟三：

假設今天 w 的值要來確認他有沒有在Bloom filter中，他也是一樣經過三個hash function，然後查看產生出的hash對應到array的三個位置的值是否為1，如果為1即可假定w在先前已經加入過這個資料集裡面。

但如果w今天不在Bloom filter裡面，但是剛好三個對應的位置的值均為1，也就是先前剛好這三處剛好被其他的值更改為1，則會產生剛才所說的False Positive。

範例:

可使用此模組：[https://github.com/jasondavies/bloomfilter.js/](https://github.com/jasondavies/bloomfilter.js/)

```js
var bloom = new BloomFilter(
  32 * 256, // 分配的Array大小
  16        // 具有多少個hash functions.
);

// 新增元素到Bloom filter.
bloom.add("foo");
bloom.add("bar");

// 測試資料是否在Bloom filter中.
bloom.test("foo");
bloom.test("bar");
bloom.test("blah");
```

#### 比特幣應用：

比特幣中的Bloom filter包含三個相關的Control messages

```
1. FilterLoad
2. FilterAdd
3. FilterClear
```

比特幣的SPV節點將會建立Bloom filter並且利用filterload訊息將其發送給Full Node，filter中會設定其需要哪些交易資訊，並且可用filteradd訊息添加資料，而不必重新發送整個Bloom filter，而filterclear是用來將Bloom filter機制取消，變回原本的區塊交換訊息的方式。

而[SPV client](https://bitcoin.org/en/glossary/simplified-payment-verification)不只可以再Bloom filter加入交易，還可以加入[public keys](https://bitcoin.org/en/glossary/public-key)  、  [signature scripts](https://bitcoin.org/en/glossary/signature-script) 與 [pubkey scripts](https://bitcoin.org/en/glossary/pubkey-script) 等等。

在比特幣的SPV client中可以將False Positive的機率提高，來增加更多的隱私，因為將會Full Node會回傳更多不相關的交易資料，更好的隱藏SPV client真實想要的交易資料。

---

#### 註1：False Positive 與 False Nagative

False Positive:  答案中表示該處是有資料的，但其實該處沒有資料。

False Nagative: 答案中表示該處沒有資料，但其實該處有資料。
