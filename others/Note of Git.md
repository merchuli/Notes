# The Learning Note for Git

> 2019.11
>
> 過去都使用 GUI 介面來操作 Git ，像是 Sourcetree，最近開始想直接用 command line 來學習操作 Git ，所以開始這個文件



## Overview

常用指令



## 常用指令

- add

  ```
  // 把文件放到 stage
  git add <file_name>
  
  // 加入全部文件到 stage
  git add .
  
  // 一個文件部分移交
  git add -p <file_name>
  // https://gitbook.tw/chapters/using-git/stage-hunks.html
  // Stage this hunk [y,n,q,a,d,j,J,g,/,s,e,?]? s 可以用 s ，就會把一個 hunk 再切成更小的部分
  ```

  

- commit

  ```
  git commit -m "The content you want to commit"
  ```

  

- merge

  ```
  
  ```

  

- delete

  ```
  // delete local branch
   git branch -d <branch_name>
   
   // delete remote branch (讓遠端跟本地同步 -> 一起刪除)
   git push origin :<branch_name>
  ```

  

- stash

- ```
  // 儲存
  git stash
  
  // 儲存成什麼名字
  git stash save "<stash_name>"
  
  // 列出所有 stash
  git stash list
  
  // 提取想要的 stash
  git stash apply stash@{0}
  
  // 捨棄不需要的 stash
git stash drop stash@{1}
  
  // 提取想要的 stash 並刪除
    git stash pop stash@{0}
  
  // 查看想要的 stash
  git stash show -p stash@{2}
  ```



- cherry-pick（待補）

  ```
  
  ```



- log

  ```
  // 列出 log 紀錄
  git log
  
  // 列出 log 紀錄，只顯示一行
  git log --oneline
  
  // 圖形化 log
  git log --graph
  ```



- branch

  ```
  // 查看現在在哪個分支，以及本地端有哪些分支
  git branch
  
  // 以 current_branch 為 base 建 new_branch
  git checkout -b <new_branch> <current_branch>
  
  // 將 local 的 new_branch 上傳到 remote
  git push -u origin <new_branch>
  
  // 建立 local new_branch 並與遠端連接
  git checkout origin/new_branch -b new_branch
  ```



- fetch（待補充更多）

  ```
  // 將遠端儲存庫的最新版下載回來
  git fetch
  ```



- reset（待補充更多）

  ```
  // 恢復到上一步，[scenario]: 剛才的 commit 後悔了，想要拆掉重做
  git reset HEAD^
  
  // 預設為 mixed 模式，commit 拆出來的檔案會留在工作目錄
  // https://gitbook.tw/chapters/using-git/reset-commit.html
  ```



- rebase（待補充）  

  ```
  // 有好幾個操作行為
  git rebase -i 
  
  // - reword 改 commit 訊息
  // - edit 還沒試成功
  // - fixup 有試成功
  
  // 跳出 rebase mode
  git rebase --abort
  
  // https://gitbook.tw/chapters/rewrite-history/change-commit-message.html
  // 會常常搭配 git push -f 處理
  ```



- config

  ```
  // show git user name
  git config user.name
  
  git config --global user.name "John Doe"
  git config --global user.email johndoe@example.com
  
  // 再次提醒，若你有傳遞 `--global` 參數，只需要做這工作一次，
  // 因為在此系統，不論 Git 做任何事都會採用此資訊。
  // 若你想指定不同的名字或電子郵件給特定的專案，只需要在該專案目錄內執行此命令，並確定未加上 `--global` 參數。 
  // Reference[1]: https://alvinalexander.com/git/git-show-change-username-email-address
  // Reference[2]: https://git-scm.com/book/zh-tw/v2/%E9%96%8B%E5%A7%8B-%E5%88%9D%E6%AC%A1%E8%A8%AD%E5%AE%9A-Git
  ```

  

 ## Reference

https://blog.darkthread.net/blog/my-git-cheatsheet/

