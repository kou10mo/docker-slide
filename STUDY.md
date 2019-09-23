# Docker

## 仮想化とコンテナ

### 仮想化とは

ハードウェア+OSで動作する実環境ではなく、
ソフトウェアにより疑似的に、OSが動作する環境

### なぜ仮想化

端末の性能があがってきたため、
一つのOSを動作させるだけでは余るリソースが増えた

サーバの動作は、ゴリゴリのシステムでもなければ到底使い切れない！
(Webサーバで使う能力はゴミみたいなもの)

[ITの基本から押さえるコンテナ入門\(2\) コンテナとは？\(2\)ITシステム構成の変遷編 \| マイナビニュース](https://news.mynavi.jp/article/zerocontena-2/)

### 仮想化の種類

主に、以下の3種類
仮想マシン(ホストOS型)
仮想マシン(ハイパーバイザ型)
コンテナ

[【Docker入門】仮想マシンとコンテナの違いから仮想環境の構築まで \| オリジナルゲーム\.com](https://original-game.com/introduction-to-docker-virtual-machine-container-virtual-environment/)

### 仮想マシン(ホストOS型)

端末上にOS(ホストOS)がインストールされており、
そのOS上に仮想化ソフト(VirtualBoxなど)を動作させる。
仮想化ソフト上で仮想マシン(ゲストOS)を動作させる。
(紛らわしいけど、仮想化ソフトもハイパーバイザではある)

### 仮想マシン(ハイパーバイザ型)

ほとんどが商用でのみ利用される(レンタルサーバ,VPSなど)
端末上にハイパーバイザが乗っており、ホストOSが存在しない。
ホストOSがない分、仮想化のオーバーヘッド(無駄)が少ない。

### コンテナ

OS上でコンテナエンジン(Dockerエンジンなど)を動作させ、
その上でコンテナと呼ばれるアプリなどが動作する。
ゲストOSの存在がなく、各コンテナはOSの資源(カーネル)を使用するため、
仮想マシンより容量が少なく、動作も軽い

### 比較

仮想マシン
o OSを自由に選べる。ハイパーバイザが動作をエミュレートするため、MacやLinuxの上でゲストOSとしてWindowsを動かすことも可能
o カスタマイズ性が高い(仮想的ではあるもののほぼ物理マシンなので、ハードウェアのリソースを割り当てて柔軟にカスタムできる)
x 容量が大きい、動作がおもい(ハイパーバイザでワンクッション)

コンテナ
o 容量が少ない、動作が軽い(ハイパーバイザのクッションがない)。
o コンテナを組み合わせて部品のように取り回しやすい(wp+mysql+phpmyadmin)
x ゲストOSという概念がないので、Mac上でWinを動かすのは無理

[VPSの二大仮想化技術「コンテナ型」vs「VM型」―― その違いとメリット・デメリット](https://www.tsukaeru.net/blog-detail/vps%E3%81%AE%E4%BA%8C%E5%A4%A7%E4%BB%AE%E6%83%B3%E5%8C%96%E6%8A%80%E8%A1%93%E3%80%8C%E3%82%B3%E3%83%B3%E3%83%86%E3%83%8A%E5%9E%8B%E3%80%8Dvs%E3%80%8Cvm%E5%9E%8B%E3%80%8D%E2%80%95%E2%80%95-%E3%81%9D%E3%81%AE%E9%81%95%E3%81%84%E3%81%A8%E3%83%A1%E3%83%AA%E3%83%83%E3%83%88%E3%83%BB%E3%83%87%E3%83%A1%E3%83%AA%E3%83%83%E3%83%88-37)

### 補足

Dockerは基本的にLinuxベースで動作  
Windowsはもとより、MacのOSもUnixベースでありLinuxではない
準仮想化技術により裏で軽量なLinuxを動かして、その仮想マシン内でDockerを利用

[Docker \- Dockerのコンテナ内で使われるOSについて｜teratail](https://teratail.com/questions/142866)

DockerイメージにUbuntuなどのLinuxOSがあるぞ！
ゲストOSはないんじゃなかったのか？  
→Linuxのコアを共有するので動かせるんです…OSだけどソフトだと思って…ねぇ

## Dockerについて

### 用語

Dockerイメージ
Dockerコンテナ
ボリューム

↓このスライドが比較的わかりやすい

[Dockerイメージの理解とコンテナのライフサイクル](https://www.slideshare.net/zembutsu/docker-images-containers-and-lifecycle)

### Dockerイメージ

コンテナを動かすのに必要となるファイルシステム。
公式に用意されているものがメインだが、何かしらのイメージをベースに自分でも作れる。
php, mysql, wordpress, node, apacheなどなど

[wordpress \- Docker Hub](https://hub.docker.com/_/wordpress)

### Dockerイメージテスト

とりあえずやってみよう

```bash
# 現在ローカルにあるイメージを確認
docker images

# hello-worldコンテナをDockerHubからローカルに取得
docker pull hello-world

# もっかい確認(あるよね？)
docker images

# 実行(もしイメージがローカルになければ、自動でpullする)
docker run hello-world
```

### Dockerコンテナ

Dockerイメージをダウンロード(pull)して実行したが、
イメージは読み取り専用であり、書き込めない。それは困る。

というわけで、読み書き可能なレイヤー(層)を用意する。
それをDockerコンテナと呼ぶのだ！

Dockerコンテナ ＝ Dockerイメージ ＋ 読み書きレイヤー



## その他トピック

### docker run オプション

[【図解】docker runのオプションいろいろ \#マンガでわかるDocker 番外編 \| マンガでわかるWebデザイン](https://webdesign-manga.com/docker_run_option/)

### ライフサイクル

[\[Docker入門\] Dockerコンテナのステータスを調べてみよう \| Skyarch Broadcasting](https://www.skyarch.net/blog/?p=16702#1.2)