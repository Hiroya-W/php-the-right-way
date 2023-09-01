---
title:   国際化とローカライズ
isChild: true
anchor:  i18n_l10n
---

## 国際化(i18n) とローカライズ(l10n) {#i18n_l10n_title}

_初学者の皆さんへのお断り: i18n と l10n はヌメロニム(訳者注: 長い英単語を数字で省略して表現する語)であり、短縮に数字が使われる省略形の一種である - この場合、internationalization は i18n、localization は l10n となる。_

まず初めに、これら2つの似た概念と、その他関連するものを定義する必要がある。

- **国際化** とは、コードを構成して、リファクタリングせずとも様々な言語や地域に適応できるようにすることだ。このアクションは、通常一度（可能であれば、プロジェクトの最初に）実行される。さもないと、おそらくソースに大きな変更を加える必要が生じてしまうだろう！
- **ローカライズ** は、以前に行われた i18n の作業に基づいて、コンテンツを翻訳することによって（主に）インターフェースを適応させると発生する。これは通常、新しい言語や地域がサポートを必要とされるたびに都度行われる。サポートされるすべての言語で利用できる必要があるので、新しいインタフェース部分が追加されるたびに更新される。
- **複数化** は、数字やカウンタを含む文字列を相互運用するために、異なる言語間で必要とされる規則を定義する。例えば英語では、要素が1つしかない場合は単数形、それ以外は複数形と呼ばれる。英語の複数形は単語の後ろにSを追加することにより示され、時には単語の一部を変更することもある。
ロシア語やセルビア語などの他の言語では、単数形に加えて、2つの複数形がある。スロベニア語やアイルランド語、アラビア語など、合計4つや5つ、あるいは6つの複数形を持つ言語もある。

## 一般的な実装方法

PHP で書かれたソフトウェアを国際化する最も簡単な方法は、配列ファイルを使ってそれらの文字列を `<h1><?=$TRANS['title_about_page']?></h1>` のように、テンプレートで使うことだ。しかしながら、運用していく中でメンテナンスの問題が発生するので、この方法は本格的なプロジェクトでは全くお勧めできない。複数化など、プロジェクトの最初期に発生する問題もある。したがって、何ページも含まれるプロジェクトであれば、この方法を試さないでいただきたい。

最も古典的で、しばしば i18n や l10n の参考にされることが多いのは、[`gettext`というUnixのツール][gettext] である。これは1995年までさかのぼり、ソフトウェアを翻訳するための完璧な実装である。十分かんたんに実行でき、強力なサポートツールを備えている。ここでは Gettext に関して説明する。また、コマンドラインを混乱させないように、l10n のソースを簡単に更新できる優れたGUIアプリケーションを紹介する。

### その他のツール

Gettext やその他の i18n の実装をサポートする一般的なライブラリが存在する。それらの中には、インストールが簡単なものや、追加の機能や i18n ファイルフォーマットを備えているものもある。本ドキュメントでは、PHPコアで提供されるツールにフォーカスするが、完全を期すために、以下で他のツールを列挙する:

- [aura/intl][aura-intl]: 国際化(I18N) ツール、特に、パッケージ指向のメッセージ変換をロケール毎に提供する。メッセージには配列形式を利用する。メッセージ抽出器は提供しないが、`intl` エクステンション（複数化されたメッセージを含む）を介した高度なメッセージフォーマットを提供する。
- [php-gettext/Gettext][php-gettext]: オブジェクト指向インタフェースでの Gettext サポート。改良されたヘルパー関数、いくつかのファイルフォーマットのための強力な抽出器（`gettext` コマンドがネイティブにサポートしないものもある）を備え、`.mo/.po` ファイル以外の他のフォーマットにエクスポートすることもできる。翻訳ファイルをJavaScriptインターフェースなどのシステムの他の部分に統合する必要がある場合に役に立つ。
- [symfony/translation][symfony]: 様々なフォーマットをサポートするが、冗長な XLIFF を使うのがお勧めだ。ヘルパー関数や組み込みの抽出器は含まれないが、内部的に `strtr()` を使ってプレースホルダをサポートする。
- [laminas/laminas-i18n][laminas]: 配列ファイルとINIファイル、あるいは Gettext フォーマットをサポートする。キャッシュレイヤを実装していて、ファイルシステムを毎回読み込む手間を削減する。また、ビューヘルパーやロケール対応した入力フィルタやバリデータも含まれている。しかしながら、メッセージ抽出器はない。

