# スマフォで始めるWebアプリ開発

## はじめに

これには、アルファ版という前バージョンがあるのですが。
「[ワンストップアプリ開発](https://booth.pm/ja/items/1733776)」という技術合同誌に収録されています。
その頃は、通勤という人類には、早すぎる苦行が当たり前の様に行われている時期だったので、
スマフォにLinux環境を入れて、電車の中で、個人でのアウトプットを増やし、自分の市場価値を高めよう！！
そういう内容でしたが、この1年、在宅で仕事することが多くなり、完全にコンセプトが崩れてしまいました。
目の前にちゃんとした開発環境（クアッドコアのCore i7のCPU・24GBのRAM・Geforce GTX 1070）があるからわざわざスマフォに入れなくていいよね？と。
そう思ったのですが、とあるネタツイートが、1040いいね・184RTという個人的に大変おバズり申し上げたので、もしかして、今でも需要があるのかな？と思い、この記事を書きました。

### ツイートについて

[https://twitter.com/endo_hizumi/status/1365981276348452868?s=21](https://twitter.com/endo_hizumi/status/1365981276348452868?s=21)
さて、このツイートなのですが。
どう環境構築してるのか？気になると思いますので、これから解説したいと思います。
Android上で使い慣れたVS Codeが動いているとなれば、ウキウキですね！
パソコンを持ち運ばずに、スマフォだけで、プログラムが作れる！
M1のMacBook Airでも、1.29Kgありますからね。
スマフォは重くても、331g(G8X ThinQ デュアルスクリーンケース装着時)

動作している環境は以下の通りです。

スマートフォン

- Xperia 1 II XQ-AT42
  - Android 10
  - Termux 0.101
  - code-server 3.9.0
  - VisualStudio Code 1.52.1

## 簡単に言うと

Androidの端末エミュレーターアプリである「Termux」で、Code-Serverのインストールと起動を行っています。
ツイートの環境は、code-serverをセットアップしたスマフォをUSB Type-Cケーブルでモバイルディスプレイに接続しています。
単純に接続しても良いのですが、せっかく１５インチのディスプレイにつなぐので、１０から試験的に搭載されたデスクトップモードを有効にしています。
試験的なので、いろいろ使い勝手の悪いところはありますが、（開発者オプションから有効にする隠れ機能ですしね。）スマフォ本体と別のアプリをディスプレイ側で起動することができます。
写真でスマフォ側にログだけが流れているのは、そういうカラクリです。
スマフォ本体に、Termuxを起動させて、ディスプレイ側でcode-serverにアクセスするためのブラウザを立ち上げています。
あと、デスクトップモードでは、キーボードとマウスが必須なので、Bluetoothで接続しています。

## Termuxとは

AndroidでLinuxのコマンドを使える端末エミュレータです。
[公式サイト](https://termux.com/)
Root化の必要がなく、ls,cd,catといった一般的なコマンドから、aptなどのパッケージマネージャーも動作させることができます。
インストールできるパッケージが用意されていて、OSSでメンテされています。（メンテナーに感謝。スパチャはどこで投げられますか？[こちら](https://github.com/termux/termux-packages/tree/master/packages)のレポジトリの”Sponsor”ボタンから寄付ができます）
また、`termux-setup-storage`を実行することで、AndroidのストレージにTermuxからアクセスできます。（普通に、cdで）
ついでに言うとUbuntuのデスクトップ環境を作って、スマフォでLinuxのGUI環境も動かせます・・・！（UserLAnd使った方が手軽ですが。)

## code-serverとは

CoderというクラウドでVS Codeが使えるサービスのコードがオープンソース化したものがcode-serverです。
Coderの場合は、オフラインや通信環境がよくないと利用できませんが、スマフォ内でcode-serverを動かすので、ネットワークがなくても利用できます。
サーバー側でVS Codeを立ち上げ、ブラウザで`localhost:8080`にアクセスすると、
ブラウザで使い慣れたVS Codeが使えます。

## 開発環境のセットアップ

### アプリのインストール

Play Storeから、Termuxと　Hacker's Keyboradをインストールします。
[Termux - Google Play のアプリ](https://play.google.com/store/apps/details?id=com.termux&hl=ja&gl=US)
[Hacker&#39;s Keyboard - Google Play のアプリ](https://play.google.com/store/apps/details?id=org.pocketworkstation.pckeyboard&hl=ja&gl=US)

Hacker's Keyboardは、PCと同じキーボード配列のソフトウェアキーボードです。
日本語こそ入力できませんが、コマンドライン操作に必須な**ESC/Tab/Ctrl**のキーが用意されています。
また、変換候補が表示されないので、スムーズなコマンド操作ができます。
携帯できるBluetoothキーボードが一番ですが、キーボードがないときに積むので、入れとくのをお勧めします。

### code-serverのインストール

Termuxを起動して、以下のコマンドを実行します。

```bash
apt update
apt install -y nodejs yarn vim
```

次に、`build-essential`と`Python`をインストールします。
(ついでに使うので、gitも添えます。)

```bash
apt install -y build-essential Python git
```

全てのソフトウェアのインストールが終わったら、code-serverのインストールを行います。

```bash
yarn global add code-server
```

多少時間がかかりますので、スマホを放置して空でも眺めましょう・・・。

### code-serverの設定

インストールが終わったら、vimを開いて、以下のコンフィグファイルを開いて、編集します。
```bash
vi ~/.config/code-server/config.yaml
```

#### Vue.jsを動かしてみる

#### flaskのチュートリアルをやってみる

## PCとのスペック比較

#### AndroidでDockerが動く・・・？

## 最後に

## 参考

- [How to get Visual Studio Code to run on Android with Termux](https://dev.to/codeledger/how-to-get-visual-studio-code-to-run-in-termux-on-android-405j)
- [MacBook Air (M1, 2020) - 技術仕様](https://support.apple.com/kb/SP825?locale=ja_JP)
- [色々なスマートフォンのサイズと重さをまとめてみた | sakura86.com](https://sakura86.com/smartphone-size-summary/#toc_id_2)
- [TermuxとBluetoothキーボードでPC禁止を回避する - Qiita](https://qiita.com/bluepost59/items/657ef9c198bd43cf59dd)
- [Termux-setup-storage - Termux Wiki](https://wiki.termux.com/wiki/Termux-setup-storage)
- [Node.js - Termux Wiki](https://wiki.termux.com/index.php?title=Node.js)