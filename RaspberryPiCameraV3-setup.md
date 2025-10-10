
# Raspberry Pi Camera v3 - 取り付けから動作確認まで


<p style="color:#6b7280; font-size:0.85rem; margin:0; opacity:0.8;">作成者: Yuto SUEHISA&nbsp;&nbsp;更新日: 2025-10-10</p>



## 対象
- Raspberry Pi 5（Raspberry Pi OS 64-bit を想定）
- Raspberry Pi Camera Module v3（CSIコネクタ接続）

## 準備
- 電源を切った状態で作業すること。
- 精密ドライバー（場合によって）と帯電防止に注意する。

## ハードウェア接続（物理接続）
1. Raspberry Pi本体のCSIポート（カメラ用フレキケーブル差込口）を見つける。
2. カメラのフレキケーブルの金属端子がカメラ側とボード側で向き合うようにセットする（通常、導体面がコネクタの端子側を向く）。
3. CSIコネクタのロック（ラッチ）を慎重に引き上げ、フレキケーブルを差し込み、ロックを押し戻して固定する。
4. ケーブルの向きや差し込みの奥行きを確認する。

注意点:
- フレキケーブルは力任せに曲げない。端子に傷や汚れがないか確認。

## ソフトウェア準備
Raspberry Pi OS を最新に更新し、必要なパッケージ（libcamera, picamera2）をインストールします。

1. システム更新

```bash
sudo apt update; sudo apt upgrade -y
sudo reboot
```

2. libcamera と関連ツールの確認/インストール

```bash
sudo apt install -y libcamera-apps libcamera-dev
```

3. Python 環境と picamera2（推奨）

Raspberry Pi OS の最近のイメージでは picamera2 が含まれていることが多いですが、含まれていない場合は以下で導入します。

```bash
# Python3 と pip の確認
sudo apt install -y python3-pip python3-pil
python3 -m pip install --upgrade pip

# picamera2 と依存ライブラリ
python3 -m pip install --user picamera2
```

注: `--user` を付けるとユーザローカルにインストールされます。システム全体に入れる場合は `sudo -H` を使います。

## カメラが認識されているか確認
1. カメラ接続後に Raspberry Pi を起動し、次のコマンドでデバイスが見えるか確認します。

```bash
libcamera-hello --version
libcamera-hello
```

`libcamera-hello` が動作してカメラのプレビューが表示されればハード/ソフトの接続は成功です（GUI 環境が必要）。

2. カメラデバイスの情報確認

```bash
v4l2-ctl --list-formats-ext
```

（v4l2 が入っていない場合 `sudo apt install -y v4l-utils`）

## 画像取得（CLI）
簡単な静止画キャプチャ:

```bash
libcamera-jpeg -o test.jpg
```

動画キャプチャ（例: 10秒）:

```bash
libcamera-vid -t 10000 -o test.h264
```

## Python でのサンプル（picamera2）
以下は picamera2 を使った簡単な静止画取得サンプルです。

```python
from picamera2 import Picamera2
from PIL import Image

picam2 = Picamera2()
config = picam2.create_still_configuration()
picam2.configure(config)
picam2.start()
frame = picam2.capture_array()
img = Image.fromarray(frame)
img.save('capture.png')
picam2.stop()

print('Saved capture.png')
```

注意: GUIなし（headless）の環境では表示系の設定やデバイスのパーミッションに注意してください。

## トラブルシューティング
- 黒い画像しか表示されない: ケーブルの向き/差し込み不良、カメラの電源問題、カメラ自体の故障の可能性。
- Permission denied エラー: `sudo` で試すか、ユーザを適切なグループに追加（例: video）
	```bash
	sudo usermod -aG video $USER
	```
- libcamera コマンドがない: `sudo apt install libcamera-apps` を実行。

## 参考リンク
- Official Picamera2 documentation: https://www.raspberrypi.com/documentation/accessories/camera.html
- libcamera project: https://www.linuxtv.org/wiki/index.php/Libcamera



