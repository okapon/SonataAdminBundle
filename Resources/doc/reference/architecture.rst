Architecture (アーキテクチャ)
=============================

The architecture of the SonataAdminBundle is primarily inspired by the Django Admin
Project, which is truly a great project. More information can be found at the
`Django Project Website`_.

このバンドルのアーキテクチャーは、主にDjango Admin Project にインスパイアされており、
それはとても大きなプロジェクトです。より多くの情報はDjango ProjectのWebサイトから手に入ります。

The Admin Class (Adminクラス)
-----------------------------

The ``Admin`` class represents the CRUD definition for a specific model. It
contains all the configuration necessary to display a rich CRUD interface for
the entity.

Adminクラスは個々のモデルに対してCRUD定義を表します。それ(Adminクラス)はEntityに対して
リッチなCRUDインターフェースを表示するために必要な設定を全て含んでいます。

Within the admin class, the following information can be defined:

Adminクラス内部で、以下の情報を定義することができます。

* ``list``: The fields displayed in the list table (リストテーブル内で表示されるフィールド);
* ``filter``: The fields available for filtering the list (リストをフィルタリングできるフィールド);
* ``form``: The fields used to edit the entity (Entityを編集するためのフィールド);
* ``show``: The fields used to show the entity (Entityを表示するためのフィールド);
* Batch actions: Actions that can be performed on a group of entitie (エンティティグループに対して機能するアクション)
  (e.g. bulk delete ・・例えば多量のdelete)

If a field is associated with another entity (and that entity also has an
``Admin`` class), then the related ``Admin`` class will be accessible from
within the first class.

もしフィールドが他のエンティティと関連性がある場合、（そしてそのエンティティもAdminクラスを
もっている場合）関連したAdminクラスは一つ目のクラス内で利用しやすいようになります。

The admin class is a service implementing the ``AdminInterface`` interface,
meaning that the following required dependencies are automatically injected:

adminクラスは AdminInterface インターフェースを実装しているサービスであり、
以下の必須依存関係が自動的に注入されることを意味しています。

* ``ListBuilder``: builds the list fields (リストフィールをを組み立てる)
* ``FormContractor``: builds the form using the Symfony ``FormBuilder`` (Symfonyの``FormBuilder``を使ってフォームを組み立てる)
* ``DatagridBuilder``: builds the filter fields (フィルターフィールドを組み立てる)
* ``Router``: generates the different urls (異なったURLを生成する)
* ``Request``
* ``ModelManager``: Service which handles specific ORM code (個別のORMコードを取り扱うサービス)
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

ここでは、FOS' User Manager はPostサービスに注入されています。

Field Definition (フィールド定義)
---------------------------------

A field definition is a ``FieldDescription`` object. There is one definition per list
field.

フィールド定義は ``FieldDescription`` オブジェクト。リストフィールドごとに一つの定義があります。

The definition contains:

この定義は以下を含んでいます

* ``name``: The name of the field definition; (フィールド定義の名前)
* ``type``: The field type; (フィールドのタイプ)
* ``template``: The template used for displaying the field (フィールドを表示する為に使うテンプレート);
* ``targetEntity``: The class name of the target entity for relations (リレーションのためのターゲットエンティティのクラス名);
* ``options``: Certain field types have additional options (あるフィールドタイプは追加のオプションを持っている);

Template Configuration (テンプレート設定)
-----------------------------------------

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

* ``filter_text.twig``: template used in the filter box (フィルターボックス内で使われるテンプレート)
* ``list_text.twig``: template used in the list table (リストテーブル内で使われるテンプレート)

CRUDController
--------------

The controller contains the basic CRUD actions. It is related to one
``Admin`` class by mapping the controller name to the correct ``Admin``
instance.
このコントローラーは基本的なCRUDアクションを含んでおり、コントローラーは
同一名のAdminインスタンスにマッピングされることにより、１つのAdminクラスに関連付けられます。

Any or all actions can be overwritten to suit the project's requirements.
いくつか、もしくは全てのアクションはプロジェクトの要求に合わせて上書きすることができます。

The controller uses the ``Admin`` class to construct the different actions.
Inside the controller, the ``Admin`` object is accessible through the
``configuration`` property.
コントローラーは異なるアクションを構成するためにAdminクラスを使います。
コントローラー内部では、Adminオブジェクトはコンフィギュレーションプロパティを通してアクセスすることができます。

Obtaining an ``Admin`` Service ( ``Admin`` サービスを取得)
----------------------------------------------------------

``Admin`` definitions are accessible through the ``sonata.admin.pool`` service or
directly from the DIC (dependency injection container). The ``Admin`` definitions 
are lazy-loaded from the DIC to reduce overhead.

``Admin`` 定義は 'sonata.admin.pool' サービスを通したり、直接DIコンテナからアクセスすることができます。
 ``Admin`` 定義はオーバーヘッドを減らすため遅延読み込みされます。

Declaring a new Admin class (新しいAdminクラスを宣言する)
---------------------------------------------------------

Once you have created an admin class, you need to make the framework aware of
it. To do that, you need to add a tag with the name ``sonata.admin`` to the
service. Parameters for that tag are:

Adminクラスを作ったときには、フレームワークにそれを気づかせなければなりません。
すべきことは、サービスのタグのnameに ``sonata.admin`` を加えます。
タグのパラメータは次の通りです。

* ``manager_type``: Label of the database manager to use (注入するためのドキュメント管理のラベル);
* ``group``: A label to allow grouping on the dashboard (ダッシュボード上でのグルーピングを割り当てるためのラベル);
* ``label``: Label to use for the name of the entity this manager handles (このマネージャーががエンティティ名を管理するために使うラベル);

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

もし自身のコントローラーでCRUD操作をハンドリングしたければ、サービス定義の最後の引数を変更してください。

  <argument>SonataNewsBundle:PostAdmin</argument>

Or if you're using a YML configuration file,

もしYML設定ファイルを使っているなら以下

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

書かなければならないコードを最小化するために ``Sonata\AdminBundle\Admin\Admin`` を継承することができます。
このベースAdminはルートを作るためルーディングサービスを使用します。
どのコントローラーをロードすべきか特定する為に Bundle:Controllerフォーマットと、 `service name`_ の両方を使うことができることに注意しましょう。

.. _`Django Project Website`: http://www.djangoproject.com/
.. _`service name`: http://symfony.com/doc/2.0/cookbook/controller/service.html
