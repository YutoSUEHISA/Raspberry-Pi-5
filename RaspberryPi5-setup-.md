# Raspberry Pi 5 セットアップ手順

## 必要なもの
- Raspberry Pi 5 本体（今回は8GB RAMを使用）
- microSDカード（推奨：32GB以上）
- 電源アダプター（USB-C経由の5V/5A 27W USB-C 電源）
- miniHDMIケーブル & モニター
- Raspberry Pi 5用 Raspberry Pi ケース
- Raspberry Pi アクティブクーラー（冷却ファン）
- キーボード & マウス
- インターネット接続（有線またはWi-Fi）

## OSのインストール
1. [Raspberry Pi Imager](https://www.raspberrypi.com/software/) をダウンロード
2. Imagerを起動し、Raspberry Pi OS 64-bitを選択
3. microSDカードをPCに挿入し、書き込み開始
4. （設定があるので後日記載・画像あり）
5. 書き込み完了後、microSDカードをPiに挿入

## 初回起動
1. Raspberry Pi 5に周辺機器を接続
2. 電源を入れる
3. ネットワークに接続し、時刻同期を行い、osのアップデート

## ネットワークへの接続と切断（設定情報を削除）
* ネットワークへの接続
```bash
nmcli con modify "Wired connection 1" ipv4.addresses "192.168.1.50/24"
nmcli con modify "Wired connection 1" ipv4.gateway "192.168.1.1"
nmcli con modify "Wired connection 1" ipv4.dns "8.8.8.8 1.1.1.1"
nmcli con modify "Wired connection 1" ipv4.method manual
nmcli con up "Wired connection 1"
```
* ネットワークからの切断（設定情報を削除）
```bash
nmcli con modify "Wired connection 1" ipv4.gateway ""
nmcli con modify "Wired connection 1" ipv4.dns ""
nmcli con modify "Wired connection 1" ipv4.method auto
nmcli con up "Wired connection 1"
```

## 時刻同期
```bash
sudo systemctl stop systemd-timesyncd
timedatectl set-time "yyyy-mm-dd hh:mm:ss"
sudo systemctl start systemd-timesyncd
```

## osのアップデート
```bash
sudo apt update
sudo apt upgrade
```

## シェルスクリプト（.sh）の作成から実行まで
1. sudo nano hoge.sh （使用頻度が高い一連のコマンドを入力）
2. sudo su
3. .(半角スペース)hoge.sh
4. exit（rootユーザから抜ける）

## 参考リンク
- [公式セットアップガイド](https://www.raspberrypi.com/documentation/computers/getting-started.html)
- [日本語化](https://note.com/coderdojoseishin/n/n18cade0e500e#3231e5f7-cc4b-4dd1-a937-049abb0909f4)