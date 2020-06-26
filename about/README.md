# C++の概要

C++は、1983年にBjarne Stroustrup(ビャーネ・ストロヴストルップ)によって開発された。C言語という既存の言語に、Simulaという別の言語が持つ「オブジェクト指向」という概念を「クラス」という形で導入したものだ。そのため、C++は概ねC言語のプログラムがそのまま動作する。ただし、C言語はC言語で独自に発展を遂げたため、一部非互換な箇所がある。

C++の特徴は、抽象的なコーディングを可能とする「高いレイヤー」と、ハードウェアに近い「低いレイヤー」の共存を可能とするところである。C++は最近の抽象的なプログラミングの概念を取り込んで成長を続けているが、一方でアセンブリやメモリ周りを直接いじるなど、ハードウェアが透けてみえるコーディングが可能である。これにより、大規模なソフトウェア開発をしつつ、メモリ効率、実行効率が高いコーディングが可能となる。

特にメモリ管理は(原則として)プログラマに任されており、JavaやPythonなどが実装しているガベージコレクションが無いのが特徴である。ガベージコレクションがある言語では、メモリの確保、解放は言語処理系が管理してくれるため、プログラマはどのメモリがいつ確保され、いつ解放されるかを意識しなくてすむ。しかし、C++ではメモリ管理はプログラマの責任であり、明示的にメモリの確保、解放をしなければならない。ただし、近年ではスマートポインタなど導入により、メモリ管理の責任を言語処理系が担ってくれるようになっている。

C++は、コンパイル言語である。Pythonなどはスクリプトをそのまま実行できるが、C++は、コンパイラにより「コンパイル」という作業を行い、実行バイナリを作成する。この実行バイナリは、実行するハードウェアの機械語で書かれており、そのまま実行できる。さらに、「なるべくコンパイル時にいろいろ解決する」という哲学があり、言語仕様に影響を与えている。

詳細な使い方は後で説明するとして、以下では主にPythonと比較しながら、C++という言語の特徴について概観する。

## コンパイルとリンク

C++はコンパイル言語であり、ソースコードをコンパイルすることで実行バイナリを作成する。実行バイナリは、そのアーキテクチャの機械語で記述されているため、「そのまま」実行することができる。このようなコードを「ネイティブコード」と呼ぶ。

C++のソースは「プログラム」と「ヘッダファイル」からなる。ヘッダファイルは、プログラムからインクルードされるためのファイルで、詳細は後述する。

C言語のソースファイルの拡張子は「c」であることが多いが、C++のソースファイルの拡張子は「cc」や「cpp」がよく使われる。通常、C言語とC++のコンパイラはセットで提供される。例えば、GNUが提供するCコンパイラは`gcc`、C++コンパイラは`g++`であり、LLVMのCコンパイラは`clang`、C++コンパイラは`clang++`である。インテルコンパイラの場合はCコンパイラが`icc`、C++コンパイラは`icpc`である。

例えばよくある「Hello World」は、C++では以下のようになる。

```cpp
#include <iostream>

int main(){
    std::cout << "Hello World" << std::endl;
}
```

これを`test.cpp`というファイルに保存して、コンパイル、実行してみる。コンパイラは`g++`を使ってみよう。

```sh
g++ test.cpp
```

すると、`a.out`という実行バイナリができるので実行してみる。

```sh
$ ./a.out
Hello World
```

上記の`std::cout`を使うのがC++標準のやり方だが、Pythonから入った人はC言語の`printf`関数を使うやり方のほうがわかりやすいかもしれない。

```cpp
#include <cstdio>

int main(){
  printf("Hello World\n");
}
```

これをコンパイル、実行しても同様な結果が得られる。このドキュメントではあまりC++標準のやり方にこだわらず、C言語の関数を断りなく使うことにする。

## main関数について

Pythonの場合、プログラムは言語処理系に食わせたスクリプトの「上から」順番に実行される。

