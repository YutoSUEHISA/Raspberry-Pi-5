
# Raspberry Pi 5 セットアップ手順


<p style="color:#6b7280; font-size:0.85rem; margin:0; opacity:0.8;">作成者: Yuto SUEHISA&nbsp;&nbsp;更新日: 2025/11/09</p>


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
	 - 設定画面例：
		 ![RaspberryPi設定](./images/image%20(1).png)
	 - 書き込み時における各種設定例：
		 ![書き込み時における各種設定1](./images/image%20(2).png)
		 ![書き込み時における各種設定2](./images/image%20(3).png)
		 ![書き込み時における各種設定3](./images/image%20(4).png)
		 ![書き込み時における各種設定4](./images/image%20(5).png)
3. microSDカードをPCに挿入し、書き込みを開始
4. 書き込み完了後、microSDカードをRaspberry Piに挿入



## 初回起動
1. Raspberry Pi 5に周辺機器（モニター、キーボード、マウス等）を接続
2. 電源を入れる
3. ネットワークに接続し、時刻同期とOSのアップデートを行う



## ネットワーク設定

### 学内ネットワークへの固定IP割り当て
```bash
nmcli con modify "Wired connection 1" ipv4.addresses "133.80.180.19/24"
nmcli con modify "Wired connection 1" ipv4.gateway "133.80.180.254"
nmcli con modify "Wired connection 1" ipv4.dns "133.80.153.2 133.80.11.100"
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

## トラブルシューティング  
- 学内ネットワークへ接続できない場合は，デスクトップPC(os:windows)にEthernetを繋ぎ，**コマンドプロンプト** から **ipconfig /all** を入力し，**ipアドレス，サブネットマスク，デフォルトゲートウェイ，DNSサーバ**を確認する．  
※macの場合は，**Terminal**で**ifconfig**を入力


## 参考リンク
- [公式セットアップガイド][def1]
- [日本語化手順][def2]
- [Raspberry Pi 5 スターターキット/コンプリートキット 組み立て][def3]  
- [IPアドレスを確認する方法(ver.windows11)][def4]  
- [IPアドレスを確認する方法(ver.Mac)][def5]  

[def1]: https://www.raspberrypi.com/documentation/computers/getting-started.html
[def2]: https://higmasan.com/iot/raspberrypi/raspberrypi5/make-raspberry-pi-5-into-japanese-environment/
[def3]: https://www.switch-science.com/pages/pi5assy?srsltid=AfmBOopoUlmHAQ2BjKSIik8BoL_TY1Tk25WjZD8TiRxkAN-faqHfmMSP
[def4]: https://solutions.vaio.com/5615#:~:text=%E3%80%8C%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E3%83%97%E3%83%AD%E3%83%B3%E3%83%97%E3%83%88%E3%80%8D%E3%81%8B%E3%82%89%E7%A2%BA%E8%AA%8D%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95%20*%20%E3%80%8C%E3%82%B9%E3%82%BF%E3%83%BC%E3%83%88%E3%80%8D%E3%83%9C%E3%82%BF%E3%83%B3%E3%82%92%E3%82%AF%E3%83%AA%E3%83%83%E3%82%AF%E3%81%97%E3%81%A6%E3%80%81%E6%A4%9C%E7%B4%A2%E7%AA%93%E3%81%AB%E3%80%8C%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E3%80%8D%E3%81%A8%E5%85%A5%E5%8A%9B%E3%81%97%E3%81%BE%E3%81%99%E3%80%82%20*%20%E6%A4%9C%E7%B4%A2%E7%B5%90%E6%9E%9C%E3%81%AB%E3%80%8C%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E3%83%97%E3%83%AD%E3%83%B3%E3%83%97%E3%83%88%E3%80%8D%E3%81%8C%E8%A1%A8%E7%A4%BA%E3%81%95%E3%82%8C%E3%82%8B%E3%81%AE%E3%81%A7%E3%82%AF%E3%83%AA%E3%83%83%E3%82%AF%E3%81%97%E3%81%BE%E3%81%99%E3%80%82%20*%20%E3%80%8C%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E3%83%97%E3%83%AD%E3%83%B3%E3%83%97%E3%83%88%E3%80%8D%E7%94%BB%E9%9D%A2%E3%81%8C%E8%A1%A8%E7%A4%BA%E3%81%95%E3%82%8C%E3%81%BE%E3%81%99%E3%80%82,%E5%8F%82%E8%80%83%E6%83%85%E5%A0%B1%20%E3%80%8CIPv6%20%E3%82%A2%E3%83%89%E3%83%AC%E3%82%B9%E3%80%8D%E3%81%8C%E8%A1%A8%E7%A4%BA%E3%81%95%E3%82%8C%E3%82%8B%E5%A0%B4%E5%90%88%E3%81%AF%E3%80%81IPv6%E3%81%AEIP%E3%82%A2%E3%83%89%E3%83%AC%E3%82%B9%E3%81%8C%E5%89%B2%E3%82%8A%E5%BD%93%E3%81%A6%E3%82%89%E3%82%8C%E3%81%A6%E3%81%84%E3%81%BE%E3%81%99%E3%80%82%20*%20%E7%A2%BA%E8%AA%8D%E5%BE%8C%E3%80%8Cexit%EF%BD%A3%E3%81%A8%E5%85%A5%E5%8A%9B%E3%81%97%E3%81%A6%E3%80%90Enter%E3%80%91%E3%82%AD%E3%83%BC%E3%82%92%E6%8A%BC%E3%81%97%E3%80%81%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E3%83%97%E3%83%AD%E3%83%B3%E3%83%97%E3%83%88%E7%94%BB%E9%9D%A2%E3%82%92%E7%B5%82%E4%BA%86%E3%81%97%E3%81%BE%E3%81%99%E3%80%82%20%E4%BB%A5%E4%B8%8A%E3%81%A7%E6%93%8D%E4%BD%9C%E3%81%AF%E5%AE%8C%E4%BA%86%E3%81%A7%E3%81%99%E3%80%82%20%E2%80%BB%E3%82%AB%E3%83%83%E3%82%B3%E5%86%85%E3%82%92%E3%82%B3%E3%83%94%E3%83%BC%E3%81%97%E3%81%A6%E3%81%94%E5%88%A9%E7%94%A8%E3%81%8F%E3%81%A0%E3%81%95%E3%81%84%E3%80%82  
[def5]: https://qiita.com/yoriblog/items/6bd310cbe919eba9504b
