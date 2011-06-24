..
    ===========================================
    Paver: Easy Scripting for Software Projects
    ===========================================

===========================================================
Paver: ソフトウェアプロジェクトのための簡易スクリプティング
===========================================================

.. image:: _static/paver_banner.jpg
    :height: 126
    :width: 240

..
    Paver is a Python-based software project scripting tool along the
    lines of Make or Rake. It is not designed to handle the dependency
    tracking requirements of, for example, a C program. It *is* designed
    to help out with all of your other repetitive tasks (run documentation
    generators, moving files around, downloading things), all with the
    convenience of Python's syntax and massive library of code.

Paver は Make や Rake によく似た Python ベースのソフトウェアプロジェクトのスクリプティングツールです。例えば、C 言語のプログラムのように必要な依存関係を追跡して扱うようには設計されていません。Python で書けることと豊富なライブラリの利便性により、雑多な全ての繰り返しタスク (ドキュメント生成、関連ファイルの移動、ファイル等のダウンロード) をこなすように *設計されています* 。

..
    If you're developing applications in Python, you get even more...
    Most public Python projects use distutils or setuptools to create source
    tarballs for distribution. (Private projects can take advantage of
    this, too!) Have you ever wanted to generate the docs before building the
    source distribution? With Paver, you can, trivially. Here's a complete
    pavement.py::

もし Python でアプリケーションを開発しているなら、これから使ってみると良いです。大半の Python の公開プロジェクトは、配布用のソースを tarball で作成するために distutils か setuptools を使います (非公開プロジェクトもこの利点を得られる！) 。これまで配布用のソースを作成する前に、ドキュメントを生成しようとしたことはありますか？Paver を使えば、まさにそういったことができます。pavement.py は次のように記述します。

::

    from paver.easy import *
    from paver.setuputils import setup
    
    setup(
        name="MyCoolProject",
        packages=['mycool'],
        version="1.0",
        url="http://www.blueskyonmars.com/",
        author="Kevin Dangoor",
        author_email="dangoor@gmail.com"
    )
    
    @task
    @needs(['html', "distutils.command.sdist"])
    def sdist():
        """Generate docs and source distribution."""
        pass

