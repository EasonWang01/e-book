# 相關密碼學介紹

密碼學被廣泛應用在現今的應用中，例如晶片、信用卡、電子郵件加密傳輸、身分認證、遠端電腦連線等等。

在現代許多協定均有使用到密碼學來進行加解密的動作，例如 : HTTPS、PGP、IPsec、SSL / TLS、SSH。

本章節將介紹雜湊函式（ Hash ）、對稱式加密、非對稱式加密，並且會以 OpenSSL 與 Node.js 進行範例實作。

---

## OpenSSL

[https://www.openssl.org/](https://www.openssl.org/)

可使用以下指令來查看OpenSSL版本

```
openssl version -a
```

OpenSSL為一個開放原始碼的函式庫套件，用C語言寫成，類Unix系統已經內建在裡面，而Windows使用者可以用Git Bash來執行。

> OpenSSL計劃在1998年開始，目標是發明一套自由的加密工具，其包含目前大部分的主流加密演算法。

查看所有可用指令與可用加密法

```
openssl help
```

![](/assets/9444.png)

而我們也可以輸入以下指令，測試電腦執行相關加密演算法時的性能與耗時

```
openssl speed
```

> 其他指令可參考:
>
> [https://wiki.openssl.org/index.php/Command\_Line\_Utilities](https://wiki.openssl.org/index.php/Command_Line_Utilities)

## Node.js Crypto模組

[https://nodejs.org/api/crypto.html](https://nodejs.org/api/crypto.html)

可使用以下指令來查看Node.js crypto模組

```
node -p crypto
```

取得可用Hash function

```
node -p crypto.getHashes()
```

取得可用之對稱式加密方法

```
node -p crypto.getCiphers()
```



