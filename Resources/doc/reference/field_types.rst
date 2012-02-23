Field Types
===========

List and Show Actions
---------------------

There are many field types that can be used in the list action or show action :
listアクションとshowアクションで使うことのできるフィールドタイプはたくさんあります。

* array: display value from an array
* boolean: display a green or red picture dependant on the boolean value, this type accepts an ``editable``
  parameter to edit the value from within the list or the show actions
* date: display a formatted date
* datetime: display a formatted date and time
* text: display a text
* trans: translate the value with a provided ``catalogue`` option
* string: display a text
* decimal: display a number
* currency: display a number with a provided ``currency`` option
* percent: display a percentage

* array: 配列から値を表示する
* boolean: boolean値に依存している緑または赤の画像を表示する。 this type accepts an ``editable``
  parameter to edit the value from within the list or the show actions
* date: フォーマットされた日付を表示する
* datetime: フォーマットされた日付を表示する
* text: テキストを表示
* trans:  ``catalogue`` オプションで提供された値に翻訳する
* string: テキストを表示
* decimal: 数字を表示
* currency:  ``currency`` オプションで提供された数字にを表示する
* percent: パーセンテージを表示する

.. note::

    If the ``SonataIntlBundle`` is installed in the project some template types
    will be changed to use localized information.
    もし ``SonataIntlBundle`` がプロジェクト内のいくつかのテンプレートタイプでインストールされて
    いたらローカライズされた情報を使うため変更される


More types might be provided based on the persistency layer defined. Please refer to their
related documentations.
永続化層定義に基づく（うまい訳がわからない）、より多くのタイプが提供されるかもしれない。
（そのときは）関連したドキュメントを参照してください。