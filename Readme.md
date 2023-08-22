# Raspberry Pi 4 への xrdp の導入方法

Created on August 22, 2023  
Updated on August 22, 2023    
Copyright (c) 2023 led-mirage

## はじめに

この資料では Raspberry Pi 4 に xrdp を導入する手順について説明します。

xrdp を導入することで Windows PC からリモートデスクトップ接続（RDP）できるようになります。VNC でもいいのかもしれませんが、個人的には RDP の方が使い勝手がいいと感じています。

検証に使用した環境は以下の通りです。

- Raspberry Pi 4 Model B 4GB
- Raspberry Pi OS 64bit Bullseye
- xrdp 0.9.17
- Windows 10 / 11（RDPクライアント側）

## 留意事項

ディスプレイに接続していないラズパイをリモート操作できるようにすることが目的ですが、以下の2つの設定をしないとまともに接続・操作ができないので注意してください。

- xorg.confファイルの編集
- VNCの有効化

詳細は後述します。

## インストール

ターミナルから次のコマンドを実行してxrdpをインストールします。

```bash
sudo apt update
sudo apt install xrdp
```

インストールが完了したら以下のコマンドでバージョンを確認できます。


```bash
xrdp --version
```

## xorg.confファイルの編集

これで完了かと思いきや、これだけだと何故かWindowsからリモート接続できません。以下のファイルを開き編集します。

```
/etc/X11/xrdp/xorg.conf
```

保存するには管理者権限が必要なので、以下のコマンドを使用してテキストエディタを開きます。

```bash
sudo nano /etc/X11/xrdp/xorg.conf
```

開いたら下の方にある以下の行を編集します。

編集前
```conf
    Option "DRMDevice" "/dev/dri/renderD128"
```

編集後
```conf
    #Option "DRMDevice" "/dev/dri/renderD128"
    Option "DRMDevice" ""
```

編集し終えたら Ctrl + O -> Enter でファイルを保存します。Ctrl + X で nano を終了できます。

## xrdpの再起動

以下のコマンドでxrdpを再起動します。

```bash
sudo systemctl restart xrdp
```

## VNCの有効化

以上で接続はできるようになるのですが、VNC を有効にしないとなぜか Windows PC からの操作が異常に遅くなる現象が発生します。以下の操作でVNCを有効にします。

```
設定 -> Raspberry Pi の設定 -> インターフェイス -> VNC（有効）
```

## 接続テスト

ラズパイを一旦終了後、ディスプレイケーブルを外し再起動してください。

その後、Windows PC からリモートデスクトップ接続し問題なく操作できたらＯＫです。

## 参考URL

https://github.com/neutrinolabs/xrdp/issues/2060  
https://qiita.com/otti83/items/1855807b87ca7a0500dc
