---
isChild: true
title: コンパイル形式のテンプレート
anchor: compiled_templates
---

## コンパイル形式のテンプレート {#compiled_templates_title}

PHPはオブジェクト指向言語として成熟してきてはいるものの、テンプレート言語としては
[いまいち][article_templating_engines] だ。
コンパイル形式のテンプレート、たとえば [Twig] や [Brainy] や [Smarty]*
が、この穴を埋めてくれる。テンプレートに特化した、新しい構文を用意してくれるんだ。
自動エスケープから継承や制御構文まで、コンパイル形式のテンプレートは、いかに読み書きしやすく、安心して使えるかを重視して作られている。
さらに、コンパイル形式のテンプレートは、別の言語でさえも使うことができる。[Mustache] がそのよい例だ。
テンプレートをコンパイルする時間がかかるので、多少はパフォーマンスに影響する。
しかし、適切にキャッシュをすれば、その影響は微々たるものだ。

**Smartyには自動エスケープ機能があるけど、これはデフォルトでは無効になっている。*

### コンパイル形式のテンプレートのシンプルな例

[Twig] ライブラリを使った。

{% highlight html+jinja %}
{% raw %}
{% include 'header.html' with {'title': 'User Profile'} %}

<h1>User Profile</h1>
<p>Hello, {{ name }}</p>

{% include 'footer.html' %}
{% endraw %}
{% endhighlight %}

### コンパイル形式のテンプレートで継承を使う例

[Twig] ライブラリを使った。

{% highlight html+jinja %}
{% raw %}
// template.html

<html>
<head>
    <title>{% block title %}{% endblock %}</title>
</head>
<body>

<main>
    {% block content %}{% endblock %}
</main>

</body>
</html>
{% endraw %}
{% endhighlight %}

{% highlight html+jinja %}
{% raw %}
// user_profile.html

{% extends "template.html" %}

{% block title %}User Profile{% endblock %}
{% block content %}
    <h1>User Profile</h1>
    <p>Hello, {{ name }}</p>
{% endblock %}
{% endraw %}
{% endhighlight %}


[article_templating_engines]: http://fabien.potencier.org/templating-engines-in-php.html
[Twig]: https://twig.symfony.com/
[Brainy]: https://github.com/box/brainy
[Smarty]: https://www.smarty.net/
[Mustache]: https://mustache.github.io/
