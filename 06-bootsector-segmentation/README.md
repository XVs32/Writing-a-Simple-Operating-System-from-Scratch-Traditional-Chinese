*在開始前你需要熟悉的概念: segmentation*

**目標: 學習在 16位 real-mode 中以 segmentation 來定址記憶體**

如果你很熟悉 segmentation 的話，也可以直接跳過本節課。

在第三課中，我們曾透過 `[org]` 使用 segmentation。
segmentation 意味著你可以對引用的所有資料指定一個偏移量。

在實作中這是使用 `cs`（Code）、`ds`（Data）、`ss`（Stack）和 `es`（Extra，即使用者定義）幾個 reg 來完成的。

需要注意的是 CPU 會自動地，*隱式* 使用這些 reg，例如你為 `ds`設定了某個值，那麼所有的記憶體訪問都將自動根據 `ds` 的值進行偏移。[ref](http://wiki.osdev.org/Segmentation)

在計算偏移後的地址時，會把地址和偏移量*重合*在一起：`segment << 4 + address`。
例如，如果 `ds` 是 `0x4d`，那麼 `[0x20]` 實際上是指 `0x4d0 + 0x20 = 0x4f0`。

理論部分先到這邊。接下來請嘗試閱讀(和修改)範例程式碼。

提示：我們需要透過通用 ref 來實現對 cs, ds etc. 的賦值。