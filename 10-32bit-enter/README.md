*在開始前你需要熟悉的概念：中斷 (interrupts)，流水線 (pipelining)*

**目標：進入 32 位保護模式並測試前面課程的程式碼**

要進入 32 位保護模式，我們需要依序執行以下步驟：

1. 停用 interrupt
2. 載入 GDT
3. 把 CPU 控制用的 reg (CR0) 中的某個位元設為 1
4. 使用一個特定的 jmp 指令來刷新 CPU pipeline
5. 更新所有的 segment regs
6. 更新 stack
7. 呼叫第一段 32 位程式碼

這個過程封裝在 `32bit-switch.asm` 中，請打開它並閱讀程式碼。

進入 32 位保護模式後，我們將呼叫 `BEGIN_PM`，這是實際有效的 32 位程式碼的進入點 (例如：kernel code 等)。你可以閱讀 `32bit-main.asm` 中的程式碼。編譯並執行這個檔案，螢幕上將會出現兩個訊息。

下一步將會是撰寫一個簡單的 kernel。