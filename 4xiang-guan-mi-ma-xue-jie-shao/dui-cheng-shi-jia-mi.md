對稱式加密\(**Symmetric-key algorithm**\)

在加密和解密時使用相同的密鑰，常見的對稱是加密算法有：DES、AES、Serpent、Blowfish、TwoFish等等

而下面展示一個對稱式加密最基本的原理:

```
密文: Test
密鑰: 11110011
```

我們先把密文轉換為ACSII 二進位

```js
function textToBinary(string) {
    return string.split('').map(function (char) {
        return '0' + char.charCodeAt(0).toString(2);
    })
}

textToBinary("Test");
// ["01010100", "01100101", "01110011", "01110100"]
```

上面每個陣列對應一個英文字母，我們把每個陣列元素與密鑰分別做XOR運算\(註1\):

```js
function XOR(a, b) {
 if(a.length !== b.length) {
   console.log("要求兩者二進位數字長度相同");
   return
 }
 let num = (parseInt(a, 2) ^ parseInt(b, 2)).toString(2);
 if(num.length !== a.length) {  // 因前面為0會被省略，所以要手動補0
    let padLength = a.length - num.length;
    num = "0".repeat(padLength) + num;
  };
  return num 
}

let key_ = "11110011"; // 密鑰

// 加密
(["01010100", "01100101", "01110011", "01110100"]).map(d => XOR(d, key_)); // 使用密鑰對每個元素做XOR

//加密後結果:  ["10100111", "10010110", "10000000", "10000111"]
```

然後進行解密

```js
//解密 (等於是把加密後的結果在與KEY做一次XOR)
(["10100111", "10010110", "10000000", "10000111"]).map(d => XOR(d, key_));

//解密後結果 ["01010100", "01100101", "01110011", "01110100"]

// 最後轉為字串
(["01010100", "01100101", "01110011", "01110100"]).map(d => String.fromCharCode(parseInt(d, 2))).join('');
// 得到字串:Test
```



簡化版為:

```js
var key = "test_key";
var password = "12345";
var encrypted = password ^ key;  
console.log(encrypted ^ key);
```

---

註1: \(兩兩數值相同為否，而數值不同時為真\): 例如1 xor 1為 0 ， 1 xor 0 為 1

F\(false\)亦代表0，T\(true\)亦代表1

![](/assets/2333.png)

至於為何要使用XOR在對稱式加密，不使用AND或OR，可參考:[https://stackoverflow.com/a/24731845](https://stackoverflow.com/a/24731845)
