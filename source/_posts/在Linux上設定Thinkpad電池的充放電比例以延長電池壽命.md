---
title: 在Linux上設定Thinkpad電池的充放電比例以延長電池壽命
date: 2018-05-05 21:00:00
tags: 
- 工作環境設定
categories:
- 筆記
---

Thinkpad可以設定電池充放電比例來延長電池壽命。例如只在電池低於60%時充電，或是不把電池充滿只充到95%左右，畢竟多充的那5%並不能提供多久的使用時間還會導致電池頻繁的充放電。

在Windows上聯想官方有提供這樣的充電控制工具能使用，不過在Linux上就沒這樣方便，因此我在一篇[日文的部落格](https://www.xmisao.com/2014/01/21/thinkpad-battery-setting-by-tlp-on-linux.html)文章上找到了一個可用的工具。在此我簡單介紹如何使用這個工具，也是幫自己留個紀錄讓下一次自己重灌時幫助自己能快速設定。

經本人測試此工具在 `Debian Jessie`、`Ubuntu 14.04`、`Ubuntu 16.04`，能正常使用。
使用環境為`Thinkpad T450`，但根據其他文章顯示 `Thinkpad X240` 也能順利控制Thinkpad的內置及外掛電池。

- [ThinkPad ACPI Battery Util](https://github.com/teleshoes/tpacpi-bat)

## 安裝
將Github的專案clone回自己的電腦
```
$ git clone https://github.com/teleshoes/tpacpi-bat.git
```

到 `tpacpi-bat` 目錄並使用root權限來執行 `./install.pl` 指令來安裝 `tpacpi-bat`。
```
$ cd tpacpi-bat
$ ./install.pl
```

## 確定預設的充電控制比例
在設置之前，請檢查每個電池的ST（開始充電電量）和SP（停止充電電量）的默認值。在電池的第1號是主電池（內置電池），2號電池的是外掛電池。
要檢查設置，請使用 `tpacpi-bat -g`。需要root權限才能使用。
在我的環境中，內置電池顯示的是我在Windows的設定，外掛電池是0。
```
$ tpacpi-bat -g ST 1
60
$ tpacpi-bat -g SP 1
90
$ tpacpi-bat -g ST 2
0 (default)
$ tpacpi-bat -g SP 2
0 (default)
```

## 充電控制比例設定
設定充電控制比例，使用 `tpacpi-bat -s` 。設置主電池（內置電池）在放電至60％後開始充電，並在充電至90％時停止充電，第二顆電池也採用同樣設定。

```
# tpacpi-bat -s ST 1 60
# tpacpi-bat -s SP 1 90
# tpacpi-bat -s ST 2 60
# tpacpi-bat -s SP 2 90
```

會使用這個配置是因為Lenovo官方有寫到建議設定：
- 充電到 90~95% 即可停止
- 電量到 60~75% 即開始充電

時常將其充到100%會加速電池劣化，但是建議3-6個月至少要將其完全放電一次，若長時間不使用也盡量要將其控制在20~30%的電量。

## 確認設定
再次使用 `tpacpi-bat -g` 來檢查設定是否成功套用。
```
# tpacpi-bat -g ST 1
60 (relative percent)
# tpacpi-bat -g SP 1
90 (relative percent)
# tpacpi-bat -g ST 2
60 (relative percent)
# tpacpi-bat -g SP 2
90 (relative percent)
```

## 結果
經過這個設定，當將下次為電腦充電時，可以確認一下電腦是不是在這個設定下進行充放電。另外由於此設置是硬體功能，因此即使在電腦沒開機時也是有效的。

## 參考資料
- [ThinkPadのバッテリーの充電・放電閾値をLinuxから設定してバッテリーの寿命を伸ばす方法](https://www.xmisao.com/2014/01/21/thinkpad-battery-setting-by-tlp-on-linux.html)
