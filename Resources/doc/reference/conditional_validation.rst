Inline Validation
=================

The inline validation is about delegating model validation to a dedicated service.
The current validation implementation built in the Symfony2 framework is very powerful
as it allows to declare validation on : class, field and getter. However these declarations
can take a while to code for complex rules. Rules must be a set of a ``Constraint``
and ``Validator`` instances.
インラインバリデーションはモデルの検証を委譲する専門のサービスです。symfony2フレームワークに
組み込まれている現在のバリデーション実装はとてもパワフルで、クラス、フィールド、ゲッターに
バリデーションを宣言することができます。しかしながら、これらの宣言は複雑なルールのために
コーディングを複雑にしてしまうことがあります。ルールには ``制約`` が課されるべきであり、
 ``バリデーター`` インスタンスであるべきでです。

The inline validation tries to provide a nice solution by introducting an ``ErrorElement``
object. The object can be used to check assertion against the model :
インラインバリデーションは ``ErrorElement`` オブジェクトを導入することにより素晴らしい解決法を
提供することを試んでます。
モデルに対してアサーションがあるかチェックすることで、このオブジェクトは使われます。

.. code-block:: php

    <?php
    $errorElement
        ->with('settings.url')
            ->assertNotNull(array())
            ->assertNotBlank()
        ->end()
        ->with('settings.title')
            ->assertNotNull(array())
            ->assertNotBlank()
            ->assertMinLength(array('limit' => 50))
            ->addViolation('ho yeah!')
        ->end();

    if (/* complex rules */) {
        $errorElement->with('value')->addViolation('Fail to check the complex rules')->end()
    }

    /* conditional validation */
    if ($this->getSubject()->getState() == Post::STATUS_ONLINE) {
        $errorElement
            ->with('enabled')
                ->assertNotNull()
                ->assertTrue()
            ->end();
    }

.. note::

    This solution relies on the validator component so validation defined through
    the validator component will be used.
    この解決方法はバリデーターコンポーネントに依存しているので、バリデーターコンポーネントを
    使うことによりバリデーションは定義されます。

Using this validator
--------------------

Just add the ``InlineConstraint`` class constraint, like this:
以下のように、 ``InlineConstraint`` 制約クラスを追加するだけです。

.. code-block:: xml

    <class name="Application\Sonata\PageBundle\Entity\Block">
        <constraint name="Sonata\AdminBundle\Validator\Constraints\InlineConstraint">
            <option name="service">sonata.page.cms.page</option>
            <option name="method">validateBlock</option>
        </constraint>
    </class>

There are two important options:
２つの重要なオプションがあります。

  - ``service``: the service where the validation method is defined
  - ``method``: the service's method to call

  - ``service``: バリデーションメソッドが定義されているサービス
  - ``method``: 呼ばれるためのサービスメソッド

The method must accept two arguments:
メソッドは２つの引数を受け取らねばなりません。

 - ``ErrorElement``: the instance where assertion can be checked
 - ``value``: the object instance

 - ``ErrorElement``: アサーションを確認するインスタンス
 - ``value``: オブジェクトインスタンス

Example from the ``SonataPageBundle``
-------------------------------------

.. code-block:: php

    <?php
    namespace Sonata\PageBundle\Block;

    use Sonata\PageBundle\Model\PageInterface;
    use Sonata\AdminBundle\Validator\ErrorElement;

    class RssBlockService extends BaseBlockService
    {
        // ... code removed for simplification

        public function validateBlock(ErrorElement $errorElement, BlockInterface $block)
        {
            $errorElement
                ->with('settings.url')
                    ->assertNotNull(array())
                    ->assertNotBlank()
                ->end()
                ->with('settings.title')
                    ->assertNotNull(array())
                    ->assertNotBlank()
                    ->assertMinLength(array('limit' => 50))
                    ->addViolation('ho yeah!')
                ->end();
        }
    }