他のフレームワークにも i18n モジュールが含まれているが、それらはフレームワークを使ったコードベース以外では利用できない。

- [Laravel] 基本的な配列ファイルをサポートする。自動抽出器はないが、テンプレートファイル向けに`@lang` ヘルパーが用意されている。
- [Yii] は配列、Gettext、データベースに基づく翻訳をサポートし、メッセージ抽出器が含まれている。PHP 5.3 以降で利用可能な [`Intl`][intl] 拡張を基盤としていて　[ICUプロジェクト] に基づいている。これにより、Yii は、数字のスペルや、日付、時刻、時間、通貨、序数の書式設定などの強力な置換を実行することができる。

抽出器が提供されていないライブラリの１つを選択する場合は、gettext フォーマットを使うのが良いだろう。そうすれば、本章の以降で説明するように、オリジナルの gettext ツールチェイン(Poedit を含む) を使うことができる。

## Gettext

### インストール

`apt-get` や `yum`などのパッケージマネージャを使って、Gettext や関連するPHPライブラリをインストールする必要があるかもしれない。インストール後、`extension=gettext.so` (Linux/Unixの場合)、あるいは `extension=php_gettext.dll` (Windowsの場合)を `php.ini` に追加して有効にする。

ここでは、[Poedit] を使って翻訳ファイルを作成する。おそらくシステムのパッケージマネージャにもあると思う。Unix, macOS, Windows で使うことができ、[ウェブサイトから無料でダウンロード][poedit_download] することもできる。

### 構造

#### ファイルの型

gettext を使う際、通常扱うファイルは3つある。主なファイルは PO(Portable Object) ファイルと、MO(Machine Object) ファイルだ。前者は読み取り可能な「翻訳されたオブジェクト」のリストであり、後者は、ローカライズを行う際に、gettext によって解釈される対応するバイナリだ。これ以外に、POT (Template) ファイルがある。これはソースファイルからの既存のキーを全て含んでいて、すべてのPOファイルを生成、更新するためのガイドとして利用できる。これらのテンプレートファイルは必須ではなく、l10n を行う際に用いるツールによっては、PO/MO ファイルだけで事足りる。言語と地域ごとにPO/MO ファイルのペアが常に１つあるが、POT はドメインごとに１つだけだ。

### ドメイン

大きなプロジェクトでは、同じ単語がコンテキストに応じて異なる意味を伝える場合、翻訳を分離する必要がある場合がある。そうした場合、それらを別の _ドメイン_ に分割する。それらは基本的に、POT/PO/MOファイルの名前付きグループであり、その場合のファイル名は前述の _翻訳ドメイン_ である。中小規模のプロジェクトでは通常、単純化するために、１つのドメインのみを使用する。その名前は任意だが、我々のコードサンプルでは「main」を使う。[Symfony] プロジェクトでは、例えば、ドメインはバリデーションメッセージの翻訳を分離するために利用される。

#### ロケールコード

ロケールは単に、言語のあるバージョンを識別するコードであり、[ISO 639-1][639-1] や [ISO 3166-1 alpha2][3166-1] 仕様に沿って定義される。言語には2つの小文字、必要に応じて下線と国や地域のコードを識別する2つの大文字が続く。[レアな言語][rare] には、3文字が使われる。

一部の話者にとっては、国の部分が冗長に思える場合がある。実際、言語の中には、オーストリアドイツ語(`de_AT`) や (ブラジルで話される)ポルトガル語(`pt_BR`) のように、異なる国の方言を持つものがある。2つ目の部分は、これらの方言を区別するために使われる。2つ目の部分がない場合、言語の「汎用」バージョン、あるいは「ハイブリッド」バージョンとして扱われる。

