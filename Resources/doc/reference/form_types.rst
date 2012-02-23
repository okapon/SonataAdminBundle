Form Types
==========

Admin related form types
------------------------

// todo


Other form types
----------------

The bundle comes with some handy form types which are available from outside the scope of the ``SonataAdminBundle``::
そのバンドルは ``SonataAdminBundle`` のスコープの外から利用できるいくつかの便利なフォームタイプを備えています。

sonata_type_immutable_array
^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``Immutable Array`` allows you to edit an array property by defining a type per key.
 ``Immutable Array`` はキーごとに決められたタイプだけ配列のプロパティを編集することを許します。

The type has a ``keys`` parameter which contains the definition for each key. A definition is an array with 3 options :
そのタイプはそれぞれのキーに対する定義を含んだキーパラメーターを持っています。（訳が意味不明ですね。。）
* key name
* type : a type name or a ``FormType`` instance
* related type parameters : please refer to the related form documentation.

* the key name
* the type : タイプ名 もしくは ``FormType`` のインスタンス
* the related type parameters : please refer to the related form documentation.

Let's say a ``Page`` have options property with some fixed key-pair values, each value has a different type : integer,
url, or string for instance.
いくつかの固定化されたキーペア値のオプションプロパティを持った ``Page`` を言って（書いて？）みよう。
例えばintegerやurlもしくはｓｔｒｉｎｇインスタンスといった、それぞれの値は異なったタイプを持っています。

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
今、そのプロパティはそれぞれのタイプに応じてタイプが設定され編集されます。

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

The ``boolean`` type is a specialized ``ChoiceType`` where the choices list is locked to 'yes' and 'no'.
 ``boolean`` タイプは選択リストがyesとnoに固定された ``ChoiceType`` に特化しています。

sonata_type_translatable_choice
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The translatable type is a specialized ``ChoiceType`` where the choices values are translated with the Symfony
Translator component.
``translatable`` type は選択値がSymfony Translator component によって翻訳された
 ``ChoiceType`` に特化しています。
(翻訳というより、値を定義して選択リストに挿し込むという意味合いか？下のコード参照)

The type has one extra parameter :
 * ``catalogue`` : the catalogue name to translate the value

このタイプは１つの追加パラメーターを持っています。
 * ``catalogue`` : 値を翻訳するためのカタログ名


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
