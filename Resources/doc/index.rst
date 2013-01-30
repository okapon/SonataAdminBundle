Admin Bundle
============

**SonataAdminBundle is split into 4 bundles (SonataAdminBundle は 4 つのバンドルにわかれています):**

* SonataAdminBundle: contains core libraries and services (コアライブラリとサービスを含んでいます)
* `SonataDoctrineORMAdminBundle <https://github.com/sonata-project/SonataDoctrineORMAdminBundle>`_: integrates Doctrine ORM project with the core admin bundle (Doctrine ORM プロジェクトをコアAdminバンドルに統合します)
* `SonataDoctrineMongoDBAdminBundle <https://github.com/sonata-project/SonataDoctrineMongoDBAdminBundle>`_: integrates MongoDB with the core admin bundle (early stage) (MongoDB をコアAdminバンドルに統合します 【初期段階】)
* `SonataDoctrinePhpcrAdminBundle <https://github.com/sonata-project/SonataDoctrinePhpcrAdminBundle>`_: integrates PHPCR with the core admin bundle (early stage) （PHPCR をコアAdminバンドルに統合します 【初期段階】)

The demo website can be found in  http://demo.sonata-project.org/admin/dashboard (admin as user and password). (こちらでデモサイトが見れます。ユーザーとパスワードはadminです)

Reference Guide
---------------

.. toctree::
   :maxdepth: 1
   :numbered:

   reference/installation
   reference/getting_started
   reference/configuration
   reference/architecture
   reference/dashboard
   reference/routing
   reference/saving_hooks
   reference/form_types
   reference/form_help_message
   reference/field_types
   reference/conditional_validation
   reference/templates
   reference/batch_actions
   reference/translation
   reference/security
   reference/advance
   reference/console
   reference/preview_mode
