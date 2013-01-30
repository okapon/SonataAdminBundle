Form Types (フォームタイプ)
===========================

Admin related form types (Adminはフォームタイプに関連しています)
----------------------------------------------------------------

The bundle come with different form types to handle model values:

バンドルはモデルの値を処理するために異なるフォームタイプから構成される。

sonata_type_model
^^^^^^^^^^^^^^^^^

The ``Model Type`` allows you to choose an existing model or create new ones. 
This type doesn't allow to directly edit the selected model.

 ``Model Type`` は存在しているモデルを選択したり、新しいモデルを作成することを許します。
このタイプは選択されたモデルを直接編集することを許可しません。

sonata_type_admin
^^^^^^^^^^^^^^^^^

The ``Admin Type`` will delegate the form construction for this model to its 
related admin class. This type is useful to cascade edition or creation of 
linked models.

 ``Admin Type`` はモデルに関連したadminクラスにformの構築を委譲します。
このタイプはカスケード表示による編集やリンクされたモデルの作成に役立ちます。

sonata_type_collection
^^^^^^^^^^^^^^^^^^^^^^

The ``Collection Type`` is meant to handle creation and edition of model 
collections. Rows can be added and deleted, and your model abstraction layer may
allow you to edit fields inline.


 ``Collection Type`` はコレクションモデルの作成と編集を扱うために重要です。
行を追加したり削除したり、モデルの抽象化層はインラインでフィールドを編集することができるでしょう。

**TIP**: A jQuery event is fired after a row has been added(*sonata-collection-item-added*) or deleted(*sonata-collection-item-deleted*). You can bind to them to trigger some custom javascript imported into your templates(eg: add a calendar widget to a just added date field)

Type configuration
^^^^^^^^^^^^^^^^^^

todo


Field configuration
^^^^^^^^^^^^^^^^^^^

- ``admin_code``: Force for any field involving a model the admin class used to 
    handle it (useful for inline editing with ``sonata_type_admin``). The 
    expected value here is the admin service name, not the class name. If not 
    defined, the default admin class for the model type will be used (even if 
    you didn't define any admin for the model type).

Other specific field configuration options are detailed in the related 
abstraction layer documentation.

Other form types
----------------

The bundle comes with some handy form types which are available from outside the
scope of the ``SonataAdminBundle``:

バンドルは ``SonataAdminBundle`` のスコープの外から利用できるいくつかの便利なフォームタイプを備えています。

sonata_type_immutable_array
^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``Immutable Array`` allows you to edit an array property by defining a type 
per key.

 ``Immutable Array`` はキーごとにタイプを定義することにより配列プロパティを編集することができます。

The type has a ``keys`` parameter which contains the definition for each key. 
A definition is an array with 3 options :

このタイプ(sonata_type_immutable_array)はそれぞれのキーごとに定義を含んだキーパラメーターを持っています。（訳が意味不明ですね。。） 
定義は３つのオプションを持った配列です

* key name
* type : a type name or a ``FormType`` instance (タイプ名 もしくは ``FormType`` のインスタンス)
* related type parameters : please refer to the related form documentation. (関係したform documentationを参照して下さい)

Let's say a ``Page`` have options property with some fixed key-pair values, each
value has a different type : integer, url, or string for instance.

例えば、固定化されたいくつかのkey-pair 値のオプションプロパティを持った ``Page`` があるとして、
それぞれの値は異なるタイプを持っています。具体的には、数字やurlもしくは文字列といったものがあるでしょう。
（翻訳がちょっと怪しいです）

.. code-block:: php

    <?php
    class Page
    {
        protected $options = array(
            'ttl'       => 1,
            'redirect'  => ''
        );

        public function setOptions(array $options)
        {
            $this->options = $options;
        }

        public function getOptions()
        {
            return $this->options;
        }
    }

Now, the property can be edited by setting a type for each type

これから、そのプロパティはそれぞれのタイプに応じてタイプが設定され編集されます。

.. code-block:: php

        <?php
        $form->add('options', 'sonata_type_immutable_array', array(
            'keys' => array(
                array('ttl',        'text', array('required' => false)),
                array('redirect',   'url',  array('required' => true)),
            )
        ));


sonata_type_boolean
^^^^^^^^^^^^^^^^^^^

The ``boolean`` type is a specialized ``ChoiceType`` where the choices list is 
locked to 'yes' and 'no'.

 ``boolean`` タイプは選択リストがyesかnoに固定された ``ChoiceType`` に特化しています。

sonata_type_translatable_choice
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The translatable type is a specialized ``ChoiceType`` where the choices values 
are translated with the Symfony Translator component.

translatable type は選択肢の値がSymfony Translator component で翻訳することに
特化した ``ChoiceType`` です。

The type has one extra parameter (このタイプは１つの追加パラメーターを持っています):

 * ``catalogue`` : the catalogue name to translate the value (値を翻訳するためのカタログの名前)

.. code-block:: php

    <?php

    // The delivery list
    class Delivery
    {
        public static function getStatusList()
        {
            return array(
                self::STATUS_OPEN      => 'status_open',
                self::STATUS_PENDING   => 'status_pending',
                self::STATUS_VALIDATED => 'status_validated',
                self::STATUS_CANCELLED => 'status_cancelled',
                self::STATUS_ERROR     => 'status_error',
                self::STATUS_STOPPED   => 'status_stopped',
            );
        }
    }

    // form usage
    $form->add('deliveryStatus', 'sonata_type_translatable_choice', array(
        'choices' => Delivery::getStatusList(),
        'catalogue' => 'SonataOrderBundle'
    ))
