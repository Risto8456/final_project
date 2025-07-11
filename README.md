# 可程式邏輯設計-期末專題-排隊號碼&櫃台叫號模擬
## test 開頭的檔案是我自己測試用的，可以刪掉
## 有做參數化設計，於 top_tb.v 中有 3 個數值可做調整
1. DT_SZ : 資料位元數
    + 每位客人的編號、時間資料的數值大小限制，預設 4 bits，0 ~ 15
    + 4 bits 方便波形圖用 16 位元表示
2. DEPTH : FIFO 深度
    + 隊列 queue 最多可排幾位客人，預設為 3
    + 若客人數到達限制，則新客人則不會進入 queue
    + 若調整 DEPTH，PTR_W 須一併調整
    + PTR_W : queue 內部指標暫存的 bit 數
      + queue 內部資料儲存方式為圓環儲存，節省位移操作，因此會需要"讀指標"與"寫指標"
      + ISE Design Suite 14.7 版本較舊，無法使用 clog2 指令，只能額外設定參數
      + 計算 : PTR_W = ceil(log2(DEPTH+1))，ceil 為無條件進位至整數
      + 若 DEPTH 為 4 (最多可存 4 位客人)，指標有 5 種狀態(01234)，需要 3 bits 來存指標
3. CNTER : 櫃檯數量
    + 每個櫃台都會有自己的 num 與 time output 腳位，但腳位數量應該要是固定的，無法更改
    + 因此先將所有櫃台的 num 與 time 分別串接成 num_bus 與 clk_bus，再交給 top_tb.v 做切片處理
    + 若 CNTER 更改，top_tb.v 第 32 行的程式碼也需做註解調整
    + 另外，若腳位有做更改，需重新開啟波型顯示器，不然可能會出錯當掉
