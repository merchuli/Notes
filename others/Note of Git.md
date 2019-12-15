# The Learning Note for Git

> 過去比較常用 GUI 介面來操作 Git ，像是 Sourcetree，最近開始想直接用 command line 來學習操作 Git ，所以開始這個文件



## Overview

常用指令



## 常用指令

- stage

  ```
  // 把文件放到 stage
  git add <file_name>
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
   
   // delete remote branch
   git push origin :<branch_name>
  ```

  

- stash

- ```
  // 儲存
  git stash
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

  

- cherry-pick

  ```
  
  ```

- log

  ```
  // 列出 log 紀錄
  git log
  // 圖形化 log
  git log --graph
  ```

- branch

  ```
  // 以 current_branch 為 base 建 new_branch
  git checkout -b <new_branch> <current_branch>
  
  // 將 local 的 new_branch 上傳到 remote
  git push -u origin <new_branch>
  ```

  