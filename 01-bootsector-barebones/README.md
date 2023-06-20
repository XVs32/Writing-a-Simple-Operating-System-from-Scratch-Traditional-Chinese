在開始前你需要熟悉的概念：組譯器、BIOS

**課程目標：建立一個被 BIOS 視為可開機磁碟的檔案**

興奮嗎？我們將建立自己的 boot sector！

理論
------

當電腦開機時，BIOS 並不知道作業系統的位置，所以 boot sector 必須位於已知且標準的位置(要不在最前，要不在最後)。一般來說會使用磁碟的第一個區段（cylinder 0, head 0, sector 0），佔用 512 個位元組。

而判斷是否為「可開機磁碟」則是一組 magic number，BIOS 會檢查 boot sector 的第 511 和第 512 個位元組是否為 `0xAA55`。

以下是一個 boot sector 的簡單例子：
```
e9 fd ff 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
[ 29 行的每行 16 個 00 ]
00 00 00 00 00 00 00 00 00 00 00 00 00 00 55 aa
```

基本上都是零，以 16 位元的值 `0xAA55` 結尾（注意x86 是 little-endian，所以數值上並不是 `0x55AA`）。
而 `e9 fd ff` 會執行一個無限迴圈。

簡易 boot sector
-----------------

你可以使用 Hex editor 來制作上述的 512 個位元組，或者只需以下的 code ：

```nasm
; Infinite loop (e9 fd ff)
loop:
    jmp loop 

; Fill with 510 zeros minus the size of the previous code
times 510-($-$$) db 0
; Magic number
dw 0xaa55 
```

編譯指令：
`nasm -f bin boot_sect_simple.asm -o boot_sect_simple.bin`

> Mac：如果出現編譯錯誤，請回頭閱讀第 00 章`

編譯成功後可以使用：

`qemu boot_sect_simple.bin`

> 在某些情況下，你可能需要使用 `qemu-system-x86_64 boot_sect_simple.bin`。如果出現 SDL 錯誤，請嘗試加上 --nographic 和/或 --curses 參數。

如果你看到一個視窗上面寫著「Booting from Hard Disk...」，那麼恭喜你！你成功制作出一個有效的 boot sector 了！
