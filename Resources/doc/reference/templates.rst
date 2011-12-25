Templates
=========

By default, an Admin class used a set of templates, it is possible to tweak the default values by editing the configuration
デフォルトでは、Adminクラスはテンプレートセットを使っており、設定を編集することでデフォルト値を変更することができます。

.. code-block:: yaml

    sonata_admin:
        templates:
            # default global templates
            layout:  SonataAdminBundle::standard_layout.html.twig
            ajax:    SonataAdminBundle::ajax_layout.html.twig

            # default value if done set, actions templates, should extends a global templates
            list:    SonataAdminBundle:CRUD:list.html.twig
            show:    SonataAdminBundle:CRUD:show.html.twig
            edit:    SonataAdminBundle:CRUD:edit.html.twig
            history:  SonataAdminBundle:CRUD:history.html.twig


Usage of each template :
それぞれのテンプレートの用途

* layout : based layout used by the dashboard and an admin class
* ajax : default layout used when an ajax request is performed
* list : the template to use for the list action
* show : the template to use for the show action
* edit : the template to use for the edit and create action
* history : the template to use for the history / audit action

* layout : ダッシュボードとAdminクラスで使われる基本レイアウト
* ajax : ajaxリクエストが行われた時に使われるデフォルトレイアウト
* list : listアクションで使われるテンプレート
* show : showアクションで使われるテンプレート
* edit : editアクション、createアクションでアクションで使われるテンプレート
* history : historyアクションと、auditアクションで使われるテンプレート

The default values will be set only if the ``Admin::setTemplates`` is not called by the Container.
 ``Admin::setTemplates`` がコンテナで呼ばれないときだけ、デフォルト値がセットされます。