### ディレクトリ構造

Gettext を使うには、特定のフォルダ構造に従う必要がある。まず、ソースリポジトリ内の l10n ファイルに対して任意のルートを選択する必要がある。その中に、必要なロケール毎のフォルダと、すべてのPO/MOペアを含む、固定の `LC_MESSAGES` フォルダを用意する。たとえば、以下のようにする： 

{% highlight console %}
<project root>
 ├─ src/
 ├─ templates/
 └─ locales/
    ├─ forum.pot
    ├─ site.pot
    ├─ de/
    │  └─ LC_MESSAGES/
    │     ├─ forum.mo
    │     ├─ forum.po
    │     ├─ site.mo
    │     └─ site.po
    ├─ es_ES/
    │  └─ LC_MESSAGES/
    │     └─ ...
    ├─ fr/
    │  └─ ...
    ├─ pt_BR/
    │  └─ ...
    └─ pt_PT/
       └─ ...
{% endhighlight %}

### 複数形

この章の「はじめに」で述べたように、言語が異なれば、複数形の規則も異なる。しかしながら、gettext を 使えば、この問題は二度と起こらなくなる。新しい`.po`ファイルを作成すると、その言語のための [複数形ルール][plural] を宣言する必要があり、複数形を区別する必要のある翻訳された部分はそれらのルールに対して異なる形式を持つ。コードの中からGettext を呼び出す場合、その文に関連する番号を指定する必要がある。また、必要に応じて文字列置換を使っていても、使用する正しい形式が決定される。

複数形のルールには使用可能な複数の数と、指定された数がどのルールに該当するかを定義する (カウントは0から始まる) `n` を用いたブーリアン型のテストが含まれる。例：

- 日本語: `nplurals=1; plural=0` - ルールは1つだけ
- 英語：`nplurals=2; plural=(n != 1);` - ルールは2つあり、Nが1の場合は1つ目
- (ブラジルで話される)ポルトガル語: `nplurals=2; plural=(n > 1);` - ルールは2つあり、Nが1より大きい場合は2つ目、そうでなければ1つ目

これで、どのように複数形ルールが機能するかの基礎を理解した。まだ理解できていないのであれば、[LingoHubチュートリアル][lingohub_plurals] の詳しい説明を参照していただきたい。必要なものを手で書くのではなく、[リスト][plural]からコピーしたいと思うかもしれない。

カウンタを使って文のローカライズを行うために Gettext を呼び出す場合、関連する数も同様に用意する必要がある。Gettext はどのルールが反映されるべきかを判断し、正しいローカライズされたバージョンを使用する。定義されたそれぞれの複数形ルール毎に異なる文を `.po` ファイルに含める必要がある。

### 実装例

理論面が一通り終わったら、少し実用的なことを扱おう。ここに、`.po` ファイルの抜粋がある。フォーマットについては気にしないでいただきたいが、代わりに全体的な内容を示す。後で簡単に編集する方法を学ぼう。

{% highlight po %}
msgid ""
msgstr ""
"Language: pt_BR\n"
"Content-Type: text/plain; charset=UTF-8\n"
"Plural-Forms: nplurals=2; plural=(n > 1);\n"

msgid "We are now translating some strings"
msgstr "Nós estamos traduzindo algumas strings agora"

msgid "Hello %1$s! Your last visit was on %2$s"
msgstr "Olá %1$s! Sua última visita foi em %2$s"

msgid "Only one unread message"
msgid_plural "%d unread messages"
msgstr[0] "Só uma mensagem não lida"
msgstr[1] "%d mensagens não lidas"
{% endhighlight %}

