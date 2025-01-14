*在開始前你需要熟悉的概念：地址偏移，指標*

**目標：學習記憶體的佈局**

請翻開課本[第 14 頁](
http://www.cs.bham.ac.uk/~exr/lectures/opsys/10_11/lectures/os-dev.pdf)<sup>1</sup>，並查看記憶體佈局的圖表。

在本章節中你只需要知道 boot sector 的位置。

很明顯地，BIOS 會把 boot sector 放在 `0x7C00` 位置，然而為了加深各位的印象，以下我們會進行一個小實驗。

我們會在 boot sector 添加一個 X 並嘗試打印在螢幕上。以下會使用 4 種不同的呼叫方式，請實驗並思考各個方案的成功/失敗原因。

**打開文件 `boot_sect_memory.asm`**

首先加入一個 X ：
```nasm
the_secret:
    db "X"
```

然後嘗試以多種不同的方式訪問 `the_secret`：

1. `mov al, the_secret`
2. `mov al, [the_secret]`
3. `mov al, the_secret + 0x7C00`
4. `mov al, 2d + 0x7C00`，其中 `2d` 是二進位檔案中 'X' 字節的實際位置

編譯並執行後你應該會看到類似 `1[2¢3X4X` 的字符串，其中 1 和 2 之後的字節只是隨機垃圾數據。

如果你在 asm 中添加或刪除了指令，請記得自行修正 `0x2d` 偏移量。

請保證你完全理解了 boot sector 的偏移量和記憶體地址後再繼續閱讀下一節的內容。

全局偏移量
-----------------

很明顯地在所有指令中都加入 `0x7c00` 的偏移將會非常不便而且不切實際，
而組譯器也貼心地提供了 `org` 命令來指定一個「全局偏移量」：

```nasm
[org 0x7c00]
```

現在你可以打開 **`boot_sect_memory_org.asm`** ，
這裡包含了在 boot sector 打印數據的標準方式，
你可以重新編譯並執行，觀察 `org` 命令如何影響各種存取記憶體的方式。

詳細了解更改內容（包括有無 `org`）的說明，請閱讀註釋。

-----

[1] 本教程是以該書為基礎而制作。有關詳細信息，請閱讀根目錄中的 README 文件。
