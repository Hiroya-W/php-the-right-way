---
title: Mac の人は
isChild: true
anchor:  mac_setup
---

## Mac の人は  {#mac_setup_title}

macOS には PHP が最初からインストールされているけど、最新の安定版からは微妙に遅れている。
最新の PHP を macOS にインストールするには、いくつかの方法がある。

### Homebrew によるインストール

[Homebrew] は macOS 用の強力なパッケージ管理ツールで、
PHP やその拡張モジュールも簡単にインストールできる。
Homebrew のコアリポジトリで、PHP 7.4、8.0、8.1、そして PHP 8.2 用の "formulae" が公開されている。

最新版の PHP をインストールするには、こんなコマンドを実行すればいい。

```
brew install php@8.2
```

Homebrew の PHP のバージョンを切り替えるには、環境変数 `PATH` を設定すればいい。
[brew-php-switcher][brew-php-switcher] を使えば、そのへんを自動的にやってくれる。

手動でリンクを消したり、追加したりすることで、使いたい PHP のバージョンに手動で切り替えることもできる。

```
brew unlink php
brew link --overwrite php@8.1
```

```
brew unlink php
brew link --overwrite php@8.2
```

### Macports によるインストール

[MacPorts] プロジェクトはオープンソースのコミュニティによる取り組みで、
OS X 上のオープンソースソフトウェアのコンパイルやインストールそしてアップグレードを簡単にできるようにする仕組みだ。
コマンドラインのソフトからX11やAquaベースのソフトにまで対応している。

MacPorts はコンパイル済みのバイナリにも対応しているので、関連するライブラリなどを毎回ソースからコンパイルしなおす必要はない。
なので、まだ何もパッケージをインストールしていない状態でも、時間の心配をする必要はない。

現時点でインストールできるのは `php54`、`php55`、`php56`、`php70`、`php71`、`php72`、`php73`、`php74`、`php80`、`php81`、`php82` のいずれかで、`port install` コマンドを使ってこのようにインストールする。

    sudo port install php74
    sudo port install php82

そして、`select` コマンドを使って、アクティブな PHP を切り替える。

    sudo port select --set php php82

### phpbrew によるインストール

[phpbrew] は、複数のバージョンの PHP をインストールして管理するためのツールだ。
使っているアプリケーションやプロジェクトによって、PHP のバージョンが異なる場合に特に便利で、
仮想マシンを用意する必要がなくなる。

### Liip のバイナリインストーラーによる PHP のインストール

[php-osx.liip.ch] は、たった一行で PHP をインストールできる方法だ。バージョン 5.3 から 7.3 に対応している。
Apple がインストールした php のバイナリは上書きせず、まったく別の場所 (/usr/local/php5) にすべてをインストールする。

### ソースからのコンパイル

もうひとつの選択肢がある。 [自分でコンパイル][mac-compile]
することだ。この方法ならインストールする PHP のバージョンを完全にコントロールできる。
この場合は、Xcode あるいはその代用ツール ["Command Line Tools for XCode"]
をインストールする必要がある。これらは、 Apple の Mac Developer Center からダウンロードできる。

### 全部入りのインストーラー

ここまでで説明した方法は PHP そのものに関するものばかりで、それ以外のたとえば [Apache][apache]、[Nginx][nginx]、SQL server などは扱っていない。
[MAMP][mamp-downloads] や [XAMPP][xampp] みたいな「全部入り」のインストーラーを使えば、このあたりのソフトも一緒に入れられるし設定もしてくれる。かんたん。
でも、インストーラーに縛られてしまって自由にいじれなくなるという弱点もある。

[Homebrew]: https://brew.sh/
[Homebrew PHP]: https://github.com/Homebrew/homebrew-php#installation
[MacPorts]: https://www.macports.org/install.php
[phpbrew]: https://github.com/phpbrew/phpbrew
[php-osx.liip.ch]: https://web.archive.org/web/20220505163210/https://php-osx.liip.ch/
[mac-compile]: https://www.php.net/install.macosx.compile
[xcode-gcc-substitution]: https://github.com/kennethreitz/osx-gcc-installer
["Command Line Tools for XCode"]: https://developer.apple.com/downloads
[apache]: https://httpd.apache.org/
[nginx]: https://www.nginx.com/
[mamp-downloads]: https://www.mamp.info/en/downloads/
[xampp]: https://www.apachefriends.org/
[brew-php-switcher]: https://github.com/philcook/brew-php-switcher
