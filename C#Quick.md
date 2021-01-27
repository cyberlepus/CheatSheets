# C#メモ

## 文字フォーマット


### 文字列：フォーマット出力
```C#
string.Format("format", ～);
"{0},{1},{2}...で対応する引数の数値を取る。
桁そろえ：右揃え桁数、'-'をつけると左揃え

```
{ *引数Index* , *桁そろえ* : *書式* }

### Cast
```C#
    // class Foo : FooBase
    Foo     foo       = new Foo();
    FooBase fooBase   = (FooBase)foo;       // ()  変換できなければ、InvalidCastException を返す
    FooBase fooBase   = foo as FooBase;     // as  変換できなければ null を返す
    bool    isFooBase = foo is FooBase;     // is  変換の可否を返す
```
値型をobjectにボックス化した場合、元に戻す場合は元の値型でキャストする。

```C#
    object lObj   = (long)123;
    object iObj   = (int) 123;
    int    iVal0  = (int) iObj;     // OK
    long   lVal0  = (long)lObj;     // OK
    int    iVal1  = lVal0;          // Compile Error 暗黙のアップキャストは不可
    long   lVal1  = iVal0;          // OK
    long   lVal2  = (long)iObj;     // × InvalidCastException
    int    iVal2  = (int) lObj;     // × InvalidCastException
```
実数、整数型(double, float, long, int, short) <-> 真偽値 bool はキャストでも変換不可。
```C#
    int    i0  = 123;
    bool   b0  = (bool)i0;          // Compile Error
```
同じビット幅での signed, unsigned の違いは暗黙の変換は不可。キャストならOK
```C#
    uint  ui0 = 123;
    int   i0 = ui0;                 // Compile Error
    int   i0 = (int)ui0;            // OK
```


### 演算子
    ==, !=, <=, <, >, >=

    string, bool は  ==, != しか使用できない
    char, short, int, long, float double は  <=, <, >, >=, ==, != が使用できる

    // ↑ Type.isValueType == true なら使える模様。


