# Random User

## 專案執行方式
- 使用 firebase 做專案的部署，網址如下  
https://random-user-71e11.web.app/
- 把專案拉到本地端後，npm ci + npm run dev 即可在本地端執行此專案
## 專案中遇到的困難、問題與解決方法
- 第一次實作到要在前端紀錄使用者目前所在的分頁、顯示狀態等資料，原先想說是不是可以用 cookie 或 localstorage 去儲存，但怕說會有儲存空間不夠的問題(尤其是 cookie 印象中只能儲存4kb的容量)，再加上自己之前沒有實作過 IndexedDB 的經驗，所以決定在這個專案中試著使用 IndexedDB，順便當作學習個經驗。找尋解決方法的方式是 google + chatgpt，後來採用的是網路上人家分享寫好的套件 IDB-Keyval。
