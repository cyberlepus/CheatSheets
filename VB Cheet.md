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
| リテラル     | 例           | 備考 |
| ---          | ---          | --- |
| 真偽値       | True         | |
|              | False        | |
| 整数         | 1234         | |
|              |  -999        | |
|              | 2200000000#  | ±2147483648を超えると後ろに#を付ける必要がある。 |
| 浮動小数点数 | 10           | 整数でも良い |
|              | 10.1         | |
|              | 20-e05       | ある桁数を超えると、自動的にこの形式に変換される |
|  文字        |              | 文字型はなし |
|  文字列      | "ああああ"   | ""で囲む |


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
## 配列
変数に() をつけると配列。配列の添え字は0以外でも可。
宣言時の個数指定+1=要素数になるので注意が必要。
```VBA
  ' 宣言時に要素数の最初を1にする
  ' Option Base 1

  ' 静的配列宣言  以後要素数は変えられない
  Dim  intArray(9)  As Integer    ' 0～9  の10個の要素
  Dim  intArray(1 To 10)  As Integer  ' 1～10 の10個の要素
  Dim  intArray(9, 9)  As Integer   ' 0～9の10個 x 0～9の10個、の2次元配列

  ' 動的配列宣言
  Dim  intArray()  As Integer     ' 数が未定。後でReDimで個数を設定する
  ReDim  intArray(100)            ' 内容を初期化して要素数を変更する。
  ReDim  Preserve intArray(100)   ' 内容を保持して要素数を変更する。

  UBound(intArray)
  LBound(intArray)

  ' Foreach をする場合
  Dim  intItem  As  Integer
  For Each  intItem  In  intArray
  ' For をする場合
  For  i = LBound(intArray)  To  UBound(intArray)
```

## 演算子
  ```VBA
    =, <>, >=, <= >, <          ' 比較演算子(等しい, 等くない, 以上、以下、大きい、小さい)
    And, Or, Xor, Not           ' 論理演算子(論理積, 論理和, 排他的論理和, 否定)
    ' 例
    If Not count <> 0 And Name = "Usami" Then
  ```

## 制御文

if
  ```VBA
      If 式   Then              ' Then の後にステートメントは書けない
          ～
      ElseIf
          ～
      Else
          ～
      End If
  ```
for ループ
  ```VBA
      For i = 1 To 10 Step 2    ' 数の繰り返し
          Exit For              ' 中断
      Next

      For LBound(Week) = 1 To UBound(Week)    ' Week配列の繰り返し
  ```
while ループ
  ```VBA
      Do While 式               ' 条件が満たしている間はループ
          Exit Do               ' 抜ける
      Loop

      Do
      Loop While  式            ' 条件が満たしている間はループ
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


  ThisWorkbook.Path はExcelファイルのあるディレクトリを指す
```VBA
    ' カレントディレクトリを実行ファイルのあるパスにする
    CreateObject("WScript.Shell").CurrentDirectory = ThisWorkbook.Path
```

### パス操作
FileSystemObject を使う。[ツール]メニュー > [参照設定] > [Microsoft Scripting Runtime] をチェック
```VBA
Dim fso As FileSystemObject
Set fso = New FileSystemObject

  fso.BuildPath(ThisWorkbook.Path, "test.txt")
'後始末
Set fso = Nothing
```


### メッセージボックス
MsgBox()


### 削除
    sheet.Range(.Cell(row, 3), .Cell(rowEnd - 1, 6)).Select
    sheet.Delete (xlShiftToUp)

# Excel操作

## シートの取得
```VBA
    Dim wb as Workbook
    wb = Workbooks("Book1.xlsx")
    Dim ws As Worksheet
    Set ws = wb.Sheets(1)           ' 左からのインデックスで指定(1～)
    Set ws = wb.Sheets("Sheet1")    ' シート名で指定
    Set ws = wb.ActiveSheet         ' 現在のアクティブシート
```

### Range指定
sheet.Range(sheet.Cells(1,2), sheet.Cells(5,6))
sheet.Range(sheet.Cells("B1"), sheet.Cells(F5))
sheet.Range(sheet.   Rows(1), sheet.Rows   (5))
sheet.Range(sheet.Columns(1), sheet.Columns(3))

### Range 取得
```VBA
    Dim rng As Range
    Set rng = Range("A2:D10")

    sRow = rng.Row                              ' 2 →  2行目
    sCol = rng.Column                           ' 1 →  1列目(A列)
    eRow = rng.Row + rng.Rows.Count - 1         '10 → 10行目
    eCol = rng.Column + rng.Columns.Count - 1   ' 4 →  4列目(D列)
```


wb. を省略した場合は、現在のアクティブな Workbookになる。

