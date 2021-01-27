# チートシート
[TOC]

## 定義
```VBA
    Const  staffName As String  = "社員情報"    ' 定数。宣言＋代入
    Dim    staffId   As Integer                 ' 変数。宣言。宣言と同時に代入が出来ない。
    Dim    staffId   As Integer = 10            ' ×(VBAではなくVBなら可能っぽい)
    Dim    staffId = 10                         ' ×(〃)
```

## データ型
```VBA
    Boolean      ' 真偽値

    Byte         '  8bit符号無 整数
    Integer      ' 16bit符号付 整数
    Long         ' 32bit符号付 整数
    LongLong     ' 64bit符号付 整数
    Single       ' 単精度 浮動小数点数型
    Double       ' 倍精度 浮動小数点数型

    String       ' 文字列。文字型は存在しない。文字列は文字の配列ではない。

    Date         ' 日付型(西暦100 年1月1日～西暦9999年12月31日)

    Object       ' オブジェクト型
    Variant      ' 全ての型を含む

```

## リテラル
```
    真偽値          True
                    False
    整数            1234
                    -999
                    2200000000#     ±2147483648を超えると後ろに#を付ける必要がある。
    浮動小数点数    10              整数でも良い
                    10.1
                    20-e05          ある桁数を超えると、自動的にこの形式に変換される
    文字                            文字型はなし
    文字列          "ああああ"      ""で囲む

```

## 構造体
```VBA
' 定義
    Type Staff              ' 型名
        Id  As Integer      ' メンバ
            :
            :
    End Type
' 宣言
    Dim  staff As Staff     ' 通常の型と同じように使える
' メンバへのアクセス
    staff.Id = 1

```

## 演算子
```VBA
    =, <>, >=, <= >, <      ' 比較演算子(等しい, 等くない, 以上、以下、大きい、小さい)
    And, Or, Xor, Not       ' 論理演算子(論理積, 論理和, 排他的論理和, 否定)
    ' 例
    If Not count <> 0 And Name = "Usami" Then
```
## 制御文
```VBA
'if
    If 式   Then        ' Then の後にステートメントは書けない
        ～
    ElseIf
        ～
    Else
        ～
    End If

'for ループ
    For i = 1 To 10 Step 2      ' 数の繰り返し
        Exit For                ' 中断
    Next

    For LBound(Week) = 1 To UBound(Week)    ' Week配列の繰り返し

' while ループ
Do While 式                     ' 条件が満たしている間はループ
    Exit Do                     ' 抜ける
Loop

Do
Loop While  式  ' 条件が満たしている間はループ
```

## 関数
Function～End  と Sub～End Sub の2通りがある。
返り値を返す場合は Function
```VBA
' 定義
    Function  FuncName(Name As String, Id As Integer) As Integer
        :                           ' 処理を書く
        Exit Function               ' 関数を中断
        FuncName = 200              ' 返り値は関数名に代入
    End Function

    Sub  FuncName(Name As String, Id As Integer)
        :                           ' 処理を書く
        Exit Sub                    ' 関数を中断
    End Sub
' 呼び出し
    result = FuncName("ArgName", 10)    ' 返り値がある場合は、引数を()で括る
    result = FuncName "ArgName", 10     ' ×ダメ
    FuncName "ArgName", 10              ' 返り値が無い場合は、引数を()で括らない
    FuncName("ArgName", 10)             ' ×ダメ

```



