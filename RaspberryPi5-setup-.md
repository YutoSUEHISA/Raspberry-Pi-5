
# Raspberry Pi 5 セットアップ手順


## 必要なもの
- Raspberry Pi 5 本体（今回は8GB RAMを使用）
- microSDカード（推奨：32GB以上）
- 電源アダプター（USB-C経由の5V/5A 27W USB-C 電源）
- miniHDMIケーブル & モニター
- Raspberry Pi 5用ケース
- アクティブクーラー（冷却ファン）
- キーボード & マウス
- インターネット接続（有線またはWi-Fi）


## OSのインストール
1. [Raspberry Pi Imager](https://www.raspberrypi.com/software/) をダウンロード
2. Imagerを起動し、「Raspberry Pi OS 64-bit」を選択  
3. microSDカードをPCに挿入し、書き込みを開始
4. （初期設定については後日記載・画像追加予定）
5. 書き込み完了後、microSDカードをRaspberry Piに挿入  
   ![RaspberryPi設定]()


## 初回起動
1. Raspberry Pi 5に周辺機器（モニター、キーボード、マウス等）を接続
2. 電源を入れる
3. ネットワークに接続し、時刻同期とOSのアップデートを行う


## ネットワーク設定

### 有線ネットワークへの固定IP割り当て
```bash
nmcli con modify "Wired connection 1" ipv4.addresses "192.168.1.50/24"
nmcli con modify "Wired connection 1" ipv4.gateway "192.168.1.1"
nmcli con modify "Wired connection 1" ipv4.dns "8.8.8.8 1.1.1.1"
nmcli con modify "Wired connection 1" ipv4.method manual
nmcli con up "Wired connection 1"
```

### ネットワーク設定の自動取得に戻す（設定情報の削除）
```bash
nmcli con modify "Wired connection 1" ipv4.gateway ""
nmcli con modify "Wired connection 1" ipv4.dns ""
nmcli con modify "Wired connection 1" ipv4.method auto
nmcli con up "Wired connection 1"
```


## 時刻同期
```bash
sudo systemctl stop systemd-timesyncd
sudo timedatectl set-time "yyyy-mm-dd hh:mm:ss"
sudo systemctl start systemd-timesyncd
```


## OSのアップデート
```bash
sudo apt update
sudo apt upgrade
```


## シェルスクリプト（.sh）の作成と実行
1. シェルスクリプトを作成（例：hoge.sh）
	```bash
	sudo nano hoge.sh
	# 使用頻度が高い一連のコマンドを記述
	```
2. rootユーザーに切り替え
	```bash
	sudo su
	```
3. スクリプトを実行
	```bash
	. /hoge.sh
	```
4. rootユーザーから抜ける
	```bash
	exit
	```

## 参考リンク
- [公式セットアップガイド](https://www.raspberrypi.com/documentation/computers/getting-started.html)
- [日本語化](https://higmasan.com/iot/raspberrypi/raspberrypi5/make-raspberry-pi-5-into-japanese-environment/)