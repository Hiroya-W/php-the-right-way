---
isChild: true
anchor:  opcode_cache
---

## オペコードキャッシュ {#opcode_cache_title}

PHPファイルを実行するときには、まずそれを[オペコード](https://secure.php.net/manual/internals2.opcodes.php) (CPU用の機械語の指示)
にコンパイルしなければいけない。
ソースコードに変更がなければ、オペコードも同じものになる。
ということは、PHP ファイルに変更がなければコンパイル処理は CPU リソースの無駄遣いになるということだ。

オペコードキャッシュは、コンパイル済みのオペコードをメモリに格納し、それ以降の呼び出しで再利用することで、
冗長なコンパイルを回避している。よくあるのは、ファイルのシグネチャや更新時刻をチェックして変更の有無を判断する方式だ。

オペコードキャッシュを使えば、アプリケーションの実行速度が相当向上する可能性がある。
PHP 5.5 からは、 [Zend OPcache][opcache-book] というオペコードキャッシュが標準で組み込まれるようになった。
使っている PHP パッケージやディストリビューションにもよるけど、普通はデフォルトで有効になっていることが多い。
[opcache.enable](https://secure.php.net/manual/opcache.configuration.php#ini.opcache.enable)
や、 `phpinfo()` の出力で確認しよう。
古いバージョンのPHPなら、PECL の拡張モジュールが使える。

オペコードキャッシュについて詳しく知りたければ、以下を参照すること。

* [Zend OPcache][opcache-book] (PHP 5.5 以降に組み込まれている)
* Zend OPcache (元 Zend Optimizer+) は [オープンソースになった][Zend Optimizer+]
* [APC] - PHP 5.4 以前のバージョン用
* [XCache]
* [WinCache] (Microsoft Windows Server 用の拡張)
* [Wikipediaにおける、PHPアクセラレータの一覧][PHP_accelerators]
* [コードの事前ロード] - PHP >= 7.4


[opcache-book]: https://secure.php.net/book.opcache
[APC]: https://secure.php.net/book.apc
[XCache]: https://xcache.lighttpd.net/
[Zend Optimizer+]: https://github.com/zendtech/ZendOptimizerPlus
[WinCache]: https://www.iis.net/downloads/microsoft/wincache-extension
[PHP_accelerators]: https://wikipedia.org/wiki/List_of_PHP_accelerators
[PHP Preloading]: https://www.php.net/opcache.preloading