..
    With that pavement file, you can just run ``paver sdist``, and your docs
    will be rebuilt automatically before creating the source distribution.
    It's also easy to move the generated docs into some other directory
    (and, of course, you can tell Paver where your docs are stored,
    if they're not in the default location.)

この pavement.py ファイルを利用して、単に ``paver sdist`` を実行すると、配布用のソースを作成する前にドキュメントが自動的に再生成されます。それから、生成したドキュメントを別ディレクトリへ移動するのも簡単です (もちろん、デフォルトの場所以外の場合、Paver へどこに置くかを伝えられます) 。

..
    Features
    --------

機能
----

..
    * Build files are :ref:`just Python <justpython>`
    * :ref:`One file with one syntax <onefile>`, pavement.py, knows how to manage
      your project
    * :ref:`File operations <pathmodule>` are unbelievably easy, thanks to the 
      built-in version of Jason Orendorff's path.py.
    * Need to do something that takes 5 lines of code? 
      :ref:`It'll only take 5 lines of code. <fivelines>`.
    * Completely encompasses :ref:`distutils and setuptools  <setuptools>` so 
      that you can customize behavior as you need to.
    * Wraps :ref:`Sphinx <doctools>` for generating documentation, and adds utilities
      that make it easier to incorporate fully tested sample code.
    * Wraps :ref:`Subversion <svn>` for working with code that is checked out.
    * Wraps :ref:`virtualenv <virtualenv>` to allow you to trivially create a
      bootstrap script that gets a virtual environment up and running. This is
      a great way to install packages into a contained environment.
    * Can use all of these other libraries, but :ref:`requires none of them <nodeps>`
    * Easily transition from setup.py without making your users learn about or
      even install Paver! (See the :ref:`Getting Started Guide <gettingstarted>` 
      for an example).

* ビルドファイルは :ref:`単なる Python <justpython>` プログラムです
* :ref:`1つのファイル内に1つの構文で記述する <onefile>`  pavement.py は、プロジェクト管理の仕組みそのものです
* :ref:`File 操作 <pathmodule>` は、Jason Orendorff が作ってくれた、組み込みの path.py により信じられないぐらい簡単です
* 5行でできることに必要なコードは？ :ref:`やはり5行だけ <fivelines>` です
* :ref:`distutils と setuptools <setuptools>` を内包し、必要に応じてその処理を変更できます
* ドキュメント生成は内部的に :ref:`Sphinx <doctools>` を利用し、テスト済みのサンプルコードを組み込むのが簡単なユーティリティも追加します
* :ref:`Subversion <svn>` を利用して、コードをチェックアウトします
* :ref:`virtualenv <virtualenv>` を利用して、仮想環境の構築、ならびに実行するブートストラップスクリプトの作成します。これは隔離された環境にパッケージをインストールする優れた方法です
* こういった全てのライブラリを利用できますが :ref:`依存関係はありません <nodeps>`
* ユーザが Paver のインストールやその使い方を学ぶことなく、簡単に setup.py から移行します！(サンプルは :ref:`スタートガイド <gettingstarted>` を参照)

..
    See how it works! Check out the :ref:`Getting Started Guide <gettingstarted>`.

Paver がどう動くのかを見てください！ :ref:`スタートガイド <gettingstarted>` をチェックアウトしよう。

..
    Paver was created by `Kevin Dangoor <http://blueskyonmars.com>`_ of `SitePen <http://sitepen.com>`_.

Paver は `SitePen <http://sitepen.com>`_ に所属する `Kevin Dangoor <http://blueskyonmars.com>`_ が作成しました。

..
    Status
    ------

ステータス
----------

..
    Paver has been in use in production settings since mid-2008, and significant 
    attention is paid to backwards compatibility since the release of 1.0.

Paver は2008年中頃から本番環境で使われています。そして 1.0 リリースから後方互換性にかなり注意を払っています。

..
    See the :ref:`changelog <changelog>` for more information about recent improvements.
 
最新の改善内容は :ref:`changelog <changelog>` を参照してください。 

..
    Installation
    ------------

インストール
------------

..
    The easiest way to get Paver is if you have setuptools_ installed.

Paver をインストールする最も簡単な方法は、 setuptools_ をインストールしておくことです。

``easy_install Paver``

..
    Without setuptools, it's still pretty easy. Download the Paver .tgz file from 
    `Paver's Cheeseshop page`_, untar it and run:

とはいえ、setuptools がなくても本当に簡単です。 `Paver の Cheeseshop ページ`_ から Paver の .tgz ファイルをダウンロード、解凍して次のように実行してください。

``python setup.py install``

.. _Paver の Cheeseshop ページ: http://pypi.python.org/pypi/Paver/
.. _setuptools: http://peak.telecommunity.com/DevCenter/EasyInstall

..
    Help and Development
    --------------------

ヘルプと開発
------------

..
    You can get help from the `mailing list`_.

困ったときは `メーリングリスト`_ で質問してください。

..
    If you'd like to help out with Paver, you can check the code out from github:

Paver を支援しようと考えているなら github からそのコードをチェックアウトできます。

``git clone https://github.com/paver/paver.git``

..
    Ideally, create a fork, fix an issue from `Paver's list of issues`_ (or create an issue
    Yourself) and send a pull request.

理想としては、リポジトリをフォークして `Paver の課題リスト`_ にある課題 (または自分で作成した課題) を修正して、pull リクエストを送ってください。

.. _メーリングリスト: http://groups.google.com/group/paver
.. _Paver の課題リスト: https://github.com/paver/paver/issues

..
    License
    -------

ライセンス
----------

..
    Paver is licensed under a BSD license. See the LICENSE.txt file in the 
    distribution.

Paver は BSD ライセンスです。再配布に関しては LICENSE.txt を参照してください。

..
    Contents
    --------

コンテンツ
----------

.. toctree::
   :maxdepth: 2
   
   foreword
   features
   getting_started
   pavement
   paverstdlib
   cmdline
   tips
   articles
   changelog
   credits

..
    Indices and tables
    ------------------

インデックスとテーブル
----------------------

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

