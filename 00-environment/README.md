
​在開始前你需要熟悉的概念：Linux、Mac、terminal、編譯器、模擬器、nasm、qemu。

**課程目標：安裝執行教學所需的軟體**

我個人是使用 Mac，不過使用 Linux 會更好，因為它已經預先安裝了所有標準工具，這能簡化我們的安裝流程。

在 Mac 上，請先[安裝 Homebrew](http://brew.sh)，然後執行 `brew install qemu nasm` 進行安裝。

請統一使用 `/usr/local/bin/nasm`。請不要使用Xcode 開發工具中的 `nasm`，大多數情況下他是無法運作的。

在某些系統上，`qemu` 會被分成多個執行檔。你可能需要使用 `qemu-system-x86_64 <binfile>` 來執行。