```py
print("Hello Python")
```

したがって、命令を「直に」書くことができた。

しかし、C++は、全ての命令は関数の中に書かなければならない。

```cpp
void hello1(void){
  printf("Hello1\n");
}

void hello2(void){
  printf("Hello2\n");
}

void hello3(void){
  printf("Hello3\n");
}
```

このままではどれを実行して良いかがわからないので、C++では「main」という関数を最初に実行すると決められている。

```cpp
int main(void){
  printf("Hello Main\n");
}
```

「`main`という特別な関数があり、その関数が最初に呼び出される」という仕様はC言語から引き継いだもので、JavaやD言語、Rust、GOなど、多くの言語で採用されている。

## プログラムの書き方について

Pythonはブロックをインデントで表現する「オフサイドルール」を採用していた。したがって、プログラムは誰がかいても似たような見た目になる。

```py
def func():
    s = 0
    for i in range(10):
        s += i
    return s

print(func())
```

さらに、改行が文の区切りである。

一方C++言語は、ブロックを中括弧 `{}`で表現する。また、文の区切りはセミコロン`;`であり、余計な改行や空白は無視される。

例えば先程のコードと同等なコードをC++で書くとこのようになる。

```cpp
#include <cstdio>

int func(void) {
  int s = 0;
  for (int i = 0; i < 10; i++) {
    s += i;
  }
  return s;
}

int main() {
  printf("%d\n", func());
}
```

ここでは、ブロック開始の中括弧を行末に書くスタイルを採用しているが、行頭におくスタイルもある。

```cpp
#include <cstdio>

int func(void)
{
  int s = 0;
  for (int i = 0; i < 10; i++)
  {
    s += i;
  }
  return s;
}

int main()
{
  printf("%d\n", func());
}
```

文の区切りがセミコロンであるので、改行はどこに入れても良い。例えば、

```cpp
  printf("%d\n", func());
```

は、

```cpp
  printf(
  "%d\n"
  ,func()
  );
```

と書いても(読みづらいが)問題ない。

文法上はどのように書いても問題ないが、あまり独自のスタイルで書くと他の人が読みづらいため、ある程度は標準のスタイルを踏襲して書いたほうが良い。C++のコーディングスタイルはいくつかあるが、フォーマッティングは原則としてclang-formatなどのスタイルフォーマッタに任せるのが良い。VSCodeなど、主要なエディタであれば保存時にフォーマッタが走るようにできるので設定しておくこと。

## 変数や関数の宣言について

変数とはメモリ上に確保された領域に貼られたラベルのようなものであり、変数を使うためには、まずメモリ上にその変数用の領域を確保する必要がある。そのために行うのが変数の宣言である。Pythonでは変数への値の代入が宣言を兼ねていたが、C++では変数の宣言と代入が別れている。また、Pythonでは変数の型は指定しなくてよかったが、C++では型を明示しなければならない。

例えばPythonで、以下のようなコードを書いたとする。

```py
a = 1
```

このコードは

* このコードを実行する時点で`a`という変数がなければ作成してから1を代入する
* 既に`a`という変数があれば、それに1を代入する

という動作をする。この代入が終わった時点で、`a`は`int`型になっている。

```py
a = 1
type(a) #=> int
```

C++では、変数は使う前にかならず「型」を明示して宣言をする必要がある。

```cpp
int a;
```

これは「`int`型の変数`a`を使うよ」という宣言であり、この時点で`a`のためのメモリが確保される。変数の宣言後は代入が可能になる。

```cpp
int a;
a = 1;
```

宣言と代入(初期化)は同時に行うこともできる。

```cpp
int a = 1;
```

宣言の前に代入を行おうとするとコンパイルエラーになる。

```cpp
a = 1; // エラー
```

また、原則としてC++では一度宣言された変数の型は変更できない。

例えばPythonでは、変数の型は途中で変えることができる。

