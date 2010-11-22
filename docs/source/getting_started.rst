.. _gettingstarted:

..
    ==========================
    Getting Started with Paver
    ==========================

==============
スタートガイド
==============

..
    Often, the easiest way to get going with a new tool is to see an example
    in action, so that's how we'll get started with Paver. In the Paver
    distribution, there are samples under docs/samples. The Getting
    Started samples are in the "started" directory under there.

新たなツールに慣れる最も簡単な方法は実際に実行できるサンプルを見てみることです。つまり、どのように Paver を使い始めれば良いかということになります。Paver ディストリビューション内の docs/sample にサンプルがあります。そして、その "started" ディレクトリにスタートガイドのサンプルがあります。

..
    The Old Way
    ===========

旧い方法
========

..
    Our first sample is called "The Old Way" (and it's in the 
    docs/samples/started/oldway directory). It's a fairly typical project
    with one Python package and some docs, and we want to be able to
    distribute it.

最初のサンプルは "旧い方法" と呼びます(docs/samples/started/oldway ディレクトリにあります)。それは配布できるようにしたい Python パッケージとドキュメントの適正且つ典型的なプロジェクトです。

..
    Python's distutils_ makes it easy indeed to create a distributable
    package. We create a setup.py file that looks like this::

Python の distutils_ を使用すると配布パッケージをとても簡単に作成できます。次のような setup.py ファイルを作成します。

::

  #<== include('started/oldway/setup.py')==>
  from distutils.core import setup

  setup(
      name="TheOldWay",
      packages=['oldway'],
      version="1.0",
      url="http://www.blueskyonmars.com/",
      author="Kevin Dangoor",
      author_email="dangoor@gmail.com"
  )
  #<==end==>

..
    With that simple setup script, you can run::

簡単な setup スクリプトで実行します。

::

  python setup.py sdist

..
    to build a source distribution::

ソースディストリビューションをビルドするには、

::

  # <== 
  # sh('cd docs/samples/started/oldway; python setup.py sdist',
  #    insert_output=False)
  # sh('ls -l docs/samples/started/oldway/dist')
  # ==>
  celkem 4
  -rw-r--r-- 1 lukas.linhart lukas.linhart 634 22. lis 15.55 TheOldWay-1.0.tar.gz
  # <==end==>

..
    Then your users can run the familiar::

それからユーザは見慣れたコマンドを実行します。

::

  python setup.py install

..
    to install the package, or use setuptools' even easier::

パッケージをインストールするために setuptools を使用するとさらに簡単です。

::

  easy_install "TheOldWay"

..
    for packages that are up on the Python Package Index.

パッケージは Python パッケージインデックスにあります。

.. _distutils: http://docs.python.org/dist/dist.html

..
    The Old Way's Docs
    ------------------

旧い方法のドキュメント
----------------------

..
    The Old Way project is at least a bit modern in that it uses Sphinx_
    for documentation. When you use sphinx-quickstart to get going with your
    docs, Sphinx will give you a Makefile that you can run to generate
    your HTML docs. So, generating the HTML docs is easy::

旧い方法プロジェクトはドキュメントに Sphinx_ を使用しているので少なくともちょっとはモダンです。sphinx-quickstart を使用してドキュメントを作成し始めるとき Sphinx は HTML ドキュメントを生成する Makefile を設けるでしょう。そのため、HTML ドキュメントを簡単に生成できます。

::

  make html

..
    Except, in this project (as in Paver itself), we want to include the
    HTML files in a docs directory in the package for presenting help to
    the users. We end up creating a shell script to do this::

但し、(Paver そのもののように)このプロジェクトでは、ユーザへヘルプを定期ょ湯するためにパッケージの docs ディレクトリに HTML ファイルを追加したいです。最終的に次のようなシェルスクリプトを作ります。

::

  # <== include("started/oldway/builddocs.sh")==>
  cd docs
  make html
  cd ..
  rm -rf oldway/docs
  mv docs/_build/html oldway/docs
  # <==end==>

..
    Of course, creating a script like this means that we have to actually
    remember to run it. We could change this script to "buildsdist.sh"
    and add a ``python setup.py sdist`` to the end of the file. But,
    wouldn't it be nicer if we could just use ``python setup.py sdist``
    directly?

もちろん、このようなスクリプトを作成することは、実際にそのスクリプトの実行を覚えておく必要があることを意味します。このスクリプトを "buildsdist.sh" に変更して、ファイルの最後に ``python setup.py sdist`` を追加することはできます。しかし、直接 ``python setup.py sdist`` を実行できればもっと良い気がしますよね？

