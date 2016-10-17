# これは何

明示的な相対インポートに関するサンプル。

明示的な相対インポートを使用しているモジュールを直接実行する場合には、
ファイル名を指定するのではなくモジュール名で指定する必要がある。

パッケージの構成は以下の通り

* mypackage
  * module01
  * utils
  * subpackage
    * module02

module01, module02はutilsを呼び出したい。
この時`from mypackage import utils`という絶対インポートではなく、
`from . import utils`もしくは`from .. import utils`という
明示的な相対インポートを指定することが出来る。

しかし後者を採用すると、submodule01.pyとsubmodule02.pyに含まれる
`if __name__ == '__main__':` 以降の命令を実行するのにファイル名を
指定する方法が使えなくなる。


    $ python mypackage/module01.py
    Traceback (most recent call last):
      File "mypackage/module01.py", line 3, in <module>
        from . import utils
    SystemError: Parent module '' not loaded, cannot perform relative import
    $ python mypackage/subpackage/module02.py
    Traceback (most recent call last):
      File "mypackage/subpackage/module02.py", line 3, in <module>
        from .. import utils
    SystemError: Parent module '' not loaded, cannot perform relative import

対策は`-m`を用いること。

    $ python -m mypackage.module01
    Util Func Called
    $ python -m mypackage.subpackage.module02
    Util Func Called


# 参考

* http://blog.amedama.jp/entry/2016/05/31/213741
* http://docs.python.jp/3/tutorial/modules.html
