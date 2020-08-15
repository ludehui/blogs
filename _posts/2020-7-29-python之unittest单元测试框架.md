---
published: true
title: python之unittest单元测试框架
category: unittest
tags: 
  - python
  - unittest
layout: post
---



# python之unittest单元测试框架

## 1.unittest介绍

提到python的单元测试框架，大家第一个想到的就是unittest。本文将为大家介绍目前流行的 Python 的单元测试框架，以python3为例讲讲它们的功能和特点并比较其异同，以让大家在面对不同场景、不同需求的时候，能够权衡利弊，选择最佳的单元测试框架。

[`unittest`](https://docs.python.org/zh-cn/3/library/unittest.html#module-unittest) 单元测试框架是受到 JUnit 的启发，与其他语言中的主流单元测试框架有着相似的风格。其支持测试自动化，配置共享和关机代码测试。支持将测试样例聚合到测试集中，并将测试与报告框架独立。

它支持自动化设计，多个测试用例共享前置（setUp()）和（tearDown()）代码，聚合多个测试用例到测试集中，并将测试和报告框架独立。

### 1.1 一些概念

**测试脚手架：***test fixture* 表示为了开展一项或多项测试所需要进行的准备工作，以及所有相关的清理操作。举个例子，这可能包含创建临时或代理的数据库、目录，再或者启动一个服务器进程。

**测试用例：**一个测试用例是一个独立的测试单元。它检查输入特定的数据时的响应。 [`unittest`](https://docs.python.org/zh-cn/3/library/unittest.html#module-unittest) 提供一个基类： [`TestCase`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase) ，用于新建测试用例。

**测试套件：***test suite* 是一系列的测试用例，或测试套件，或两者皆有。它用于归档需要一起执行的测试。

**测试运行器：***test runner* 是一个用于执行和输出测试结果的组件。这个运行器可能使用图形接口、文本接口，或返回一个特定的值表示运行测试的结果。

## 2. 使用方法

### 2.1 一个实例

下面贴一段简单的代码来测试三种字符串方法：

```
import unittest

class TestStringMethods(unittest.TestCase):

    def test_upper(self):
        self.assertEqual('foo'.upper(), 'FOO')

    def test_isupper(self):
        self.assertTrue('FOO'.isupper())
        self.assertFalse('Foo'.isupper())

    def test_split(self):
        s = 'hello world'
        self.assertEqual(s.split(), ['hello', 'world'])
        # check that s.split fails when the separator is not a string
        with self.assertRaises(TypeError):
            s.split(2)

if __name__ == '__main__':
    unittest.main()
```

以上代码中首先创建了一个TestStringMethods类，该类继承[unitest.TestCase](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase)类，这样我们就完成了一个测试样例的创建，其中含有三个独立的测试是三个类的方法，这些方法的命名都以 `test` 开头。,这个命名约定告诉测试运行者类的哪些方法表示测试。

每个用例都采用 `unittest` 内置的断言方法来判断被测对象的行为是否符合预期，比如：

- 在 `test_upper` 测试中，使用 [assertEqual](https://link.zhihu.com/?target=https%3A//docs.python.org/3/library/unittest.html%23unittest.TestCase.assertEqual) 检查是否是预期值
- 在 `test_isupper` 测试中，使用 [assertTrue](https://link.zhihu.com/?target=https%3A//docs.python.org/3/library/unittest.html%23unittest.TestCase.assertTrue) 或 [assertFalse](https://link.zhihu.com/?target=https%3A//docs.python.org/3/library/unittest.html%23unittest.TestCase.assertFalse) 验证是否符合条件
- 在 `test_split` 测试中，使用 [assertRaises](https://link.zhihu.com/?target=https%3A//docs.python.org/3/library/unittest.html%23unittest.TestCase.assertRaises) 验证是否抛出一个特定异常

最后的代码块中，演示了运行测试的一个简单的方法。 [`unittest.main()`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.main) 提供了一个测试脚本的命令行接口。当在命令行输运行该测试脚本，上文的脚本生成如以下格式的输出:

```
...
----------------------------------------------------------------------
Ran 3 tests in 0.000s

OK
```

在在调用测试脚本时添加 `-v` 参数使 [`unittest.main()`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.main) 显示更为详细的信息，生成如以下形式的输出:

```
test_isupper (__main__.TestStringMethods) ... ok
test_split (__main__.TestStringMethods) ... ok
test_upper (__main__.TestStringMethods) ... ok

----------------------------------------------------------------------
Ran 3 tests in 0.001s

OK
```

以上例子演示了 [`unittest`](https://docs.python.org/zh-cn/3/library/unittest.html#module-unittest) 中最常用的、足够满足许多日常测试需求的特性。文档的剩余部分详述该框架的完整特性。

### 2.2 命令行界面

#### 2.2.1 命令行指令

unittest 模块可以通过命令行运行模块、类和独立测试方法的测试:

```
python -m unittest test_module1 test_module2
python -m unittest test_module.TestClass
python -m unittest test_module.TestClass.test_method
```

你可以传入模块名、类或方法名或他们的任意组合。

同样的，测试模块可以通过文件路径指定:

```
python -m unittest tests/test_something.py
```

这样就可以使用 shell 的文件名补全指定测试模块。所指定的文件仍需要可以被作为模块导入。路径通过去除 '.py' 、把分隔符转换为 '.' 转换为模块名。若你需要执行不能被作为模块导入的测试文件，你需要直接执行该测试文件。

在运行测试时，你可以通过添加 -v 参数获取更详细（更多的冗余）的信息。

```
python -m unittest -v test_module
```

当运行时不包含参数，开始 [探索性测试](https://docs.python.org/zh-cn/3/library/unittest.html#unittest-test-discovery)

```
python -m unittest
```

用于获取命令行选项列表：

```
python -m unittest -h
```

#### 2.2.2 命令行选项

unittest支持这些命令行选项:

- `-b````, ``--buffer```

  在测试运行时，标准输出流与标准错误流会被放入缓冲区。成功的测试的运行时输出会被丢弃；测试不通过时，测试运行中的输出会正常显示，错误会被加入到测试失败信息。

- `-c````, ``--catch```

  当测试正在运行时， Control-C 会等待当前测试完成，并在完成后报告已执行的测试的结果。当再次按下 Control-C 时，引发平常的 [`KeyboardInterrupt`](https://docs.python.org/zh-cn/3/library/exceptions.html#KeyboardInterrupt) 异常。See [Signal Handling](https://docs.python.org/zh-cn/3/library/unittest.html#signal-handling) for the functions that provide this functionality.

- `-f````, ``--failfast```

  当出现第一个错误或者失败时，停止运行测试。

- `-k```

  只运行匹配模式或子串的测试方法和类。可以多次使用这个选项，以便包含匹配子串的所有测试用例。包含通配符（*）的模式使用 [`fnmatch.fnmatchcase()`](https://docs.python.org/zh-cn/3/library/fnmatch.html#fnmatch.fnmatchcase) 对测试名称进行匹配。另外，该匹配是大小写敏感的。模式对测试加载器导入的测试方法全名进行匹配。例如，`-k foo` 可以匹配到 `foo_tests.SomeTest.test_something` 和 `bar_tests.SomeTest.test_foo` ，但是不能匹配到 `bar_tests.FooTest.test_something` 。

- `--locals```

  在回溯中显示局部变量。

命令行亦可用于探索性测试，以运行一个项目的所有测试或其子集。

### 2.3 Test Fixtures

Test Fixtures测试前置（setUp）和清理（tearDown）方法。

- **[setUp()](https://link.zhihu.com/?target=https%3A//docs.python.org/3/library/unittest.html%23unittest.TestCase.setUp) ：**测试前置方法 用来做一些准备工作，比如建立数据库连接。它会在用例执行前被测试框架自动调用。
- **[tearDown()](https://link.zhihu.com/?target=https%3A//docs.python.org/3/library/unittest.html%23unittest.TestCase.tearDown) ：**测试清理方法 用来做一些清理工作，比如断开数据库连接。它会在用例执行完成（包括失败的情况）后被测试框架自动调用。测试前置和清理方法可以有不同的执行级别。

#### 2.3.1 生效级别：测试类

如果我们希望单个测试类中只执行一次前置方法，再执行该测试类中的所有测试，最后执行一次清理方法，那么需要在测试类中定义好 [setUpClass()](https://link.zhihu.com/?target=https%3A//docs.python.org/3/library/unittest.html%23unittest.TestCase.setUpClass) 和 [tearDownClass()](https://link.zhihu.com/?target=https%3A//docs.python.org/3/library/unittest.html%23unittest.TestCase.tearDownClass)：

```text
class MyTestCase(unittest.TestCase):
    def setUpClass(self):
        pass

    def tearDownClass(self):
        pass
```

#### 2.3.2 生效级别：测试模块

如果我们希望单个测试模块中只执行一次前置方法，再执行该模块中所有测试类的所有测试，最后执行一次清理方法，那么需要在测试模块中定义好 [setUpModule()](https://link.zhihu.com/?target=https%3A//docs.python.org/3/library/unittest.html%23setupmodule-and-teardownmodule) 和 [tearDownModule()](https://link.zhihu.com/?target=https%3A//docs.python.org/3/library/unittest.html%23setupmodule-and-teardownmodule)：

```text
def setUpModule():
    pass

def tearDownModule():
    pass
```

### 2.4 跳过测试和预计失败

`unittest` 支持直接跳过或按条件跳过测试，也支持预计测试失败：

- 通过 [skip](https://link.zhihu.com/?target=https%3A//docs.python.org/3/library/unittest.html%23unittest.skip) 装饰器或 [SkipTest](https://link.zhihu.com/?target=https%3A//docs.python.org/3/library/unittest.html%23unittest.SkipTest) 直接跳过测试
- 通过 [skipIf](https://link.zhihu.com/?target=https%3A//docs.python.org/3/library/unittest.html%23unittest.skipIf) 或 [skipUnless](https://link.zhihu.com/?target=https%3A//docs.python.org/3/library/unittest.html%23unittest.skipUnless) 按条件跳过或不跳过测试
- 通过 [expectedFailure](https://link.zhihu.com/?target=https%3A//docs.python.org/3/library/unittest.html%23unittest.expectedFailure) 预计测试失败

```text
class MyTestCase(unittest.TestCase):

    @unittest.skip("直接跳过")
    def test_nothing(self):
        self.fail("shouldn't happen")

    @unittest.skipIf(mylib.__version__ < (1, 3),
                     "满足条件跳过")
    def test_format(self):
        # Tests that work for only a certain version of the library.
        pass

    @unittest.skipUnless(sys.platform.startswith("win"), "满足条件不跳过")
    def test_windows_support(self):
        # windows specific testing code
        pass

    def test_maybe_skipped(self):
        if not external_resource_available():
            self.skipTest("跳过")
        # test code that depends on the external resource
        pass

    @unittest.expectedFailure
    def test_fail(self):
        self.assertEqual(1, 0, "这个目前是失败的")
```



