..
    You can `create new distutils commands`_, but do you really want to
    drop stuff like that in the distutils/command package in your
    Python library directory? And how would you call the sdist command
    anyway? setuptools_ helps, but it still requires setting up a module
    and entry point for this collection of commands.

`新たな distutils コマンドを作成する`_ こともできますが、Python ライブラリディレクトリの distutils/command パッケージにそんな機能を本当に追加したいでしょうか？そして、いずれにしても sdist コマンドをどうやって呼び出すのでしょうか？ setuptools_ は助けてくれますが、それでもこのコマンドのコレクションのためにエントリポイントとモジュールのセットアップが必要になります。

.. _create new distutils commands: http://docs.python.org/dist/node84.html
.. _新たな distutils コマンドを作成する: http://www.python.jp/doc/release/dist/api-reference.html
.. _setuptools: http://peak.telecommunity.com/DevCenter/setuptools
.. _Sphinx: http://sphinx.pocoo.org

..
    Work with me here
    -----------------

私のところでは動作する
----------------------

..
    I just `know` there are some people reading this and thinking
    "man, what a contrived example!". Building, packaging, distributing
    and deploying of projects is quite custom for every project.
    Part of the point of Paver is to make it easy to handle whatever
    weird requirements arise in your project. This example may seem
    contrived, but it should give you an idea of how easy Paver
    makes it to get your tasks done.

これを読んで "うわっ、なんて不自然なサンプルだ！" と考える人がいることも `分かります` 。プロジェクトのビルド、パッケージング、配布、デプロイは全てのプロジェクトでかなりカスタマイズされます。Paver の重要なことの1つはプロジェクトでどんな奇妙な要求が起こっても簡単に扱えるようにすることです。このサンプルは不自然に見えるかもしれませんが、どうやって Paver がそういったタスクを簡単に扱うかについてのアイディアを説明します。

..
    The New Way
    ===========

新しい方法
==========

..
    Let's bring in Paver now to clean up our scripting a bit. Converting
    a project to use Paver is really, really simple. Recall the setup
    function from our Old Way setup.py::

これまでのセクションのスクリプティングを今すぐクリーンアップして Paver を少し取り入れてみましょう。Paver を使用するために既存のプロジェクトから移行することは、本当に、本当に簡単です。旧い方法の setup.py から setup() 関数を呼び出してください。

::

  # <== include("started/oldway/setup.py", "setup")==>
  setup(
      name="TheOldWay",
      packages=['oldway'],
      version="1.0",
      url="http://www.blueskyonmars.com/",
      author="Kevin Dangoor",
      author_email="dangoor@gmail.com"
  )
  # <==end==>

..
    Getting Started with Paver
    --------------------------

Paver を使い始める
------------------

..
    setup.py is a standard Python script. It's just called setup.py
    as a convention. Paver works a bit more like Make or Rake.
    To use Paver, you run ``paver <taskname>`` and the paver
    command will look for a pavement.py file in the current directory.
    pavement.py is a standard Python module. A typical pavement will 
    import from paver.easy to get a bunch of convenience functions
    and objects and then import other modules that include useful
    tasks::

setup.py は普通の Python スクリプトです。それは慣例として setup.py と呼ばれます。Paver は Make や Rake のように動作します。Paver を使用するために ``paver <taskname>`` を実行します。そして paver コマンドはカレントディレクトリの pavement.py ファイルを探します。pavement.py は普通の Python モジュールです。典型的な pavement.py は paver.easy から便利な関数、オブジェクトのセットをインポートします。それから役に立つタスクを含むその他のモジュールをインポートします。

::

    # <== include('started/newway/pavement.py', 'imports')==>
    from paver.easy import *
    import paver.doctools
    from paver.setuputils import setup
    # <==end==>

..
    Converting from setup.py to pavement.py is easy. Paver provides
    a special ``options`` object that holds all of your build options.
    ``options`` is just a dictionary that allows attribute-style
    access and has some special searching abilities. The options
    for distutils operations are stored in a ``setup`` section of the
    options. And, as a convenience, Paver provides a setup function
    that sets the values in that options section (and goes a step
    further, by making all of the distutils/setuptools commands 
    available as Paver tasks). Here's what the conversion looks like::

