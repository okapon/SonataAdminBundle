Architecture
============

The architecture of the SonataAdminBundle is primarily inspired by the Django Admin
Project, which is truly a great project. More information can be found at the
`Django Project Website`_.
このSonataAdminBundleのアーキテクチャーは、主にDjango Admin Project にインスパイアされており、
それはとても大きなプロジェクトです。より多くの情報はDjango ProjectのWebサイトから手に入ります。

The Admin Class
---------------

The ``Admin`` class represents the CRUD definition for a specific model. It
contains all the configuration necessary to display a rich CRUD interface for
the entity.
Adminクラスは個々のモデルに対してCRUD定義を表します。それ(Adminクラス)はEntityに対して
リッチなCRUDインターフェースを表示するために必要な設定を全て含んでいます。

Within the admin class, the following information can be defined:
Adminクラス内部で、以下の情報を定義することができます。

* ``list``: The fields displayed in the list table;
* ``filter``: The fields available for filtering the list;
* ``form``: The fields used to edit the entity;
* ``show``: The fields used to show the entity;
* Batch actions: Actions that can be performed on a group of entities
  (e.g. bulk delete)

* ``list``: リストテーブル内で表示されるフィールド
* ``filter``: リストをフィルタリングできるフィールド
* ``form``: Entityを編集するためのフィールド
* ``show``: Entityを表示するたものフィールド
* Batch actions: エンティティグループに対して機能するアクション
  (例えば、たくさんのdelete)

If a field is associated with another entity (and that entity also has an
``Admin`` class), then the related ``Admin`` class will be accessible from
within the first class.
もしフィールドが他のエンティティと関連性がある場合、（そしてそのエンティティもAdminクラスを
もっている場合）関連したAdminクラスは一つ目のクラス内で利用しやすいようになります。

The admin class is a service implementing the ``AdminInterface`` interface,
meaning that the following required dependencies are automatically injected:
adminクラスは AdminInterface インターフェースを実装しているサービスであり、
以下の必須依存関係が自動的に注入されることを意味しています。

* ``ListBuilder``: builds the list fields
* ``FormContractor``: builds the form using the Symfony ``FormBuilder``
* ``DatagridBuilder``: builds the filter fields
* ``Router``: generates the different urls
* ``Request``
* ``ModelManager``: Service which handles specific ORM code
* ``Translator``

* ``ListBuilder``: リストフィールをを組み立てる
* ``FormContractor``: Symfonyの``FormBuilder``を使ってフォームを組み立てる
* ``DatagridBuilder``: フィルターフィールドを組み立てる
* ``Router``: 異なったURLを生成する
* ``Request``
* ``ModelManager``: 個別のORMコードを取り扱うサービス
* ``Translator``

Therefore, you can gain access to any service you want by injecting them into
the admin class, like so:
それゆえ、Adminクラス内に注入することによってあなたが望むどんなサービスでもアクセスすることが
できます。以下のように。

.. code-block:: xml

    <service id="sonata.news.admin.post" class="%sonata.news.admin.post.class%">
        <tag name="sonata.admin" manager_type="orm" group="sonata_blog" label="post"/>
        <argument />
        <argument>%sonata.news.admin.post.entity%</argument>
        <argument>%sonata.news.admin.post.controller%</argument>

        <call method="setUserManager">
            <argument type="service" id="fos_user.user_manager" />
        </call>

    </service>

Here, the FOS' User Manager is injected into the Post service.
ここでは、FOS' User Manager はPostサービスに注入されている。

Field Definition
----------------

A field definition is a ``FieldDescription`` object. There is one definition per list
field.
フィールド定義は ``FieldDescription`` オブジェクト。リストフィールドごとに一つの定義があります。

The definition contains:
この定義は以下を含んでいます

* ``name``: The name of the field definition;
* ``type``: The field type;
* ``template``: The template used for displaying the field;
* ``targetEntity``: The class name of the target entity for relations;
* ``options``: Certain field types have additional options;

* ``name``: フィールド定義の名前
* ``type``: フィールドのタイプ
* ``template``: フィールドを表示する為に使われるテンプレート
* ``targetEntity``: リレーションのためのターゲットエンティティのクラス名
* ``options``: あるフィールドタイプは追加のオプションを持っている

Template Configuration
----------------------

The current implementation uses Twig as the template engine. All templates
are located in the ``Resources/views/CRUD`` directory of the bundle. The base
template extends two layouts:
現在の実装はTwigをテンプレートエンジンとして使っています。全てのテンプレートはbundle内の
 ``Resources/views/CRUD`` デレクトリ内に格納されています。

* ``AdminBundle::standard_layout.html.twig``
* ``AdminBundle::ajax_layout.html.twig``

