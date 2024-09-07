# Elixir Testnet v3 バリデーター セットアップガイド

## 目次

1. [準備](#1-準備)
2. [Docker のインストール](#2-dockerのインストール)
3. [validator.env ファイルの作成](#3-validatorenvファイルの作成)
4. [バリデーターの登録](#4-バリデーターの登録)
5. [バリデーターの実行](#5-バリデーターの実行)
6. [バリデーターのアップグレード](#6-バリデーターのアップグレード)

## 1. 準備

### ハードウェア要件:

- メモリ: 8GB 以上
- インターネット接続: 100Mbit の信頼性の高い接続
- ストレージ: 100GB のディスクスペース

## 2. Docker のインストール

1. 最新の Docker がインストールされていることを確認します。
2. 以下のコマンドをターミナルで実行して、Docker が正しくインストールされているか確認します:

   ```bash
   sudo docker ps
   ```

   このコマンドを実行すると、コンテナが一覧表示されます。

3. Docker がインストールされていない場合は、[Docker Engine Installation Guide](https://docs.docker.com/engine/install/)を参照してインストールしてください。

**今回はサーバーレンタルと Docker のインストール部分は割愛します。**

Docker のインストールや環境構築に関する参考情報が必要な場合は、以下のリソースをご覧ください：

- [だれでもできるノード構築](https://note.com/kagebunchin/n/nfb21d2055a5e)
- [初めて Contabo を使って Docker 環境をつくる](https://note.com/kosk_t/n/n39bd6ba6cef9)


Docker のインストールが完了したら、次のステップに進んでください

## 3. validator.env ファイルの作成

1. [validator.env サンプル](https://files.elixir.finance/validator.env)をダウンロードします。

2. ダウンロードした場合は、ファイルを編集します。手動で作成する場合は、以下のコマンドを使用します:

   ```bash
   touch validator.env
   nano validator.env
   ```

3. ファイルに以下の内容を記入し、適切な値に置き換えてください:

   ```env
   ENV=testnet-3
   STRATEGY_EXECUTOR_IP_ADDRESS=あなたの公開IP (プライベートIPではありません)
   STRATEGY_EXECUTOR_DISPLAY_NAME=好きな名前
   STRATEGY_EXECUTOR_BENEFICIARY=以前使っていたEVMアドレス (なければ新規作成)
   SIGNER_PRIVATE_KEY=以前使っていたEVMアドレスの秘密鍵 (なければ新規作成)
   ```

   公開 IP アドレスの確認方法:

   ```bash
   curl ifconfig.me
   ```

4. 編集が完了したら、Ctrl + O で保存し、Ctrl + X で終了します。

## 4. バリデーターの登録

1. [Alchemy Sepolia Faucet](https://sepoliafaucet.com/)にアクセスし、   gasになるethトークンをgetします。
2. [Elixir Network Testnet v3 ダッシュボード](https://testnet-3.elixir.xyz/)に移動し、ウォレットを接続します。
3. 右上の「MINT 1,000 MOCK」ボタンをクリックします。
4. MOCK トークンをステークし、バリデーターを登録します。
5. 下にスクロールして[カスタム バリデーター]ボタンをクリックし、登録したウォレットアドレスを入力して[委任]をクリックします。

## 5. バリデーターの実行

1. Docker イメージを取得します:

   ```bash
   sudo docker pull elixirprotocol/validator:v3
   ```

2. バリデーターを開始します:

   ```bash
   sudo docker run -d \
     --env-file ~/validator.env \
     --name elixir \
     elixirprotocol/validator:v3
   ```

3. 動作確認:

   ```bash
   sudo docker logs -f elixir
   ```

### **エラーがなく動作している場合、セットアップは完了です。**

## 6. バリデーターのアップグレード

バリデーターをアップグレードするには、以下の手順を実行します:

```bash
sudo docker kill elixir
sudo docker rm elixir
sudo docker pull elixirprotocol/validator:v3
sudo docker run -d \
  --env-file ~/validator.env \
  --name elixir \
  --restart unless-stopped \
  elixirprotocol/validator:v3
```

## お疲れさまでした！これで Elixir Testnet v3 のバリデーターセットアップが完了です。

＊内容も簡単だったので今回はアウトプットのためのだけのガイド記事書きました！もしうまくいかない場合は公式 discord で確認してください。
