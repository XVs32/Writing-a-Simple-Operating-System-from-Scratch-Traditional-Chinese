**在開始前你需要熟悉的概念：交叉編譯器 (cross-compiler)**

**目標：建立開發 kernel 用的環境**

譯者注：原文內容是基於 MAC 的開發環境建立，而譯者手上的是 Ubuntu 系統，因此這邊也會以 Ubuntu 的環境建立為準。

無論如何，一旦我們開始使用高階語言，將無可避免地需要使用 [cross-compiler](http://wiki.osdev.org/Why_do_I_need_a_Cross_Compiler%3F)。

所需套件
---------

一般請況下你的 Ubuntu 系統已經預載了，如果沒有的話就請安裝以下套件：

- gcc
- binutils

---------
Cross compile 指令
雖然我們安裝了 gcc, 但實際需要編譯的並不是在現代64位系統上運行的可執行檔。
以本教程為例，我們需要的是32位用的可執行檔。
幸運的是，我們只需要加上 `-m32` 即可。

gcc: `gcc -m32 -fno-pie`

`-m32`是指定 32 位，
而`fno-pie`則是 Position Independent Executable (PIE)，
我們在寫的是 kernel，是放在固定位置的，自然也不需要 PIE 了。

使用 linker 時也是同樣的道理。

ld: `ld -m elf_i386`

---

完成！於是編譯器和 linker 的環境建立就到此為止，然而原文教程是基於 MAC 環境。
用的是 `i386-elf-` 系列，於是山不轉路轉，來配合他一下。

```
alias i386-elf-gcc='gcc -m32 -fno-pie'
alias i386-elf-ld='ld -m elf_i386'
```

這樣就不用特別修改後面使用的 Makefile 了。
注意有需要的話請把以上 `alias` 自行加入到 `.bashrc`，可以省卻每次重新開機後的設定。