setup.py から pavement.py に変換することは簡単です。Paver は全てのビルドオプションを含む特殊な ``options`` オブジェクトを提供します。 ``options`` は属性スタイルのアクセスを許可して特別な検索機能を持つ辞書です。distutils の操作オプションはそのオプションの ``setup`` セクションに格納されます。そして、慣例として Paver はその options セクションに値をセットする setup 関数を提供します(さらに一歩踏み込むと、全ての distutils/setuptools コマンドを作ることで Paver のタスクとして利用できます)。ここにその移行がどのように行うかを紹介します。

::

  # <== include('started/newway/pavement.py', 'setup')==>
  setup(
      name="TheNewWay",
      packages=['newway'],
      version="1.0",
      url="http://www.blueskyonmars.com/",
      author="Kevin Dangoor",
      author_email="dangoor@gmail.com"
  )
  # <==end==>

..
    Paver is compatible with distutils
    ----------------------------------

Paver は distutils 互換
-----------------------

..
    Choosing to use Paver does not mean giving up on distutils or
    setuptools. Paver lets you continue to use distutils and setuptools
    commands. When you import a module that has Paver tasks in it,
    those tasks automatically become available for running. If you
    want access to distutils and setuptools commands as well, you can either
    use the ``paver.setuputils.setup`` function as described
    above, or call ``paver.setuputils.install_distutils_tasks()``.

Paver を選択するということは distutils や setuptools を使用しないということではありません。Paver は distutils や setuptools コマンドを使用し続けます。Paver のタスクを持つモジュールをインポートするとき、それらのタスクは実行時に自動的に利用可能になります。distutils や setuptools に対して同様にアクセスしたいなら、上述したように ``paver.setuputils.setup`` を使用するか ``paver.setuputils.install_distutils_tasks()`` を呼び出すか、どちらでも使用することができます。

..
    We can see this in action by looking at ``paver help``::

実際に ``paver help`` を実行すると見ることができます。

