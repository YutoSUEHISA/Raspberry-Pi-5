

# Cefore セットアップ手順まとめ

---
作成者: Yuto SUEHISA  作成日: 2025年10月8日  更新日: 2025年10月9日
---


## 1. Ceforeとは
Ceforeは、分散型ファイルシステムやネットワーク通信の研究・開発に利用されるCCNベースのオープンソースソフトウェアです。主にコンテンツ指向ネットワーク（Content-Centric Networking, CCN）や情報指向ネットワーク（Information-Centric Networking, ICN）の実験・評価に用いられます。


## 2. 必要な環境・前提条件
- OS: Raspberry Pi OS, Linux, macOS, Windows（推奨はLinux）
- USB（解凍済みファイルをRaspberry Piへ移すため）
-ネットワークへの接続（ライブラリのインストールのため）
- その他依存パッケージ（詳細は公式リポジトリ参照）


## 3. インストール手順
1. [必要なパッケージのインストール](https://github.com/cefore/cefore/releases)（例: Raspberry Pi OS）
- zipファイルをUSBにコピー
- USBをRaspberry Piに挿入し、`/home/ユーザー名`に配置

2. ライブラリのインストール（ネットワークへの接続必）
   ```sh
   sudo apt-get install libssl-dev automake
   ```

3. ビルド
   ```sh  
   unzip cefore-x.x.x.zip # zipファイルを解凍
   cd cefore-x.x.x
   aclocal
   autoconf
   automake
   ./configure --enable-csmgr --enable-cache  # csmgr,ローカルキャッシュを有効化
   make
   sudo make install
   sudo ldconfig
   ```



## 4. 動作確認方法
   ```sh
   sudo cefnetdstart
   cefstatus
   sudo cefnetdstop
   ```


## 5. アンインストール手順
1. ceforeディレクトリに移動：
   ```sh
   cd cefore-x.x.x
   ```
2. アンインストールを実行：
   ```sh
   sudo make uninstall
   ```
3. `/usr/local/bin`から実行ファイルが消えているか確認


## 6. 固定IPアドレスの割り当て方法（nmtuiを使用）  
1. LANケーブルを接続
2. ターミナルで以下を実行：
   ```sh
   sudo nmtui
   ```
3. 表示されたメニューから「Edit a connection」を選択
4. 「Wired connection 1」（またはeth0に対応する接続名）を選択し、編集モードへ
5. 「SHOW」を選択してアドレス欄を編集できるようにし、以下を入力：
   - Addresses: `192.168.1.x/24`
   - Gateway: `192.168.1.1`
   - DNS servers: `8.8.8.8,1.1.1.1`
6. 設定を保存してメニューを抜ける
7. NetworkManagerを再起動して設定を適用：
   ```sh
   sudo systemctl restart NetworkManager
   ```
8. 設定が適用されたか確認：
   ```sh
   hostname -I
   ```
   先ほど設定した固定IPが表示されれば成功です。


## 7. トラブルシューティング
- `autoconf`に失敗する場合は，`aclocal`を実行．
- `make`の実行中に同じコードが流れている場合は，`Ctrl+C`で処理を停止する．`make clean`を実行し再度`make`を実行する．  
- `make`の実行中に未来時刻に関するメッセージが表示された場合は，時刻同期を行う必要がある．    
   ```bash
   sudo systemctl stop systemd-timesyncd
   sudo timedatectl set-time "yyyy-mm-dd hh:mm:ss"
   sudo systemctl start systemd-timesyncd
   ```


## 8. 参考リンク
- [Cefore公式GitHubリポジトリ](https://github.com/cefore/cefore)
- [Cefore公式ドキュメント](https://cefore.net/)