```py
a = 1
type(a) # => int
a = 'text'
type(a) # => int
```

しかし、C++では、適合しない型の値を代入しようとするとエラーとなる。

```cpp
int a = 1;
a = "text"; // エラー
```

Pythonでは、実行時に定義があれば変数や関数を使うことができる。例えばこんなコードを考える。

```py
def func2():
    func1()

def func1():
    print("Hello")

func2()
```

このプログラムの`func2`の定義時点では`func1`は定義されていない。しかし、`func2`を呼び出す段階では`func1`は定義されているため、問題なく実行することができる。

しかし、C++では、ファイルを上から読んでいって、「その時点で定義されていない関数や変数」は使うことができない。

```cpp
#include <cstdio>

void func2(){
  func1();  // エラー。func1なんて知らないよ、と怒られる
}

void func1(){
  printf("Hello\n");
}

int main() {
  func2();
}
```

順序を入れ替えて、`func2`より先に`func1`を書くようにすればコンパイル、実行できる。

```cpp
#include <cstdio>

void func1(){
  printf("Hello\n");
}

void func2(){
  func1();
}

int main() {
  func2();
}
```

## C++のバージョンについて

PythonがPython2とPython3でかなり違うように、C++にもバージョンがある。ただし、C++は後方互換性を重視しており、、原則として新しいバージョンのC++コンパイラでも、古いバージョンのソースコードをそのままコンパイル、実行することができ、同じ結果が得られる。

Pythonでは、言語処理系に`--version`を指定すると、「言語のバージョン」を得ることができた。

```sh
$ python --version
Python 2.7.5
$ python3 --version
Python 3.6.8
```

しかし、C++では、コンパイラに`--version`を指定しても、得られるのは「コンパイラのバージョン」であって、「言語のバージョン」ではない。以下はMacにおいて、GCCとClangのC++コンパイラのバージョンを表示した例である。

```sh
$ g++-9 --version
g++-9 (Homebrew GCC 9.1.0) 9.1.0
Copyright (C) 2019 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

$ clang++ --version
Apple clang version 11.0.3 (clang-1103.0.32.62)
Target: x86_64-apple-darwin19.5.0
Thread model: posix
InstalledDir: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin
```

C++の言語仕様は国際規格として定められており、C++の「言語としてのバージョン」は、その規格と対応している。例えば 1998年に定められた「SO/IEC 14882:1998」は、通称「C++98」と呼ばれている。C++は2011年に定められた「ISO/IEC 14882:2011」、通称「C++11」で大きな変更を受けた。その後、概ね3年ごとに規格が改定され、C++14、C++17などと呼ばれている。

コンパイラがどの規格のC++をコンパイルできるかは、コンパイラのバージョンだけからはわからない。使いたい規格に対応しているコンパイラでも、コンパイル時に指定が必要な場合がある。例えばMacのclang++ 11.0.3では、C++11の機能をコンパイルしようとすると、警告を出してくる。

```cpp
int func() {
  return 1;
}

int func2() {
  auto a = func();
  return a;
}
```

```sh
$ clang++ -c test.cpp
test.cpp:6:3: warning: 'auto' type specifier is a C++11 extension
      [-Wc++11-extensions]
  auto a = func();
  ^
1 warning generated.
```

これは、「C++11の機能を使ってるけど、意識的か？」という確認だ。`-std=c++11`オプションを指定することで、「C++11の機能を使うよ」と宣言できる。

```sh
clang++ -c -std=c++11 test.cpp
```

多くの言語処理系で「どのバージョンのC++を使うか」を宣言するオプションを持つので、C++14や17を使いたければ、その旨を宣言してやる必要がある。ただし、このドキュメントでは原則としてC++11までの機能しか使わず、かつ利用を想定しているコンパイラはC++11に対応しているため、「C++のバージョン」についてはあまり気にしなくて良い。