::

  # <== sh('cd docs/samples/started/newway; paver help')==>
  [?1034h---> paver.tasks.help
  Usage: paver [global options] taskname [task options] [taskname [taskoptions]]

  Options:
    --version             show program's version number and exit
    -n, --dry-run         don't actually do anything
    -v, --verbose         display all logging output
    -q, --quiet           display only errors
    -i, --interactive     enable prompting
    -f FILE, --file=FILE  read tasks from FILE [pavement.py]
    -h, --help            display this help information

  Tasks from distutils.command:
    bdist            - create a built (binary) distribution
    bdist_dumb       - create a "dumb" built distribution
    build            - build everything needed to install
    build_clib       - build C/C++ libraries used by Python extensions
    build_scripts    - "build" scripts (copy and fixup #! line)
    clean            - clean up temporary files from 'build' command
    install_data     - install data files
    install_headers  - install C/C++ header files
    upload           - upload binary package to PyPI

  Tasks from nose.commands:
    nosetests        - Run unit tests using nosetests

  Tasks from paver.doctools:
    cog              - Runs the cog code generator against the files matching your 
      specification
    doc_clean        - Clean (delete) the built docs
    html             - Build HTML documentation using Sphinx
    uncog            - Remove the Cog generated code from files

  Tasks from paver.misctasks:
    generate_setup   - Generates a setup
    minilib          - Create a Paver mini library that contains enough for a simple
      pavement
    paverdocs        - Open your web browser and display Paver's documentation

  Tasks from paver.tasks:
    help             - This help display

  Tasks from setuptools.command:
    alias            - define a shortcut to invoke one or more commands
    bdist_egg        - create an "egg" distribution
    bdist_rpm        - create an RPM distribution
    bdist_wininst    - create an executable installer for MS Windows
    build_ext        - build C/C++ extensions (compile/link to build directory)
    build_py         - "build" pure Python modules (copy to build directory)
    develop          - install package in 'development mode'
    easy_install     - Find/get/install Python packages
    egg_info         - create a distribution's .egg-info directory
    install          - install everything from build directory
    install_egg_info - Install an .egg-info directory for the package
    install_lib      - install all Python modules (extensions and pure Python)
    install_scripts  - install scripts (Python or otherwise)
    register         - register the distribution with the Python package index
    rotate           - delete older distributions, keeping N newest files
    saveopts         - save supplied options to setup.cfg or other config file
    sdist            - create a source distribution (tarball, zip file, etc.)
    setopt           - set an option in setup.cfg or another config file
    test             - run unit tests after in-place build
    upload_docs      - Upload documentation to PyPI

  Tasks from sphinx.setup_command:
    build_sphinx     - Build Sphinx documentation

  Tasks from pavement:
    deploy           - Deploy the HTML to the server
    html             - Build the docs and put them into our package
    sdist            - Overrides sdist to make sure that our setup
  # <==end==>

..
    That command is listing all of the available tasks, and you can see
    near the top there are tasks from distutils.command. All of the
    standard distutils commands are available.

ヘルプコマンドは利用可能な全てのタスクを表示します。その上部付近に distutils.command からのタスクがあります。標準の distutils の全コマンドが利用できます。

..
    There's one more thing we need to do before our Python package
    is properly redistributable: tell distutils about our special files.
    We can do that with a simple MANIFEST.in::

Python パッケージが適切に再配布可能になる前に行う必要があることが1つあります。distutils に特別なファイルを伝えます。それは簡単な MANIFEST.in で実行できます。

::

    # <== include('started/newway/MANIFEST.in')==>
    include setup.py
    include pavement.py
    include paver-minilib.zip
    # <==end==>

..
    With that, we can run ``paver sdist`` and end up with the
    equivalent output file::

そのファイルを使用して ``paver sdist`` を実行すると、最終的に等価な出力ファイルが生成されます。

::

  # <== 
  # sh('cd docs/samples/started/newway; paver sdist',
  #    insert_output=False)
  # sh('ls -l docs/samples/started/newway/dist')
  # ==>
  celkem 32
  -rw-r--r-- 1 lukas.linhart lukas.linhart 28971 22. lis 15.55 TheNewWay-1.0.tar.gz
  # <==end==>

..
    It also means that users of The New Way can also run ``paver install``
    to install the package on their system. Neat.

新しい方法プロジェクトのユーザはシステム上にパッケージをインストールするために ``paver install`` も実行できることにもなります。

..
    But people are used to setup.py!
    --------------------------------

しかし人は setup.py を使用する！
--------------------------------

..
    ``python setup.py install`` has been around a long time. And while
    you could certainly put a README file in your package telling
    people to run ``paver install``, we all know that no one actually
    reads docs. (Hey, thanks for taking the time to read this!)

``python setup.py install`` は長い間使われてきました。 ``paver install`` を実行することをユーザへ伝えるためにパッケージ内に必ず README を置くようにしますが、誰も実際にはそのドキュメントを読まないことを私たちみんなが知っています。(そうそう、このガイドを読む時間を取ってくれてありがとう！)

..
    No worries, though. You can run ``paver generate_setup`` to get a
    setup.py file that you can ship in your tarball. Then your users
    can run ``python setup.py install`` just like they're used to,
    and Paver will take over.

そうだとしても心配しないで。tarball の中に入れる setup.py ファイルを生成するために ``paver generate_setup`` を実行することができます。それから、ユーザは慣れた方法で ``python setup.py install`` を実行して Paver がその処理を引き継ぎます。

..
    But people don't have Paver yet!
    --------------------------------

しかし人はまだ Paver を持っていない！
-------------------------------------

..
    There are millions of Python installations that don't have Paver yet,
    but have Python and distutils. How can they run a Paver-based install?

まだ Paver を持っていない Python のインストール方法は何百万もありますが Python と distutils は持っています。どうやって Pathon ベースのインストールが実行されるだろうか？

..
    Easy, you just run ``paver minilib`` and you will get a file called
    paver-minilib.zip. That file has enough of Paver to allow someone
    to install most projects. The Paver-generated setup.py knows to look
    for that file and use it if it sees it.

それは簡単です、 ``paver minilib`` を単に実行すると paver-minilib.zip というファイルが作られます。そのファイルには多くのプロジェクトをインストールするために Paver の十分な機能を持っています。Paver が生成した setup.py はそのファイルを探すようになっていて、見つかったらそのファイルを使用します。

..
    Worried about bloating your package? The paver-minilib is not large::

あなたのパッケージの肥大化が心配？ paver-minilib は大きくありません。

::

  # <==
  # sh('cd docs/samples/started/newway ; paver minilib',
  #    insert_output=False)
  # sh('ls -l docs/samples/started/newway/paver-minilib.zip')
  # ==>
  -rw-r--r-- 1 lukas.linhart lukas.linhart 27309 22. lis 15.55 docs/samples/started/newway/paver-minilib.zip
  # <==end==>

..
    Paver itself is bootstrapped with a generated setup file and a
    paver-minilib.

Paver そのものは生成された setup ファイルと paver-minilib でブートストラップされます。

..
    Hey! Didn't you just create more work for me?
    ---------------------------------------------

やぁ！あなたは私のためにもっと作業を作らないの？
------------------------------------------------

..
    You might have noticed that we now have three commands to run in
    order to get a proper distribution for The New Way. Well, you can
    actually run them all at once: ``paver generate_setup minilib sdist``.
    That's not terrible, but it's also not great. You don't want to
    end up with a broken distribution just because you forgot one of
    the tasks.

新しい方法プロジェクトの適正なディストリビューションを作るために3つのコマンドが実行できることに気付いたかもしれません。えっと、実際に全てのコマンド ``paver generate_setup minilib sdist`` を同時に実行することはできます。しかし、それは思うほどひどい方法ではありませんが、良い方法でもありません。実際にはあなたはそのタスクのどれかを忘れるので、そのことにより最終的に壊れたディストリビューションを作りたくはないと思います。

..
    By design, one of the easiest things to do in Paver is to extend
    the behavior of an existing "task", and that includes distutils
    commands. All we need to do is create a new sdist task in our
    pavement.py::

意図的に、Paver で実行する最も簡単な方法の1つは既存の "タスク" の処理を拡張して distutils コマンドを含めるようにします。必要な全ての処理を pavement.py の新たな sdist タスクとして作成することです。

::

  # <== include('started/newway/pavement.py', 'sdist')==>
  @task
  @needs('generate_setup', 'minilib', 'setuptools.command.sdist')
  def sdist():
      """Overrides sdist to make sure that our setup.py is generated."""
      pass
  # <==end==>

..
    The @task decorator just tells Paver that this is a task and not just
    a function. The @needs decorator specifies other tasks that should
    run before this one. You can also use the `call_task(taskname)`
    function within your task if you wish. The function name determines
    the name of the task. The docstring is what shows up in Paver's
    help output.

@task デコレータは Paver へこれはタスクであって関数ではないことを伝えます。@needs デコレータはこのタスクが実行される前に実行すべき他のタスクを指定します。お好みでタスク内で `call_task(taskname)` 関数を呼び出すこともできます。その関数名はタスクの名前を決定します。docstring は Paver のヘルプに出力される内容になります。

..
    With that task in our pavement.py, ``paver sdist`` is all it takes
    to build a source distribution after generating a setup file
    and minilib.

上述した pavement.py のタスクで setup ファイルと minilib を生成した後でソースディストリビューションをビルドするために必要なコマンドは ``paver sdist`` だけです。

..
    Tackling the Docs
    -----------------

ドキュメントに立ち向かう
------------------------

..
    Until the tools themselves provide tasks and functions that make
    creating pavements easier, Paver's Standard Library will include
    a collection of modules that help out for commonly used tools. 
    Sphinx is one package for which Paver has built-in support.

そのツールそのものが pavement を簡単に作成するタスクと関数を提供するまで Paver の標準ライブラリは共通ツールとして役立つモジュールのコレクションを追加します。Sphinx は Paver がビルトインサポートしているパッケージの1つです。

..
    To use Paver's Sphinx support, you need to have Sphinx installed
    and, in your pavement.py, ``import paver.doctools``. Just performing
    the import will make the doctools-related tasks available.
    ``paver help html`` will tell us how to use the html command::

Paver の Sphinx サポートを使用するために Sphinx をインストールする必要があります。そして pavement.py に ``import paver.doctools`` を追加します。インポートを実行するだけで doctools 関連のタスクが利用できます。 ``paver help html`` を実行すると html コマンドの使用方法を表示します。

::

  # <== sh('paver help paver.doctools.html')==>
  [?1034h---> paver.tasks.help

  paver.doctools.html
  -------------------
  Usage: paver paver.doctools.html [options]

  Options:
    -h, --help  display this help information

  Build HTML documentation using Sphinx. This uses the following
      options in a "sphinx" section of the options.

      docroot
        the root under which Sphinx will be working. Default: docs
      builddir
        directory under the docroot where the resulting files are put.
        default: build
      sourcedir
        directory under the docroot for the source files
        default: (empty string)
      

  # <==end==>

..
    According to that, we'll need to set the builddir setting, since we're
    using a builddir called "_build". Let's add this to our pavement.py::

表示されたヘルプによると "_build" という builddir を使用するので builddir を設定する必要があります。pavement.py にこの設定を追加してみましょう。

::

  # <== include('started/newway/pavement.py', 'sphinx')==>
  options(
      sphinx=Bunch(
          builddir="_build"
      )
  )
  # <==end==>

..
    And with that, ``paver html`` is now equivalent to ``make html`` using
    the Makefile that Sphinx gave us.

これにより ``paver html`` は Sphinx が持っていた Makefile を使用して ``make html`` を実行することと等価になります。

..
    Getting rid of our docs shell script
    ------------------------------------

ドキュメントからシェルスクリプトを取り除く
------------------------------------------

..
    You may remember that shell script we had for moving our generated
    docs to the right place::

生成したドキュメントを適切な場所へ移動するシェルスクリプトを覚えておいた方が良いです。

::

  # <== include('started/oldway/builddocs.sh')==>
  cd docs
  make html
  cd ..
  rm -rf oldway/docs
  mv docs/_build/html oldway/docs
  # <==end==>

..
    Ideally, we'd want this to happen whenever we generate the docs.
    We've already seen how to override tasks, so let's try that out
    here::

理想的には、ドキュメントを生成するときはこのように実行したいです。タスクをオーバーライドする方法は既に確認したので、ここでそれをやってみましょう。

::

  # <== include('started/newway/pavement.py', 'html')==>
  @task
  @needs('paver.doctools.html')
  def html(options):
      """Build the docs and put them into our package."""
      destdir = path('newway/docs')
      destdir.rmtree()
      builtdocs = path("docs") / options.builddir / "html"
      builtdocs.move(destdir)
  # <==end==>

..
    There are a handful of interesting things in here. The equivalent of
    'make html' is the @needs('paver.doctools.html'), since that's
    the task we're overriding.

この設定には少し興味深いことがあります。オーバーライドしたタスクを使用するので 'make html' と @needs('paver.doctools.html') は等価になります。

..
    Inside our task, we're using "path". This is a customized
    version of Jason Orendorff's path module. All kinds of file
    and directory operations become super-simple using this module.

タスク内部では "path" を使用しています。これは Jason Orendorff がカスタマイズした path モジュールです。どのようなファイルやディレクトリ操作もこのモジュールを使用すると超簡単になります。

..
    We start by deleting our destination directory, since we'll be copying
    new generated files into that spot. Next, we look at the built
    docs directory that we'll be moving::

新たに生成したファイルを対象ディレクトリにコピーするので、先ずはそのディレクトリを削除することから始めます。次に移動するためのビルドした docs ディレクトリを探します。

::

  # <== include('started/newway/pavement.py', 'html.builtdocs')==>
  builtdocs = path("docs") / options.builddir / "html"
  # <==end==>

..
    One cool thing about path objects is that you can use the natural
    and comfortable '/' operator to build up your paths.

path オブジェクトの優れた機能の1つはパスを構成するために '/' 演算子を直感的且つ快適に使用できることです。

..
    The next thing we see here is the accessing of options. The
    options object is available to your tasks. It's basically a dictionary
    that offers attribute-style access and can search for variables
    (which is why you can type options.builddir instead of
    the longer options.sphinx.builddir). That property of options is
    also convenient for being able to share properties between sections.

次にここで紹介したオプションのアクセスです。オプションオブジェクトはタスクで利用できます。それは基本的に属性スタイルのアクセスを提供するディレクトリで(長い options.sphinx.builddir の代わりに options.builddir を入力できる)オプションの変数を検索できます。オプションのプロパティはセクション間でプロパティを共有可能にするためにも便利です。

..
    And with that, we eliminate the shell script as a separate file.

これにより、分割したファイルとしてシェルスクリプトを取り除きます。

..
    Fixing another wart in The Old Way
    ----------------------------------

旧い方法の他の欠点を修正する
----------------------------

..
    In the documentation for The Old Way, we actually included the
    function body directly in the docs. But, we had to cut and paste
    it there. Sphinx does offer a way to include an external file
    in your documentation. Paver includes a better way.

旧い方法のドキュメントでは、実際にその関数の内部に直接的に docs を追加しました。しかし、その場所でカット&ペーストする必要がありました。Sphinx はドキュメントに外部ファイルを含める方法を提供します。Paver はさらにもっと良い方法で追加します。

..
    There are a couple of parts to the documentation problem:

ドキュメント作成には複数の問題があります。

..
    1. It's good to have your code in separate files from your docs
       so that the code can be complete, runnable and, above all,
       testable programs so that you can be sure that everything works.
    2. You want your writing and the samples included with your writing
       to stand up as reasonable, coherent documents. Python's doctest
       style does not always lend itself to coherent documents.
    3. It's nice to have the code sample that you're writing about
       included inline with the documents as you're writing them.
       It's easier to write when you can easily see what you're
       writing about.

1. 何よりもちゃんと動くことを保証するためにテスト可能なプログラムで、実行可能で、完全なコードになるようにドキュメントとは分割したファイルにコードを書くことは良いことです。
2. ドキュメントと、合理的且つ理路整然とした内容に沿うドキュメントに含められるサンプルが欲しいです。Python の doctest スタイルはいつも理路整然としたドキュメントに役立つとは限りません。
3. ドキュメントへインラインで、書かれた内容に関連するコードサンプルを追加することは素晴らしいです。書かれた内容がすぐに理解できる場合、書くことが簡単になります。

..
    #1 and #3 sound mutually exclusive, but they're not. Paver has a
    two part strategy to solve this problem. Let's look at part of the index.rst
    document file to see the first part::

#1 と #3 は相互排他的ですが、そうではありません。Paver ではこの問題を解決するために2種類の戦略があります。1つ目の戦略を理解するために index.rst の一部を見てましょう。

::

  # <== include("started/newway/docs/index.rst", "mainpart")==>
  Welcome to The New Way's documentation!
  =======================================

  This is the Paver way of doing things. The key functionality here
  is in this powerful piece of code, which I will `include` here in its entirety
  so that you can bask in its power::

    # [[[cog include("newway/thecode.py", "code")]]]
    # [[[end]]]

  # <==end==>

..
    In The New Way's index.rst, you can see the same mechanism being used that
    is used in this Getting Started guide. Paver includes Ned Batchelder's
    Cog_ package. Cog lets you drop snippets of Python into a file and have
    those snippets generate stuff that goes into the file. Unlike a template
    language, Cog is designed so that you can leave the markers in and
    regenerate as often as you need to. With a template language, you have
    the template and the finalized output, but not a file that has both.

新しい方法の index.rst では、このスタートガイドで使用されているのと同じ仕組みが見れます。Paver は Ned Batchelder の Cog_ パッケージを含みます。Cog はあるファイルに Python のスニペットを用意して、それらのスニペットが生成した出力内容をそのファイルの中に組み入れます。テンプレート言語とは違い、Cog はファイルにマーカーを入れて必要になる度に再度生成できるように設計されています。テンプレート言語だと、テンプレートと最終的な出力結果を持っていますが、1つのファイルに両方の内容は持てません。

..
    So, as I'm writing this Getting Started document, I can glance up and see
    the index.rst contents right inline. You'll notice The #[[[cog part in there
    is calling an include() function. This is the second part offered by
    Paver. Paver lets you specify an "includedir" for use with Cog.
    This lets you include files relative to that directory. And, critically,
    it also lets you mark off sections of those files so that you can
    easily include just the part you want. In the example above, we're picking
    up the 'code' section of the newway/thecode.py file. Let's take a look
    at that file::

そのため、私がこのスタートガイドを書いているように、チラッと見て index.rst のコンテンツの適切なインラインを確認できます。そこにある #[[[cog の部分は include() 関数を呼び出していることに気付きます。これは Paver で提供される2つめの戦略です。Paver は Cog で使用する "includedir" を指定させます。これは関連するファイルをそのディレクトリに含めるようにさせます。そして、決定的に追加したい部分のみを簡単に含めることができるように、そういったファイルのセクションを区切らせます。上述したサンプルでは newway/thecode.py ファイルの 'code' セクションから取り出します。それでは newway/thecode.py ファイルの中身を覗いてみましょう。

::

  # <== sh("cat docs/samples/started/newway/newway/thecode.py") ==>
  """This is our powerful, code-filled, new-fangled module."""

  # [[[section code]]]
  def powerful_function_and_new_too():
      """This is powerful stuff, and it's new."""
      return 2*1
  # [[[endsection]]]
  # <==end==>

..
    Paver has a Cog-like syntax for defining named sections. So, you just
    use the ``include`` function with the relative filename and the section
    you want, and it will be included. Sections can even be nested (and
    you refer to nested sections using familiar dotted notation).

Paver は名前セクションを定義するために Cog のような構文があります。そのため、関連するファイ名と追加したいそのセクションで ``include()`` 関数を使用してください。セクションはネストして使用することもできます(ドット表記でネストされたセクションを参照します)。

.. _Cog: http://nedbatchelder.com/code/cog/

..
    Bonus Deployment Example
    ------------------------

おまけのデプロイサンプル
------------------------

..
    pavements are just standard Python. The syntax for looping and things
    like that are just what you're used to. The options are standard Python
    so they can contain lists and other objects. Need to deploy to
    multiple hosts? Just put the hosts in the options and loop over them.

pavement.py は普通の Python プログラムです。ループやその他の構文はあなたが慣れ親しんだコーディングと同じです。そのオプションは普通の Python プログラムなのでリストやその他のオブジェクトを含めることができます。複数のホストへデプロイする必要があるときは？単にそのオプションにホストを追加してループさせるだけで良いです。

..
    Let's say we want to deploy The New Way project's HTML files to a
    couple of servers. This is similar to what I do for Paver itself, though
    I only have one server. First, we'll set up some variables to use for
    our deploy task::

複数のサーバへ新しい方法プロジェクトの HTML ファイルをデプロイしたいと仮定してください。私は1つのサーバしか持っていないけれど、これは私が Paver そのものに行うこととよく似ています。先ず、deploy タスクに使用する複数の変数を設定します。

::

  # <== include('started/newway/pavement.py', 'deployoptions')==>
  options(
      deploy = Bunch(
          htmldir = path('newway/docs'),
          hosts = ['host1.hostymost.com', 'host2.hostymost.com'],
          hostpath = 'sites/newway'
      )
  )
  # <==end==>

..
    As you can see, we can put whatever kinds of objects we wish into
    the options. Now for the deploy task itself::

ご覧の通り、オプションの中にどのようなオブジェクトでも望みのモノを追加できます。今は deploy タスクそのもののためです。

::

  # <== include("started/newway/pavement.py", "deploy")==>
  @task
  @cmdopts([
      ('username=', 'u', 'Username to use when logging in to the servers')
  ])
  def deploy(options):
      """Deploy the HTML to the server."""
      for host in options.hosts:
          sh("rsync -avz -e ssh %s/ %s@%s:%s/" % (options.htmldir,
              options.username, host, options.hostpath))
  # <==end==>

..
    You'll notice the new "cmdopts" decorator. Let's say that you have
    sensitive information like a password that you don't want to include
    in your pavement. You can easily make it a command line option for that
    task using cmdopts. options.deploy.username will be set to whatever
    the user enters on the command line.

新たな "cmdopts" デコレータがあります。pavement に記述したくないパスワードのような機密情報があると仮定してください。cmdopts を使用するそのタスクのコマンドラインオプションを簡単に作ることができます。options.deploy.username  はコマンドラインでユーザが入力した内容をセットします。

..
    It's also worth noting that when looking up options, Paver gives
    priority to options in a section with the same name as the task. So,
    options.username will prefer options.deploy.username even if there
    is a username in another section.

オプションオブジェクト内を探す必要もありません。Paver はそのタスクとして同じ名前でセクションにあるオプションを優先します。そのため、options.username は他のセクションで username が存在したとしても options.deploy.username を優先して選択します。

..
    Our deploy task uses a simple for loop to run an rsync command
    for each host. Let's do a dry run providing a username to see
    what the commands will be::

deploy タスクは各ホストに rsync コマンドを実行するために単純なループを使用します。rsync コマンドがどうなるかを確認するためにユーザ名を入力して dry run を実行してみましょう。

::

  # <== sh("cd docs/samples/started/newway; paver -n deploy -u kevin")==>
  ---> pavement.deploy
  rsync -avz -e ssh newway/docs/ kevin@host1.hostymost.com:sites/newway/
  rsync -avz -e ssh newway/docs/ kevin@host2.hostymost.com:sites/newway/
  # <==end==>

..
    Where to go from here
    ---------------------

この次にどこへ行くのか
----------------------

..
    The first thing to do is to just get started using Paver. As you've seen
    above, it's easy to get Paver into your workflow, even with existing
    projects.

最初に行うことは Paver を使い始めることです。これまで説明したように、あなたのワークフローに Paver を組み込むことは既存プロジェクトであっても簡単です。

..
    Use the ``paver help`` command.

``paver help`` コマンドを使用してください。

..
    If you really want more detail now, you'll want to read more about 
    :ref:`pavement files <pavement>` and the 
    :ref:`Paver Standard Library <stdlib>`.

あなたが今すぐ詳細を知りたいなら :ref:`pavement ファイル <pavement>` と :ref:`Paver 標準ライブラリ <stdlib>` を読んでみると良いでしょう。
