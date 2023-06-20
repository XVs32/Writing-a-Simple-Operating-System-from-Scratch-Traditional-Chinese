*在開始前你需要熟悉的概念：interrupts、CPU registers*

**課程目標：使上一章節的 boot sector 打印文字**

在這一章節中我們會對上一章節的無限迴圈 boot sector 進行一些改進，讓它在螢幕上打印文字。其中會使用到 interrupt。

在這個範例中，我們將把 "Hello" 這個詞的每個字母寫入到 `al` 寄存器（`ax` 的 bottom half），把 `0x0e` 寫入到 `ah` 寄存器（`ax` 的 top half），然後觸發 `0x10` interrupt，這是一個用於視訊服務的通用 interrupt。(譯者注：由於 interrupt 的數量有限，我們不可能針對每個 BIOS routine 都配置 interrupt，因此 BIOS 會對 interrupt 進行多工化，具體一般會根據 `ax` 的值來決定實際要執行的指令)

在 `ah` 中的 `0x0e` 告訴 interrupt 我們希望執行的實際功能是「以 tty 模式將 `al` 的內容寫入」。

把狀態設定為 tty 模式的工作只需要執行一次，因為我們是 CPU 上唯一運行的東西。
但真實世界中，其他 process 可能在 CPU 上運行，並留下了垃圾數據在 `ah` 中。這是我們需要注意的。

不過在這個示例中，我們不需要擔心這個問題。

新的 boot sector 如下所示：
```nasm
mov ah, 0x0e ; tty mode
mov al, 'H'
int 0x10
mov al, 'e'
int 0x10
mov al, 'l'
int 0x10
int 0x10 ; 'l' is still on al, remember?
mov al, 'o'
int 0x10

jmp $ ; jump to current address = infinite loop

; padding and magic number
times 510 - ($-$$) db 0
dw 0xaa55 
```

編譯，執行指令跟上一章節一樣

`nasm -fbin boot_sect_hello.asm -o boot_sect_hello.bin`

`qemu boot_sect_hello.bin`

現在 boot sector 在進入無限迴圈前將會顯示「Hello」。
