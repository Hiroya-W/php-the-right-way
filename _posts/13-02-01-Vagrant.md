---
isChild: true
anchor:  vagrant
---

## Vagrant {#vagrant_title}

[Vagrant]を使えば、既知の仮想環境を使って自分用のボックスを作れて、その環境の構成も、たったひとつの設定ファイルだけでできる。
このボックスを手動で設定することもできるし、
[Puppet]とか[Chef]みたいな「プロビジョニング」ソフトにおまかせすることだってできる。
ベースとなる環境を配布できるようにしておけば、複数の開発環境をまったく同じ状態に構築できる。
複雑怪奇なコマンドを羅列した「環境構築手順書」だとかいうのもいらなくなるってこと。
ベース環境を「破棄」したり作りなおしたりするのもそんなに手間がかからないので、
まっさらな環境を用意するのもお手軽にできる。

Vagrantは、フォルダを作って、ホストと仮想マシンの間でコードを共有する。
つまり、ホストマシンで作ったり編集したりしたコードを、そのまま仮想マシンの中で実行できるっていうことだ。

### ちょっとした手助け

Vagrantを使い始めるときの手助けになるのが、これらのサービスだ。

- [Puphpet][Puphpet]: PHPの開発用の仮想マシンを作ってくれる、シンプルなGUI。
  **PHPに特化している。**
  ローカルVM以外に、クラウドサービスにデプロイすることもできる。
  プロビジョニングにはPuppetを使う。
- [Phansible][Phansible]: 使いやすいインターフェイスで、PHP プロジェクト用の Ansible Playbook を生成してくれる。

[Vagrant]: https://www.vagrantup.com/
[Puppet]: https://puppet.com/
[Chef]: https://www.chef.io/
[Puphpet]: https://github.com/puphpet/puphpet
[Phansible]: http://phansible.com/
