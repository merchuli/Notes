# 前後端分離注意事項_jq

**Feb. 21, 2018 @ Senao**

## Overview

以隱形賣場(VIP)前後端分離的案例，說明 jq 版前後端分離應注意事項。



## Outline

[TOC]

## 資料夾架構

- 專案全部放在 [feature] 資料夾下，[feature] 資料夾不可省略，因為從 online 的網址，設定只要第一層吃到 「feature」就會指向放在 S3 bucket 上的 jq 專案，所以為方便指向，需要多一層資料夾封裝。
- [feature] 資料夾下分為
  - images, 放圖片
  - javascripts, 放 js 檔
  - stylesheets, 放 css 檔
  - vip, html 檔, 隱賣專用。若未來有 event site 的需求，請在 feature 底下新增 eventSite 資料夾
- [javascripts] 資料夾下有
  - pages, 放置頁面所需求的 js 檔
  - vendor, 第三方套件的 js 檔
  - env.js, 環境檔, 根據當下網址判斷是哪個環境，並指定要串接 API 的位址
  - config.js, 設定檔
  - const.js, 常數檔

> 要注意 env, config, const 載入有順序性



## jq 版前後端分離須注意事項

### Render HTML

- 使用 template render html


- template 用 script 包裝起來
- 形式是 script type="text/html" id="template-xxxxx"
  - type 是 html
  - 使用 id 區別各個 template，方便後續以此取得該 template 的 html
- 把 template 要填入的資料挖空，以雙大括號包起來，例如：{{VARIABLE}}


- 使用在 helper.js 定義 replaceTemplate 將挖空的地方做變數替換




### CORS(Cross-Origin Resource Sharing)，跨域

#### 同源政策(Same-Origin policy)

- 同通訊協定(protocal)
  - http / https => 不同協定


- 同通訊埠號(port)
  - :8080 / :4200 => 不同 port


- 同主機位址/網域(domain)
  - event.senao.com.tw / online.senao.com.tw



#### 呼叫端

- 加入自定義的標頭檔
  - header 的 ContentType 指定為 application/x-www-form-urlencoded; charset=UTF-8
  - xhrFields: {**withCredentials**: true}

>CORS 的 Content-Type 標頭可允許的值，此處使用 application/x-www-form-urlencoded
>
>- `application/x-www-form-urlencoded`
>- `multipart/form-data`
>- `text/plain`



#### 被呼叫端

- 加上 header('Access-Control-Allow-Origin: xxxx' ), xxxx 為允許存取的來源

>此語法以 PHP 為例



### Serialize(序列化)

PHP 接收 POST 時，需要先經過序列化處理

1. 從 js 傳遞參數到 php 時，會自動序列化
2. 但如果是從 angular 過去的話，需要自行序列化




## Reference Link

[1]

[Cross Domain Issue](https://paper.dropbox.com/doc/cross-domain-issue-t59EQzEOlFW5qbWmSTl55)