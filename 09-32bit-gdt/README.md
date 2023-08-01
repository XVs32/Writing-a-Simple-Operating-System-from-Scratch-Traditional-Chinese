**在開始前你需要熟悉的概念：GDT**

**目標：編寫 GDT（Global Descriptor Table）**

還記得第 6 課的 segmentation 嗎？

在 32 位保護模式下，segmentation 的運作方式有所不同。在 GDT 中，偏移量被包含在 Segment Descriptor 中，SD 負責定義基址（32 位元）、大小（20 位元）以及一些旗標，例如唯讀、權限等。詳細請打開 os-dev.pdf 檔案，查看第 34 頁的圖或參考 GDT 的維基百科頁面。

編寫 GDT 最簡單的方式是定義兩個 Segment，一個用於程式碼，另一個用於資料。Segment 之間可以重疊，同時也意味著並沒有記憶體保護，但對於 booting 來說沒有也沒差，在後續的高階程式碼再解決這個問題也不晚。

需要注意的是，為了位址管理上的便利，第一個 GDT 的位址必須是 `0x00`。

此外，CPU 需要透過「GDT 描述子」（GDT Descriptor）來載入 GDT，其中包含實際 GDT 的大小（16 位元）和位址（32 位元）。而 GDT Descriptor 可以通過 `lgdt` 指令載入。

請閱讀本節課的 GDT 程式碼。關於 flag 的詳細資訊，請參考 os-dev.pdf 文件。

在下一課中，我們終於會切換到 32 位保護模式並測試這幾節課的程式碼。