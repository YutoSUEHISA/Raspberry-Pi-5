
# Cefore セットアップ手順まとめ


## 1. Ceforeとは
Ceforeは、分散型ファイルシステムやネットワーク通信の研究・開発に利用されるCCNベースのオープンソースソフトウェアです。主にコンテンツ指向ネットワーク（Content-Centric Networking, CCN）や情報指向ネットワーク（Information-Centric Networking, ICN）の実験・評価に用いられます。


## 2. 必要な環境・前提条件
- OS: Raspberry Pi OS, Linux, macOS, Windows（推奨はLinux）
- Git
- CMake
- GCCまたはClang
- OpenSSL
- Boostライブラリ
- その他依存パッケージ（詳細は公式リポジトリ参照）


## 3. インストール手順
1. 必要なパッケージのインストール（例: Raspberry Pi OS）
   - 事前に解凍したファイルをUSBにコピー
   - USBをRaspberry Piに挿入し、`/home/ユーザー名`に配置

2. ライブラリのインストール
   ```sh
   sudo apt-get install libssl-dev automake
   ```

3. ビルド
   ```sh
   aclocal
   autoconf
   automake
   ./configure --enable-csmgr --enable-cache  # csmgr,ローカルキャッシュを有効化
   make
   sudo make install
   sudo ldconfig
   ```


## 4. アンインストール手順
1. ceforeディレクトリに移動
   ```sh
   cd cefore-x.x.x
   ```
2. アンインストールを実行
   ```sh
   sudo make uninstall
   ```
3. `/usr/local/bin`から実行ファイルが消えているか確認


## 5. 動作確認方法
1. Ceforeのバージョン確認
   ```sh
   cefgetfile -v
   ```
2. サンプル通信の実行
   - 公式ドキュメントのサンプルコマンドを参照してください。


## 6. トラブルシューティング
- 依存パッケージの不足エラーが出る場合は、エラーメッセージに従い追加でインストールしてください。
- ビルドエラー時はCMakeやBoostのバージョンを確認してください。
- 詳細なエラー内容は`build`ディレクトリ内のログを参照してください。


## 7. 参考リンク
- [Cefore公式GitHubリポジトリ](https://github.com/cefore/cefore)
- [Cefore公式ドキュメント](https://cefore.net/)


## 8. 固定IPアドレスの割り当て方法（nmtuiを使用）
1. ターミナルで以下を実行：
   ```sh
   sudo nmtui
   ```
2. 表示されたメニューから「Edit a connection」を選択
3. 「Wired connection 1」（またはeth0に対応する接続名）を選択し、編集モードへ
4. 「SHOW」を選択してアドレス欄を編集できるようにし、以下を入力：
   - Addresses: `192.168.1.x/24`
   - Gateway: `192.168.1.1`
   - DNS servers: `8.8.8.8,1.1.1.1`
5. 設定を保存してメニューを抜ける
6. NetworkManagerを再起動して設定を適用：
   ```sh
   sudo systemctl restart NetworkManager
   ```
7. 設定が適用されたか確認：
   ```sh
   hostname -I
   ```
   先ほど設定した固定IPが表示されれば成功です。