最初のセクションはヘッダのように機能し、`msgid` と `msgstr` は特別に空っぽだ。これはファイルエンコーディング、複数形、その他あまり関係ないことを説明している。
2つ目のセクションは単純な文字列を英語から(ブラジルで話される)ポルトガル語へ翻訳している。3つ目のセクションも同じことをしているが、[`sprintf`][sprintf]からの文字列置換を使って、翻訳にユーザ名と訪問日が含まれるようにする。
最後のセクションは複数化形式の例で、単数と複数のバージョン英語で`msgid`として、対応する翻訳を`msgstr` 0と1として表示している（複数形ルールで指定された数字に続く）。ここで、文字列置換も使われているので、数字は`%d`を使って文の中で直接参照することができる。複数形は常に2つの`msgid`(単数と複数) があるので、翻訳のソースとして複雑な言語を用いないことをお勧めする。 

### l10n のキーに関する議論

お気づきかもしれないが、我々は英語の実際の文をソースIDとして使っている。その `msgid` はすべての`.po` ファイルで同じように使われている。これはつまり、他の言語も同じフォーマットと同じ`msgid`フィールドを持っているが、`msgstr`行は翻訳されているということだ。

翻訳キーに関していえば、2つの主要な「流派」がある。

1. _実際の文としての `msgid`_.
    主な利点は下記の通り：
    - 特定の言語で翻訳されていないソフトウェアがある場合、表示されるキーは何らかの意味を保持している。例：英語からスペイン語に翻訳したが、フランス語に翻訳するのに助けを必要とする場合、フランス語の文が欠けた新しいページを公開すると、そのウェブサイトの一部が代わりに英語で表示される。
    - 翻訳者にとっては、何が起こっているかを理解し、`msgid` に基づいて適切な翻訳を行う方がはるかに簡単だ。
    - ある言語（ソース言語）に対して、「無料の」l10n を提供する。
    - 唯一の欠点は、実際のテキストを変更する必要がある場合、複数の言語ファイルで同じ `msgid` を置換する必要があることだ。

2. _一意な構造化されたキーとしての `msgid`_
これはアプリケーション内の文の役割を、構造化された方法で記述する。これには、文字列の内容ではなく、文字列が配置されたテンプレート、あるいはその一部が含まれている。
    - テキストの内容をテンプレートのロジックから分離して、コードを整理するにはすばらしい方法だ。
    - しかしながら、翻訳者がコンテキストを見逃す問題が発生するかもしれない。ソース言語ファイルは、他の翻訳の基礎として必要になる。例：理想的な形では、開発者は `en.po` ファイルを管理し、翻訳者は、例えば `fr.po` に何を書くかを理解するためにそれを読む。
    - 翻訳がないと、意味のないキーが画面に表示される(前述の翻訳されていないフランス語のページでは、`Hello there, User!` ではなく、`top_menu.welcome` が表示される)。 公開前に無理やり翻訳を完成させるのはよいことだが、翻訳の問題は画面上で非常にひどいものになるだろう。しかしながら、ライブラリの中には、他のアプローチと同様の振る舞いをもつ特定の言語を「フォールバック」として指定するオプションが含まれているものもある。

[Gettextのマニュアル][manual] では、通常、翻訳者やユーザがトラブルに見舞われた場合に簡単に対処できるように、最初のアプローチを採用している。なのでここでもそのような作業を行う。しかしながら、[Symfonyドキュメント][symfony-keys] では、キーワードに基づく翻訳を採用していて、テンプレートに影響を及ぼすことなくすべての翻訳を個別に変更できるようにしている。

### お決まりの使い方

一般的なアプリケーションでは、ページに静的なテキストを書く際にGettext関数を用いる。これらの文は、`.po` ファイルに現れ、翻訳され、`.mo` ファイルにコンパイルされ、実際のインタフェースをレンダリングを行う際にGettext によって使われる。なので、これまで議論してきた内容を段階的な例でまとめてみよう。

#### 1. いくつかの異なる gettext 呼び出しを含むサンプルテンプレートファイル

{% highlight php %}
<?php include 'i18n_setup.php' ?>
<div id="header">
    <h1><?=sprintf(gettext('Welcome, %s!'), $name)?></h1>
    <!-- code indented this way only for legibility -->
    <?php if ($unread): ?>
        <h2><?=sprintf(
            ngettext('Only one unread message',
                     '%d unread messages',
                     $unread),
            $unread)?>
        </h2>
    <?php endif ?>
