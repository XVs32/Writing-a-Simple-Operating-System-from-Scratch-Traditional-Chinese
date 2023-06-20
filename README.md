Writing a Simple Operating System—from Scratch

###### tags: `Writing a Simple Operating System—from Scratch`

---

如何從零開始創建作業系統！

## 譯者前言

這是基於[這篇](https://github.com/cfenollosa/os-tutorial)英文文章所翻譯的教學，主要是作為我個人在學習過程中的筆記。譯者是曾在台灣留學的澳門人，因此專業用語上會以台灣的用詞為主。雖說是翻譯，但譯者在閱讀本教學的同時也會參考本教學所參考的 Writing a Simple Operating System —
from Scratch 一書，譯者可能會按個人習慣對文本內容進行補充或修改。

---

我一直想學習如何從頭開始製作作業系統。在大學裡曾學習如何實現pagination, semaphores, memory management 等等），但是：

* 我從來沒有從自己的 boot sector 開始寫過
* 大學的東西大多都還回去了
* 我並不認為閱讀 kernel 的原始碼是一個學習的好方法

因此基於 [Writing a Simple Operating System —
from Scratch](http://www.cs.bham.ac.uk/~exr/lectures/opsys/10_11/lectures/os-dev.pdf) 一書和 [OSDev wiki](http://wiki.osdev.org/)，我將嘗試製作逐步的教學跟程式碼範例。你可以把這份教學視為前述的書中的實作部分。

追加參考資料:[the little book about OS development](https://littleosbook.github.io),
[JamesM's kernel development tutorials](https://web.archive.org/web/20160412174753/http://www.jamesmolloy.co.uk/tutorial_html/index.html)

Features
--------

- 本教程是一個針對對底層運算感興趣的人。假如你對對作業系統的運作方式感到好奇，但沒有時間或意願從頭到尾閱讀一次 Linux kernel 的原始碼，那你來對地方了。
- 本教學的理論知識將會減到最少。需要理論知識的話請自行Google(教學內會提供關鍵字)。
- 每節課的所需時間會盡可能壓縮在5-15分鐘。我能做到的話，那你應該也能做到的！

How to use this tutorial
------------------------

1. 從第一個文件夾開始，按順序閱讀。由於每節課之間有連貫性，所以如果你直接跳到05的資料夾，你會不知道為什麼跳出來一個`mov ah，0x0e`，因為這在02的資料夾已經說明過了。當然，如果你很清楚自己在幹什麼的話也是可以跳過啦。

2. 打開README並閱讀第一行，該行會說明了在閱讀原始碼之前你應該具備的知識。如果出現你沒見過的專有名詞，請自行Google相關資料。第二行會列出每節課的目標，這會說明我們為什麼要做這些事情。"Why"和"How"是一樣重要的。
 
3. 把剩下的README讀完。拜托，這README已經**非常精簡**了。

4. （可選）在閱讀完README之後，嘗試自己寫出要求的 code。

5. 查看原始碼範例。裡面有詳細的註解。

6. （可選）自行進行實驗並嘗試玩壞你的檔案。理解修改哪個部分會帶來什麼後果是幫助你理解課程內容的一個好方法。

總結：首先閱讀每個資料夾的README，然後閱讀原始碼。如果你時間足夠的話就嘗試自己寫code實驗。

Strategy
-------- 

我們會在這個作業系統上執行很多不同的操作:

- 不使用 GRUB 的前提下把系統 Boot 起來 - DONE!
- 進入 32-bit 模式 - DONE
- 從組合語言到C語言 - DONE!
- Interrupt handling - DONE!
- 螢幕輸出和鍵盤輸入 - DONE!
- 一個小型的基本 `libc`，可以在開發過程中根據需要擴展 - DONE!
- Memory management
- 一個 filesystem 用於儲存檔案
- 寫一個簡單 shell
- User mode
- 簡單的文本編輯器
- Multiple processes 和 scheduling


如果情況許可的話，我們也可能會嘗試:

- 一個70年代風格的BASIC解譯器！ 
- 一個 GUI
- 網絡支援

Contributing
------------

這是一個個人學習項目，雖然已經有一段時間沒有更新，但我仍然希望將來可以繼續下去。

在此感謝所有指出錯誤並提交pull request的人。我需要一些時間來review，所以目前不能給出任何保證。

請隨意fork這個repo。如果你對繼續這個項目感興趣，打算繼續下去的話，我會幫你的repo標記為 "main fork"。
