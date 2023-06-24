*在開始前你需要熟悉的概念：stack*

**目標：學習如何使用 stack**

stack 是非常重要的功能，值得我們另寫一個例子。

請記住 `bp` reg 保存有 stack 的基地址（即 base address），而 `sp` 存儲 stack 的頂部，而 stack 從 `bp` 開始向下長。

本節內容非常直觀，請直接打開 `boot_sect_stack.asm` 文件，閱讀原始碼並嘗試理解註釋。

也可以嘗試在不同的位置訪問 stack，觀察 stack 的變化以及不同指令對 stack 的影響。
