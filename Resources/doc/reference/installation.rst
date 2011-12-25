Installation
============

Download bundles
----------------

To begin, add the dependent bundles to the ``vendor/bundles`` directory. Add
the following lines to the file ``deps``
まずはじめに、依存したバンドルを ``vendor/bundles`` ディレクトリに追加してください。
 ``deps`` ファイルに以下を書きたしてくさい。
::

  [SonatajQueryBundle]
      git=http://github.com/sonata-project/SonatajQueryBundle.git
      target=/bundles/Sonata/jQueryBundle

  [SonataAdminBundle]
      git=http://github.com/sonata-project/SonataAdminBundle.git
      target=/bundles/Sonata/AdminBundle

  [KnpMenuBundle]
      git=https://github.com/KnpLabs/KnpMenuBundle.git
      target=/bundles/Knp/Bundle/MenuBundle

  [KnpMenu]
      git=https://github.com/KnpLabs/KnpMenu.git
      target=/knp/menu

and run
そして実行::

  bin/vendors install

Configuration
-------------

Next, be sure to enable the bundles in your autoload.php and AppKernel.php
files:
次に、バンドルを有効化するためautoload.php と AppKernel.phpに追記してください。

.. code-block:: php

  <?php
  // app/autoload.php
  $loader->registerNamespaces(array(
      // ...
      'Sonata'     => __DIR__.'/../vendor/bundles',
      'Knp\Bundle' => __DIR__.'/../vendor/bundles',
      'Knp\Menu'   => __DIR__.'/../vendor/knp/menu/src',
      // ...
  ));

  // app/AppKernel.php
  public function registerBundles()
  {
      return array(
          // ...
          new Sonata\jQueryBundle\SonatajQueryBundle(),
          new Sonata\AdminBundle\SonataAdminBundle(),
          new Knp\Bundle\MenuBundle\KnpMenuBundle(),
          // ...
      );
  }

The bundle also contains several routes. Import them by adding the following
code to your application's routing file:
このバンドルはいくつかのルートを含んでいます。アプリケーションルーティングファイルに以下のコードを
書き足し読み込んで下さい。

.. code-block:: yaml

    # app/config/routing.yml
    admin:
        resource: '@SonataAdminBundle/Resources/config/routing/sonata_admin.xml'
        prefix: /admin

    _sonata_admin:
        resource: .
        type: sonata_admin
        prefix: /admin

Now, install the assets from the different bundles:
``php app/console assets:install web --symlink``.
At this point you can access to the dashboard with the url:
``http://yoursite.local/admin/dashboard``.
異なったバンドルからアセットをインストールします。``php app/console assets:install web --symlink`` 
この時点で、次のURLでダッシュボードにアクセスできます。``http://yoursite.local/admin/dashboard``

.. note::

    If you're using XML or PHP to specify your application's configuration,
    the above configuration and routing will actually be placed in those
    files, with the correct format (i.e. XML or PHP).
   もしアプリケーション設定を記述するためにXMLやPHPを使っていたら、上の設定やルーティングはそれらの
   正しいフォーマットで置き換えてください。

The last important step is security, please refer to the dedicated section.
最後の重要なステップはセキュリティーですが、それは専用のセクションを参照して下さい。

Users management
----------------

By default, the AdminBundle does not come with any user management, however it is most likely the application
requires such feature. The Sonata Project includes a ``SonataUserBundle`` which integrates the ``FOSUserBundle``.
デフォルトでは、Adminbundleはユーザーマネージメント機能はないのですが、しかしながら
アプリケーションはそのような機能を必要としていることでしょう。
Sonata Project は FOSUserBundle が統合された SonataUserBundleを 含んでいます。

The ``FOSUserBundle`` adds support for a database-backed user system in Symfony2. It provides a flexible framework
for user management that aims to handle common tasks such as user login, registration and password retrieval.
FOSUserBundle はsymfony2でデータベース支援されたユーザーシステムを提供します。このバンドルは、
ログイン、登録、パスワード再発行などといっった共通タスクを処理するユーザーマネージメント用の
柔軟なフレームワークです。

The ``SonataUserBundle`` is just a thin wrapper to include the ``FOSUserBundle`` into the ``AdminBundle``. The
``SonataUserBundle`` includes :
SonataUserBundle はFOSUserBundleを含んだ小さなラッパーに過ぎません。
SonataUserBundleは以下の物を含んでいます。

* A default login area
* A default ``user_block`` template which is used to display the current user and the logout link
* 2 Admin class : User and Group
* A default class for User and Group.

* デフォルトのログインエリア
* 現在のユーザーとログアウトリンクを表示するために使われるデフォルトの「user_block」テンプレート
* ２つのAdminクラス：UserとGroup
* UserとGroupのためのデフォルトクラス

There is a little magic in the ``SonataAdminBundle`` if the bundle detects the ``SonataUserBundle`` class, then
the default ``user_block`` template will be changed to use the one provided by the ``SonataUserBundle``.
SonataAdminBundleにはちょっとしたマジックがあり、SonataUserBundleを見つけます、それゆえ
デフォルトのuser_blockテンプレートはSonataUserBundleが準備されると置き換わることでしょう。

The install process is available on the dedicated `SonataUserBundle's documentation area <http://sonata-project.org/bundles/user/master/doc/reference/installation.html>`_
インストール方法はこちらの `SonataUserBundle's documentation エリア<http://sonata-project.org/bundles/user/master/doc/reference/installation.html>'_ まで
