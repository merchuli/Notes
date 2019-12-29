# SSH - 同一裝置多個 Git 帳號

>2019 / 12 / 15
>
>以下以 Mac 為例，其他系統猜測原理差不多，只是用法可能略有不能

假設有以下不同帳號

| Email                 | Username       | 空間      |
| --------------------- | -------------- | --------- |
| merchuli@personal.com | merchu         | GitHub    |
| merchuli@company.com  | company_merchu | GitHub    |
| merchuli@personal.com | other_merchu   | Bitbucket |



以下步驟以產生一組 SSH 為例

## Step 1 產生 SSH Key

進到 ~/.ssh `cd ~/.ssh`

```
ssh-keygen -t rsa -C "merchli@company.com"
```

>  `ssh-keygen`:  產生 ssh 的金鑰

- 會遇到第一個輸入

  ```
  // 輸入你的檔案要存的名字，通常會用 id_rsa_xxx 命名，xxx 可以自己取自己想取的名字
  Generating public/private rsa key pair.
  // 公司用就可命名為 id_rsa_work，私人就可以命名為 id_rsa_personal
  Enter file in which to save the key (/Users/XXX/.ssh/id_rsa): id_rsa_work
  ```

- 接下來會詢問要不要設定私鑰的密碼，可以直接 enter 跳過

  ```
  Enter passphrase (empty for no passphrase):
  Enter same passphrase again:
  ```

- 完成後會產生私鑰、公鑰（副檔名 pub）：

  ```
  Your identification has been saved in id_rsa_work.
  Your public key has been saved in id_rsa_work.pub.
  The key fingerprint is:
  SHA256:<一串 hash 值> merchuli@company.com
  # 此處直接複製參考資料示意
  The key's randomart image is:
  +---[RSA 2048]----+
  |  .  .     .     |
  |   E. . . o .    |
  |    *. . = - .   |
  | . + o. o . = . .|
  |. o o  .S. o . ..|
  |..    .oo o o   o|
  | . . .o=   + . ++|
  |. = . o....   o.B|
  |.+     .ooo   .=+|
  +----[SHA256]-----+
  ```


- 之後可以到 ~/.ssh/ （ `cd ~/.ssh`）即可找到私鑰（id_rsa_work）及公鑰（id_rsa_work.pub）兩個檔案



## Step 2 將公鑰放到要設定的空間

[以 Github 為例](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/)

- 複製公鑰 `pbcopy < ~/.ssh/id_rsa_xxx.pub`
- 到 setting 頁面的 **SSH and GPG keys** 選項
- 點擊 **New SSH key** 或 **Add SSH key**
- 命名 SSH 名稱，例如 Work
- 貼上剛剛複製的公鑰



## Step 3 加入設定檔

為了讓 Git 識別每一個空間對應的 key value，會需要有一個設定檔（黑魔法）

- 進到 ~/.ssh/ 資料夾 `cd ~/.ssh`

- 使用 vim 建立一個 config 檔 `vi config`

- 依照以下格式做設定

  | 輸入欄位                 | 意義                                                   |
  | ------------------------ | ------------------------------------------------------ |
  | Host                     | 連線至遠端的別稱（自己命名）                           |
  | HostName                 | 遠端的 Domain 或 IP                                    |
  | Port                     | 遠端 SSH 連接埠（預設的話可以不填）                    |
  | User                     | 使用者名稱（參照最開頭的帳號表格）                     |
  | PreferredAuthentications | 偏好使用的驗證（publickey）                            |
  | IdentityFile             | 私鑰的位置（可以是相對或絕對位置，但建議使用絕對位置） |

  範例：

  ```
  # Work
  Host work
  HostName github.com
  User git
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_rsa_work
  
  # Personal
  Host personal
  HostName github.com
  User git
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_rsa_personal
  ```
  



## Step 4 更新 SSH Agent

```
# 清除舊設定
ssh-add -D

# 加入 SSH key
ssh-add id_rsa_work
ssh-add id_rsa_personal

# 確認加入清單
ssh-add -l
```



## Step 5 測試 SSH 連線

```
ssh -T work
Hi ghPersonal! You've successfully authenticated, but GitHub does not provide shell access.
ssh -T personal
Hi ghWork! You've successfully authenticated, but GitHub does not provide shell access.
```



## 遇到狀況時的排除方法

- 用 `ssh-add -D` 指令刪除**所有** SSH Agent 管理的 key，並重新回到 Step 4 再重新加入一次
- 移除 known_hosts 檔案 `rm known_hosts`，將資料夾 ~/.ssh/ 中的 known_hosts 檔案移除並回到 Step 5 重試
- config 中的 IdentityFile 使用絕對位置
- [Always use the "git" user](https://help.github.com/en/github/authenticating-to-github/error-permission-denied-publickey#always-use-the-git-user)



## commit log 紀錄

（待補）



## Reference

[1] Wiki SSH 定義

https://zh.wikipedia.org/wiki/Secure_Shell

[2] 多個SSH Key與帳號的設定(Mac)

https://dotblogs.com.tw/as15774/2018/04/30/174737

[3] 同時使用多個 Git 帳號

http://finfin.github.io/2015/01/17/multiple-git-accounts.html