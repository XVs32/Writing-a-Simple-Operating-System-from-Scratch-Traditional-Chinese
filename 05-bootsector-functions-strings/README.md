*在開始前你需要熟悉的概念: 控制結構、函式呼叫、字串**

**目標: 學習使用組合語言編寫基本的程式碼（迴圈、函式）**

Boot Sector 的完成是近在眼前了。

我們將會在第七節課開始從磁碟讀取資料，這會是在載入核心前的最後一個步驟。但首先我們需要編寫一些具有控制結構、函式呼叫和完整字串使用的程式碼。

字串
-------

字串的定義方式與位元組相同，但以空字元結尾（跟 C 語言基本一致）。

```nasm
mystring:
    db 'Hello, World', 0
```

組譯器把單引號包含的範圍編譯為 ASCII，而獨立的零將作為位元組 `0x00`（空字元）傳遞。

控制結構
------------------

我們已經學會一個：`jmp $` 可以作為無窮迴圈使用。

而組合語言中跳轉的執行與否可以由*前面*指令的結果來決定。例如：

```nasm
cmp ax, 4      ; if ax = 4
je ax_is_four  ; do something (by jumping to that label)
jmp else       ; else, do another thing
jmp endif      ; finally, resume the normal flow

ax_is_four:
    .....
    jmp endif

else:
    .....
    jmp endif  ; not actually necessary but printed here for completeness

endif:
```

當 cmp 成立時，則會執行 je，否則將會跳過。

組合語言中存在許許多多的跳轉條件：等於、小於等等。定義是相當直觀的，詳細請自行 google。

函式呼叫
-----------------

在組合語言中，呼叫函式只是跳轉到一個指定的標籤。

但麻煩的是參數傳遞，一般來說這分為兩個步驟：

1. 把參數放在特定 reg 或 address
2. 使函式呼叫具有通用性且不產生副作用

第一步很單純。作為例子我們先暫且使用 `al`（實際上是 `ax`）來作為參數的存放地點。

```nasm
mov al, 'X'
jmp print
endprint:

...

print:
    mov ah, 0x0e  ; tty code
    int 0x10      ; print() assume that 'al' already has the character
    jmp endprint  ; this label is also pre-agreed
```

單純是單純，但通用性為零。
目前的 `print` 函式只會返回到 `endprint`。
如果其他函式想要呼叫它那可怎麼辦(當返回地點不是 `endprint`)？
這讓我們無法重複這段程式碼。

我們可以透過以下兩點來改進目前的程式碼：
- 另找地點保存返回位址，使其可以變化
- 把當前的 regs 另作備份，使函式在使用 regs 時不致對全體產生副作用

關於保存返回位址，CPU 已有硬體實作。作為韌體開發只需使用 `call` 和 `ret`即可。

而關於 regs 的保存，可以透過使用基於 stack 的指令：`pusha` 和 `popa`，
這會自動把所有 regs 壓入，或從 stack 中恢復。

引用外部檔案
------------------------

引用 library 的重要性應該不用說明的吧 LOL。

語法如下：
```nasm
%include "file.asm"
```

列印十六進位值
-------------------

在下一課中，我們會需要從磁碟讀取資料，我們提供了 `boot_sect_print_hex.asm` 來打印十六進位位元組，而不僅僅是 ASCII 字元。

來寫 Code ！
-----

可以開始寫 code 了。`boot_sect_print.asm` 是一個 library，將會在 main 中被 `%include`。
這是用於在螢幕上列印位元組。還附送一個換行符打印。
順帶一提我們熟悉的 `'\n'` 實際上是兩個位元組，換行符 `0x0A` 和回車符 `0x0D`。
這是從古早年代的打字機所流傳下來的，
你可以嘗試刪除回車符位元組來看看會有甚麼效果。

正如上面所述，`boot_sect_print_hex.asm` 是用於打印位元組的 library。

而存於 entry point 的主文件 `boot_sect_main.asm` 載入了幾個字串和位元組，
然後呼叫 `print` 和 `print_hex`，
最後進入無窮迴圈。
