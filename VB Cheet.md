# チートシート

## リテラル

## 定義

    staffSheetName As String = "社員情報"
    Const staffSheetName As String = "社員情報"     定数    関数内、外OK
    Dim

## 演算子
	=, <>, >=, <= >, <		比較演算子(等しい, 等くない, 以上、以下、大きい、小さい)
	And, Or, Not			論理演算子(論理積, 論理和, 論理否定)
	' 例
	If Not count <> 0 And Name = "Usami" Then

## 制御文
```Visual Basic .NET
'if
    If 式   Then        // Then の後に改行が必要
        ～
    ElseIf
        ～
    Else
        ～
    End If
'for
	// 数の繰り返し
    For i = 1 To Range()
        if Exit Next
    Next

	// 配列の繰り返し
	Dim Week() As String 
    For LBound(Week) = 1 To HBound(Week)
```

## 関数