</div>

<h1><?=gettext('Introduction')?></h1>
<p><?=gettext('We\'re now translating some strings')?></p>
{% endhighlight %}

- [`gettext()`][func] は、指定された言語に対して、`msgid` を対応する `msgstr` に変換する。同様に動作する簡単な関数 `_()` もある。
- [`ngettext()`][n_func] は同じことをするが、複数形ルールを適用する。
- [`dgettext()`][d_func] や、[`dngettext()`][dn_func] もあり、１回の呼び出しでドメインを上書きできるようにする。ドメイン設定の詳細については、次の例を参照していただきたい。

#### 2. サンプルセットアップファイル(上で用いた `i18n_setup.php`)、正しいロケールの選択や、Gettext の設定を行う

{% highlight php %}
<?php
/**
 * Verifies if the given $locale is supported in the project
 * @param string $locale
 * @return bool
 */
function valid($locale) {
   return in_array($locale, ['en_US', 'en', 'pt_BR', 'pt', 'es_ES', 'es']);
}

//setting the source/default locale, for informational purposes
$lang = 'en_US';

if (isset($_GET['lang']) && valid($_GET['lang'])) {
    // the locale can be changed through the query-string
    $lang = $_GET['lang'];    //you should sanitize this!
    setcookie('lang', $lang); //it's stored in a cookie so it can be reused
} elseif (isset($_COOKIE['lang']) && valid($_COOKIE['lang'])) {
    // if the cookie is present instead, let's just keep it
    $lang = $_COOKIE['lang']; //you should sanitize this!
} elseif (isset($_SERVER['HTTP_ACCEPT_LANGUAGE'])) {
    // default: look for the languages the browser says the user accepts
    $langs = explode(',', $_SERVER['HTTP_ACCEPT_LANGUAGE']);
    array_walk($langs, function (&$lang) { $lang = strtr(strtok($lang, ';'), ['-' => '_']); });
    foreach ($langs as $browser_lang) {
        if (valid($browser_lang)) {
            $lang = $browser_lang;
            break;
        }
    }
}

// here we define the global system locale given the found language
putenv("LANG=$lang");

// this might be useful for date functions (LC_TIME) or money formatting (LC_MONETARY), for instance
setlocale(LC_ALL, $lang);

// this will make Gettext look for ../locales/<lang>/LC_MESSAGES/main.mo
bindtextdomain('main', '../locales');

// indicates in what encoding the file should be read
bind_textdomain_codeset('main', 'UTF-8');

// if your application has additional domains, as cited before, you should bind them here as well
bindtextdomain('forum', '../locales');
bind_textdomain_codeset('forum', 'UTF-8');

// here we indicate the default domain the gettext() calls will respond to
textdomain('main');

// this would look for the string in forum.mo instead of main.mo
// echo dgettext('forum', 'Welcome back!');
?>
{% endhighlight %}

#### 3. 最初に実行するための翻訳の準備

Gettext が独自フレームワークの i18n パッケージに対して持っている大きな利点の１つは、広範で強力なファイルフォーマットだ。「ああ、Gettext のフォーマットを理解し、手で編集することは非常に困難だ。単純な配列の方が簡単だ！」間違いなく、[Poedit] のようなアプリケーションが _大いに_ 助けになる。[彼らのウェブサイト][poedit_download]からプログラムを入手することができ、すべてのプラットフォームで無料で利用することができる。これは、簡単に使えるツールであり、同時に非常に強力なツールである - なぜなら、Gettext が使えるすべての機能を使っているからだ。このガイドは PoEdit 1.8 に基づいている。

最初の実行では、メニューから、「File > New...」を選択する必要がある。言語は直接入力するように求められる。ここで翻訳したい言語を選択／フィルタするか、`en_US` や `pt_BR` のような、前述した形式を使用できる。

