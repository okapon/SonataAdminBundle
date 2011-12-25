Advance
=======

Service Configuration
---------------------

When you create a new Admin service you can configure its dependencies, by default services who are injected are:
新しいAdminサービスを作るとき、その依存性注入を設定することができます。デフォルトで注入されているのは以下の通りです。

========================      =============================================
Dependencies                  Service Id
========================      =============================================
model_manager                 sonata.admin.manager.%manager-type%
form_contractor               sonata.admin.builder.%manager-type%_form
show_builder                  sonata.admin.builder.%manager-type%_show
list_builder                  sonata.admin.builder.%manager-type%_list
datagrid_builder              sonata.admin.builder.%manager-type%_datagrid
translator                    translator
configuration_pool            sonata.admin.pool
router                        router
validator                     validator
security_handler              sonata.admin.security.handler
menu_factory                  knp_menu.factory
router_builder                sonata.admin.route.path_info
label_translator_strategy     sonata.admin.label.strategy.form_component
=========================     =============================================

Note: %manager-type% is replace by the manager type (orm, odm...)
Note: %manager-type% は(orm, odm...)といった管理タイプに置き換えられます。

You have 2 ways of defining the dependencies inside a ``services.xml``.
``services.xml`` に依存性を定義する方法は２つあります。

* With a tag attribute, less verbose::
* あまり詳細でないタグ属性を用いる

.. code-block:: xml

        <service id="acme.project.admin.security_feed" class="AcmeBundle\ProjectBundle\Admin\ProjectAdmin">
            <tag
                name="sonata.admin"
                manager_type="orm"
                group="Project"
                label="Project"
                label_translator_strategy="sonata.admin.label.strategy.native"
                router_builder="sonata.admin.route.path_info"
                />
            <argument />
            <argument>AcmeBundle\ProjectBundle\Entity\Project</argument>
            <argument />
        </service>

* With a method call, more verbose
* より詳細なメソッドの呼び出しを用いる

.. code-block:: xml

        <service id="acme.project.admin.project" class="AcmeBundle\ProjectBundle\Admin\ProjectAdmin">
            <tag
                name="sonata.admin"
                manager_type="orm"
                group="Project"
                label="Project"
                />
            <argument />
            <argument>AcmeBundle\ProjectBundle\Entity\Project</argument>
            <argument />

            <call method="setLabelTranslatorStrategy">
                <argument type="service" id="sonata.admin.label.strategy.native" />
            </call>

            <call method="setRouterBuilder">
                <argument type="service" id="sonata.admin.route.path_info" />
            </call>
        </service>


If you want to modify the service who are going to be injected, add the following code to your
application's config file:
もしサービスを注入するように変更したければ、以下のコードをあなたのアプリケーション設定ファイルに追加してください。

.. code-block:: yaml

    # app/config/config.yml
    admins:
        sonata_admin:                                           #method name, you can find the list in the table above
            sonata.order.admin.order:                           #id of the admin service's
                model_manager: sonata.order.admin.order.manager #id of the your service


Admin Extension
---------------

S