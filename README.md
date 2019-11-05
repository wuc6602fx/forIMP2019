# forIMP2019
本文件包含五張流程圖，輔助說明投稿中雜湊時間鎖與混和模式的文字內容。
以下流程圖將以Alice、Bob分別為以太坊和Corda的使用者。場景為Alice要用美元(cash)，交換Bob在Corda上的汽車(car)。
圖1為雜湊時間鎖的作法，理論上，若兩個timeouts的值(T1,T2)設定適當，HTLC方法是可以順利執行原子交換（atomic swap）。
<br><br>
<p align="center">
  <img width="800" height="300" src="https://github.com/wuc6602fx/forIMP2019/blob/master/1.png">
  <br>圖 1：雜湊時間鎖作法
</p>
<br><br>為了確保交換應用的雙方能達成銀貨兩訖，在我們的實作架構中，兩個區塊鏈平台都透過智能合約(下圖的Asset SC)實現資產的雜湊鎖與時間鎖的功能，並設立仲裁人服務(下圖的Arbitrator)。若先被上鎖的資產其鎖定時間長度需足以讓新擁有人能夠成功解鎖資產，即使新擁有人嘗試解鎖失敗後，仍可通知仲裁人介入，並有足夠時間進行仲裁，執行資產之強制交換，如圖2；若先被上鎖的資產時間鎖長度非常長，如果對方對應的資產已經timeout而rollback，則其單一資產鎖定就失去了意義。這時可以請仲裁人介入處理，讓先被上鎖的資產得以提早rollback回原擁有人，如圖3。
<br><br>
<p align="center">
  <img width="800" height="600" src="https://github.com/wuc6602fx/forIMP2019/blob/master/2.png">
  <br>圖 2：雜湊時間鎖 - 強制交換
</p>
<br><br>
<p align="center">
  <img width="800" height="600" src="https://github.com/wuc6602fx/forIMP2019/blob/master/3.png">
  <br>圖 3：雜湊時間鎖 - 強制返還
</p>
<br><br>最後，我們提出一種新的混合模式來支援：以雜湊時間鎖為基礎，加入搭配中繼模式舉證程序的有限度仲裁功能。舉例而言，仲裁人應使用者要求介入時，除了查詢Corda上紀錄之資產狀態外，也需請求Corda公證人就查詢結果簽署數位簽章。接著仲裁人方能附上此交易證據，對以太坊智能合約提出執行強制處理的要求；以太坊端的智能合約配合修改，僅在驗證過仲裁人所附的數位簽章後，才會尊循其強制處理資產（返還或交換）的指令，如圖4、5。
<br><br>
<p align="center">
  <img width="800" height="600" src="https://github.com/wuc6602fx/forIMP2019/blob/master/4.png">
  <br>圖 4：混合模式 - 強制交換
</p>
<br><br>
<p align="center">
  <img width="800" height="600" src="https://github.com/wuc6602fx/forIMP2019/blob/master/5.png">
  <br>圖 5：混合模式 - 強制返還
</p>
