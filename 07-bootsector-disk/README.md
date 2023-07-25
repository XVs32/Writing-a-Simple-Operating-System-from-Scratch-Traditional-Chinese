*在開始前你需要熟悉的概念: 硬碟, cylinder, head, sector, carry bit*

**目標: 讓 Boot Sector 從硬碟載入資料以啟動 kernel**

由於 OS 是不太可能塞進只有 512 bytes 的 Boot Sector，因此我們需要從硬碟載入 kernel。

而幸運的是 BIOS 內有提供跟硬碟的對接，讓我們可以透過 API 來操作硬碟。
為此，我們把 `al` 設置為 `0x02`（並將其他 reg 設置為所需的cylinder, head, sector），然後執行 `int 0x13`。

詳細請參考[int 13h 說明](http://stanislavs.org/helppc/int_13-2.html)。

在這一節課中，我們需要使用*carry bit*，它是每個 reg 中的一個額外的 bit，用於記錄操作是否溢出：

```nasm
mov ax, 0xFFFF
add ax, 1 ; ax = 0x0000，carry bit = 1
```

carry bit 在這邊被用作控制其他指令的判斷式，例如 `jc`（如果 carry bit 是 set，則跳轉）。

接下來請自行閱讀 `boot_sect_disk.asm` 中從硬碟讀取資料的完整過程。

在 `boot_sect_main.asm` 準備了磁碟讀取的參數並呼叫 `disk_load`。
在這裡我們寫入了一些不屬於 Boot Sector，超出了 512 bytes 的資料。

順帶一提，Boot Sector 是第 0 號硬碟的第 0 cylinder 的第 0 head 中的第 1 個 sector（sector 是從 1 開始的）。
因此，512 bytes 之後的 512 bytes 是對應於第 0 號硬碟的第 0 cylinder 的第 0 head 中的第 2 個 sector。

而我們的主程式將會填充這些資料並讓 Boot Sector 去讀取。

**注意: "It doesn't work, I don't know why" 的時侯，請確保 qemu 從正確的磁碟開機，並將 `dl` 設置為對應的硬碟**

一般情況下 BIOS 在呼叫 Bootloader 前會自行把 `dl` 設置為硬碟編號。
然而在使用 qemu 從硬碟開機時，可能會出現問題。

以下提供兩個可能的解決方法：
1. 使用 `-fda` 選項，例如，`qemu -fda boot_sect_main.bin`，這會把 `dl` 設置為 `0x00`。
2. 使用 `-boot` 旗標，例如，`qemu boot_sect_main.bin -boot c`，這會自動把 `dl` 設置為 `0x80`，讓 Bootloader 可以讀取資料。