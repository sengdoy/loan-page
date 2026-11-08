# 放款狀態頁 包覆頁（loan-page）

用途：**遮掉 Google Apps Script 最頂那列「這個應用程式是由 Google Apps Script 的使用者建立」警語。**

- 真正的放款頁面還是 Apps Script 網頁應用程式（`/exec`），只是被這個包覆頁用 `iframe` 包起來。
- 因為警語列在 Google 自己的外殼層（另一個網域），我們的頁面不可能動它，所以用「負邊距把外殼往上推出螢幕」的方法裁掉。
  寬螢幕裁 47px、窄螢幕（含手機）裁 81px（`index.html` 最上面那三段變數可以自己改）。

## 日常使用

| 位置 | 網址 |
|---|---|
| 你給訪客／自己加書籤 | `https://sengdoy.github.io/loan-page/` |
| 掛單設定頁 | `https://sengdoy.github.io/loan-page/?page=settings` |
| 放款帳號管理頁 | `https://sengdoy.github.io/loan-page/?page=admin` |
| GAS 原網址（幕後來源，不用給訪客看） | `https://script.google.com/macros/s/AKfycbz2AwTOyfcZRAKcs4eTBp5J2QhptcxOfk8Tclb-AoY9QzCpdiKsEVyVjhG7nkhJUuLU/exec` |

## 檔案

| 檔案 | 用途 |
|---|---|
| `index.html` | 包覆頁本體（GAS 網址＋裁剪偏移都在里面的「設定」區塊，改這裡就好） |
| `404.html` | GitHub Pages 的 404 導回主頁（保留 `?page=...`） |
| `robots.txt` | 不讓搜尋引擎收這個包覆頁 |

## 除錯／微調裁剪高度

網址後面加參數即可，不用改程式碼：

- `?c=0` → 完全不裁剪（可以看到 Google 原本那列警語長什麼樣子、內容從哪個像素開始）
- `?c=81` → 手動把裁剪高度改成 81px
- `?page=settings`／`?page=admin` → 直接開設定頁／管理頁（包覆頁會把 `?page` 轉給 GAS）

診斷模式（有 `?c=`）右下角會顯示「裁剪 xxpx（寬 yyy）」方便觀察。

## 更新這個包覆頁

```
cd 放款狀態頁包覆頁
git add -A && git commit -m "說明" && git push
```

push 完 Pages 通常 1～2 分鐘內生效（硬性重新整理瀏覽器：Ctrl+F5）。

## 對應的 GAS 端（v7.9.5）

`LendingView.gs` 的 `doGet` 會收 `?base=` 參數（包覆頁把自己的幕前網址餵回去），前端 `data-base` 有值時
「掛單設定／管理頁」連結就指向包覆頁網址，用戶點了不會跳到有警語列的 GAS 原網址；直連 GAS 網址時行為不變。
