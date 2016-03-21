# ansible_study
ansibleお勉強用

目的 ： [ansibleのbest practice](http://docs.ansible.com/ansible/playbooks_best_practices.html) に従って構築やメンテが出来るようにする

## 実行方法
`ansible -i hosts all -a 'uname' --user=hoge --private-key=~/.ssh/id_rsa -vvvv`

`.ssh/config`をちゃんと書けばuserとprivate-keyの設定オプション指定しなくていいらしい。

## 役割解説
http://yteraoka.github.io/ansible-tutorial/ より役割について

> group_vars/
>
> グループ毎の変数を定義するのに使います、role ではありません、role 毎の変数は roles/{role_name}/vars/ を使います。
> 例えばロケーション毎に異なる変数など。
> ファイル名はインベントリファイルで設定するグループ名です。
> インベントリファイルにグループ変数を書くことも可能です
>
> host_vars/
>
> ホスト毎の変数を定義するのに使います。
> group_vars と同じくファイル名はインベントリファイルに設定したホスト名です。
> インベントリファイルの中でホスト名の右に並べて書くこともできます
>
> roles/
>
> このディレクトリの下に role (役割) 毎のディレクトリを作成し、
> それぞれの role にさらに files/, handlers/, tasks/, templates/, vars/, defaults/, meta/ というサブディレクトリを作成します (その role で使うもののみ)
>
> roles/*/files/
>
> 当該 role の task で使うファイル関連モジュール (copy) が src として使うファイルの置き場所。任意のファイル名で置きます
>
> roles/*/handlers/
>
> 設定変更後にサービスの再起動をさせたりする場合に、notify という定義で処理を呼び出しますが、その呼び出されるハンドラをここで定義します。
> main.yml というファイルで作成しますが、include という定義で複数ファイルをそこから読み込ませることが可能です
>
> roles/*/tasks/
>
> 何かをインストールしたり、ユーザーを作成したりする task 定義のファイルをここに置きます。
> handlers と同じように main.yml というファイルが起点となります
>
> roles/*/templates/
>
> template モジュールで利用するテンプレートファイルを置きます。
> このモジュールでは Jinja2 (神社) というテンプレートエンジンが使われていて .j2 という拡張子を使います
>
> roles/*/vars/
>
> role 毎に設定する変数を定義するファイルを置きます。handlers や tasks と同じく main.yml ファイルが起点となります
>
> roles/*/defaults/
>
> 当該 role で使う変数の default 値を設定します。
> 他で設定されていなければここで設定した値が使われます。
> main.yml に書きます。この変数が一番低い優先度です [「Ansible の変数の優先順」](http://blog.1q77.com/2013/10/ansible-precedence-rules/)
>
> roles/*/meta/
>
> role の依存設定を書きます。A という role が B という role に依存しているなら roles/A/meta/main.yml に次のように書きます
>
> ```
> —-—
> dependencies:
>   - role: B
>     foo: "bar"
> ```
>
> site.yml
>
> ansible-playbook コマンドに渡す大元 (root) の playbook ファイルです
>
> test-servers
>
> 今回のインベントリファイルです。
> 実際の運用では development, stage, production などというそれぞれの環境毎のファイルにするのが Best Practice っぽいです
>
> wordpress.yml
>
> role 毎の対象グループ、対象ホストなどを定義します。今回は wordpress サーバーをセットアップする wordpress role とするため、このファイル名としてあります。site.yml で include されています