次に、先に説明したディレクトリ構造を使ってファイルを保存する。さらに、「Extract from sources」をクリックし、ここで抽出タスクと翻訳タスクの様々な設定を行う。「Catalog > Properties」で、後からこれらすべてを見つけることができるようになる。

- ソースパス：ここには、`gettext()`(とその兄弟) が呼び出されるプロジェクトのすべてのフォルダを含める必要がある。これは通常はtemplates/views フォルダだ。これは唯一の必須設定項目である。
- トランザクションプロパティ：
    - プロジェクト名とバージョン、チーム、およびチームのメールアドレス、.poファイルヘッダに含まれる有用な情報
    - 複数形：前述したルールを参照してほしい。サンプルへのリンクもそこにある。ほとんどの場合、PoEdit にはすでに多くの言語の複数形ルールの手頃なデータベースが含まれているので、デフォルトオプションのままにしておいて良い。
    - 文字セット： UTF-8 (推奨)
    - ソースコードの文字セット：コードベースで使っている文字セットをここに設定する。おそらく、UTF-8ってことでいいよね？
- ソースキーワード：基盤となるソフトウェアは、`gettext()` や同様の関数呼び出しがいくつかのプログラミング言語でどのように見えるかを知っているが、独自の翻訳関数を作成する方が良い場合もある。ここでは、そんな別のメソッドを追加する。これについては、後述する「ヒント」の章で説明する。

これらのポイントを設定した後、PoEditはソースファイルをスキャンしてローカライズの呼び出しを全て洗い出す。スキャンするたびに、PoEdit は検出されたものとソースファイルから削除された内容のサマリを表示する。新しいエントリは空のまま変換テーブルに入力され、ローカライズされたバージョンの文字列を入力し始める。それが保存されると、.mo ファイルは同じフォルダに（再び)コンパイルされ、プロジェクトが国際化される。

#### 4. 翻訳文字列

すでにお気づきかもしれないが、ローカライズされた文字列には、2つの主なタイプがある。単数のものとそれを複数形にしたものだ。1つ目は、ソースとローカライズされた文字列の2つのボックスがあるだけだ。Gettext/Poedit にはソースファイルを変更する権限がないので、ソース文字列は変更できない。ソース自体を変更して、再スキャンする必要がある。ヒント：翻訳行を右クリックすると、その文字列が使われているソースファイルと行のヒントが表示される。
他方、複数形文字列には2つのソース文字列を表示する2つのボックスと、様々な最終形式が設定可能なタブが含まれている。

ソースを変更して翻訳を更新する必要がある場合はいつでも、Refresh をクリックするだけで Poedit はコードを再スキャンし、存在しないエントリを削除し、変更されたエントリをマージして新しいエントリを追加する。あなたが行った他の翻訳に基づいて、いくつかの翻訳を推測しようとする場合もある。それらの推論と変更されたエントリには「Fuzzy」マーカーが付与され、検証が必要であることが示され、リスト中に金色で表示される。もし翻訳チームがいて、誰かが不確かなことを書こうとする場合にも便利だ。Fuzzy をマークするだけで、他の誰かが後からレビューしてくれるからだ。

最後に、「View > Untranslated entries first」マークをつけておくことをお勧めする。これは、エントリを忘れないようにするのに _大いに_ 役に立つからだ。このメニューから、必要に応じて、翻訳者たちにコンテキストの情報を残すことができる UI をオープンすることもできる。

### ヒントとコツ

#### キャッシング問題の可能性

PHP を Apache 上のモジュール(`mod_php`) として実行している場合、`.mo` ファイルがキャッシュされる問題に直面するかもしれない。これは最初に読み込まれた時に発生し、更新するには、サーバを再起動する必要がある。Nginx と PHP5 では通常、翻訳キャッシュを更新するのに、数ページを更新するだけでよく、PHP7 ではほとんど必要ない。

#### 付加的なヘルパー関数