The base templates can be configured in the Service Container. So you can easily tweak
the layout to suit your requirements.
ベーステンプレートはサービスコンテナで設定することができます。ですので、簡単に
要求に適したレイアウトに調整することができます。

Each field is rendered in three different ways and each has its own Twig
template. For example, for a field with a ``text`` type, the following three
templates will be used:
それぞれのフィールドは３つの異なった方法で描画され、それぞれが自身のTwigテンプレートを持って
います。例えば、 ``text`` タイプのフィールドは、以下の３つのテンプレートが使われています。

* ``filter_text.twig``: template used in the filter box
* ``list_text.twig``: template used in the list table

* ``filter_text.twig``: フィルターボックス内で使われるテンプレート
* ``list_text.twig``: リストテーブル内で使われるテンプレート

CRUDController
--------------

The controller contains the basic CRUD actions. It is related to one
``Admin`` class by mapping the controller name to the correct ``Admin``
instance.
このコントローラーは基本的なCRUDアクションを含んでおり、同一名の
Adminインスタンスにマッピングされることにより、１つのAdminクラスに関連付けられます。

Any or all actions can be overwritten to suit the project's requirements.
いくつか、もしくは全てのアクションはプロジェクトの要求に合わせて上書きすることができます。

The controller uses the ``Admin`` class to construct the different actions.
Inside the controller, the ``Admin`` object is accessible through the
``configuration`` property.
コントローラーは異なるアクションを構成するためにAdminクラスを使います。
コントローラー内部では、Adminオブジェクトはコンフィギュレーションプロパティを通してアクセスすることができます。

Obtaining an ``Admin`` Service
------------------------------

``Admin`` definitions are accessible through the 'sonata.admin.pool' service or
directly from the DIC (dependency injection container). The ``Admin`` definitions are lazy
loaded from the DIC to reduce overhead.
``Admin`` 定義は 'sonata.admin.pool' サービスを通したり、直接DIC（ディペンデンシーインジェクションコンテナ)からアクセスすることができます。
 ``Admin`` 定義はオーバーヘッドを減らすため遅延読み込みされます。

Declaring a new Admin class
---------------------------

Once you have created an admin class, you need to make the framework aware of
it. To do that, you need to add a tag with the name ``sonata.admin`` to the
service. Parameters for that tag are:
Adminクラスを作ったときには、フレームワークにそれを気づかせなければなりません。
すべきことは、サービスのタグのnameに ``sonata.admin`` を加えます。
タグのパラメータは次の通りです。

* ``manager_type``: Label of the document manager to inject;
* ``group``: A label to allow grouping on the dashboard;
* ``label``: Label to use for the name of the entity this manager handles;

* ``manager_type``: 注入するためのドキュメント管理のラベル
* ``group``: ダッシュボード上でのグルーピングを割り当てるためのラベル
* ``label``: このマネージャーががエンティティ名を管理するために使うラベル

Examples:

.. code-block:: xml

    <!-- app/config/config.xml -->
    <service id="sonata.news.admin.post" class="Sonata\NewsBundle\Admin\PostAdmin">

        <tag name="sonata.admin" manager_type="orm" group="sonata_blog" label="post"/>

        <argument />
        <argument>Sonata\NewsBundle\Entity\Post</argument>
        <argument>SonataAdminBundle:CRUD</argument>
</service>

If you want to define your own controller for handling CRUD operations, change the last argument
in the service definition to::

  <argument>SonataNewsBundle:PostAdmin</argument>

Or if you're using a YML configuration file,
もしyam設定ファイルを使っているなら以下の通りです。

.. code-block:: yaml

    services:
       sonata.news.admin.post:
          class: Sonata\NewsBundle\Admin\PostAdmin
          tags:
            - { name: sonata.admin, manager_type: orm, group: sonata_blog, label: post }
          arguments: [null, Sonata\NewsBundle\Entity\Post, SonataNewsBundle:PostAdmin]


You can extend ``Sonata\AdminBundle\Admin\Admin`` class to minimize the amount of
code to write. This base admin class uses the routing services to build routes.
Note that you can use both the Bundle:Controller format or a `service name`_ to
specify what controller to load.
書くべきコードを最小化するために ``Sonata\AdminBundle\Admin\Admin`` クラスを継承することができます。
このベースAdminクラスはルートを作るためルーディングサービスを使用します。Bundle:Controllerフォーマットと、
どのコントローラーをロードすべきか特定する service name の両方を使うことができることを気に留めておいてください。

.. _`Django Project Website`: http://www.djangoproject.com/
.. _`service name`: http://symfony.com/doc/2.0/cookbook/controller/service.html