多くの人々に好まれているように、`gettext()` の代わりに、`_()` を使う方が簡単だ。フレームワークに備わっている多くのカスタム i18n ライブラリも、変換されたコードを短くするために`t()` に似たものを使っている。しかしながら、これはショートカットを備えた唯一の関数だ。`ngettext()` のための`__()`や`_n()`をプロジェクトに追加したり、華美な`_r()` を`gettext()` や `sprintf()`の呼び出しに追加したくなるかもしれない。[php-gettextのGettext][php-gettext] のような他のライブラリでも、これらのようなヘルパー関数が提供されている。

そうした場合、それらの新しい関数から文字列を抽出する方法をGettextユーティリティに指示する必要がある。しかし恐れる必要はない。とても簡単だ。これは `.po` ファイルにあるフィールド、あるいは Poedit の設定画面だ。エディタでは、そのオプションは「Catalog > Properties > Source keywords」の中にある。覚えておいてほしいのは、Gettext は多くの言語のためのデフォルト関数がすでにわかっているので、そのリストが空っぽのように見えても恐れる必要はない、ということだ。[特定の形式][func_format] に従って、これらの新しい関数の仕様を含める必要がある。

- 単に文字列の翻訳を返す`t()`のようなものを作成する場合、`t` と指定することができる。
Gettextは関数の引数が翻訳されるべき文字列であると認識する.
- 関数に複数の引数がある場合、最初の文字列がどれであるかを指定することができる。また、必要に応じて、複数形も指定することができる。例えば、以下のように：`__('one user', '%d users', $number)` 関数を呼び出すと、その指定は`__:1,2` になり、これは最初の形式が1つ目の引数で、2つめの形式が2つ目の引数であることを意味する。代わりに、数字が最初の引数として指定された場合、指定は、`__:2,3`となり、最初の形式が2つ目の引数であること示す、という具合だ。

これらの新しいルールを`.po` ファイルに含めたあと、新たにスキャンを実行すると、以前と同様、簡単に新しい文字列が取り込まれる。

### リファレンス

* [Wikipedia: i18n and l10n](https://en.wikipedia.org/wiki/Internationalization_and_localization) 
* [Wikipedia: Gettext](https://en.wikipedia.org/wiki/Gettext)
* [LingoHub: PHP internationalization with gettext tutorial][lingohub]
* [PHP Manual: Gettext](https://www.php.net/manual/book.gettext.php)
* [Gettext Manual][manual]

[Poedit]: https://poedit.net
[poedit_download]: https://poedit.net/download
[lingohub]: https://lingohub.com/blog/2013/07/php-internationalization-with-gettext-tutorial/
[lingohub_plurals]: https://lingohub.com/blog/2013/07/php-internationalization-with-gettext-tutorial/#Plurals
[plural]: https://docs.translatehouse.org/projects/localization-guide/en/latest/l10n/pluralforms.html
[gettext]: https://en.wikipedia.org/wiki/Gettext
[manual]: https://www.gnu.org/software/gettext/manual/gettext.html
[639-1]: https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes
[3166-1]: https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2
[rare]: https://www.gnu.org/software/gettext/manual/gettext.html#Rare-Language-Codes
[func_format]: https://www.gnu.org/software/gettext/manual/gettext.html#Language-specific-options
[aura-intl]: https://github.com/auraphp/Aura.Intl
[php-gettext]: https://github.com/php-gettext/Gettext
[symfony]: https://symfony.com/components/Translation
[laminas]: https://docs.laminas.dev/laminas-i18n/
[laravel]: https://laravel.com/docs/master/localization
[yii]: https://www.yiiframework.com/doc/guide/2.0/en/tutorial-i18n
[intl]: https://www.php.net/manual/intro.intl.php
[ICUプロジェクト]: https://icu.unicode.org/
[symfony-keys]: https://symfony.com/doc/current/translation.html#using-real-or-keyword-messages

[sprintf]: https://www.php.net/manual/function.sprintf.php
[func]: https://www.php.net/manual/function.gettext.php
[n_func]: https://www.php.net/manual/function.ngettext.php
[d_func]: https://www.php.net/manual/function.dgettext.php
[dn_func]: https://www.php.net/manual/function.dngettext.php
