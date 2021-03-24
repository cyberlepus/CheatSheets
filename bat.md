# if
基本的な書き方。elif といった書き方はない。
```
    if 条件式 コマンドA
    if 条件式 コマンドA else コマンドB
    if 条件式 コマンドA else if 条件式 コマンドB else コマンドC
```
()で括ることで改行を含んだ複数のコマンドを１つとして認識される。以下の様に書くことができる。
注意点として(の前と)の後には必ず半角スペースを入れる必要がある。
```
    if 条件式 (
        コマンドA-1
        コマンドA-2
    ) else if 条件式 (
        コマンドB-1
        コマンドB-2
    ) else (
        コマンドC-1
        コマンドC-2
    )
```
echoコマンドはコマンド以降の行終了まで全て表示文と認識される。
パイプライン(|)やリダイレクト(>)があった場合も同様。
()を使う事で1行で書く事もできる
```
    ×  if 条件式 echo TRUE! else echo FALSE!
    ×  if 条件式 echo TRUE! > out.txt else echo FALSE! > out.txt
        ↓
    〇  if 条件式 (echo TRUE!) else (echo FALSE!)
    〇  if 条件式 (echo TRUE! > out.txt) else (echo FALSE! > out.txt)
```
## 条件式
```
文字列
    "A" == "B"      AとBの値は等しい
    not "A" == "B"  AとBの値は等しくない
    /i "A" == "B"   条件式の前 /i つけると大文字、小文字の区別をつけないで比較
    文字列は""で括らなくても成り立つが、あった方が安全。例えば変数を使った場合に空文字列でも成り立つ。

数値
    A equ B         AはBと等しい
    A neq B         AはBと等しくない
    A gtr B         AはBより大(a >  b)
    A geq B         AはB以上  (a >= b)
    A lss B         AはBより小(a <  b)
    A leq B         AはB以下  (a <= b)
ファイルがあるか
    exist  ファイルパス
環境変数が定義されているか
    defined  環境変数
補足
    - 条件式の前に not を入れると、否定となる。
    - 条件式の論理演算子はない。AND は以下のように書ける。
      if 条件式A if 条件式B
```

## Robocopy

コピーの指定
```
/COPYALL    コピーをする(全てのファイル情報＝属性、タイムスタンプ、セキュリティ、所有者、監査...等)。
/COPY:DAT   コピーをする(データ、属性、タイムスタンプ。指定しない場合のデフォルト動作
/L          コピーしない(テスト用)
/XO         コピー元が新しいならコピー。
```
少し特殊な指定
```
/XJ         ディレクトリとファイルの接合ポイントとシンボリック リンクを除外。
            /XJD = ディレクトリのみ /XJF = はファイルのみ
/XL         コピー先にファイルがあるのみコピー。
```
robocopyは非常にログが冗長。抑制のオプションは付けとくべき。
コピー処理でもコピー先にあるファイルも表示されるため分かりにくい。
```
/XX     コピー先のみのファイルを除外する。
        コピー処理では処理の動作に無関係だが、ログ抑制のためにつけた方が良い。
/NDL    ディレクトリとファイル名で行が分かれて表示されるのを防ぐ
                       1    D:\APK\cc2_k_usami_APK\trunk\APK\Source\QAReport\public\
        *EXTRA File         1248    QAReportDef.cpp
                            ↓
        *EXTRA File         1248    D:\Project\SVN-Tools-Windows\QA\QAReport\CommonSource\QAReportDef.cpp
/NC     '*EXTRA File' や 'Newer' といった表示を消す
/NS     ファイルサイズを消す
/NJS    「処理後の情報」を表示しないようにする。
                     Total    Copied   Skipped  Mismatch    FAILED    Extras
          Dirs :         1         0         1         0         0         3
         Files :         1         0         1         0         0         1
         Bytes :     1.2 k         0     1.2 k         0         0       462
         Times :   0:00:00   0:00:00                       0:00:00   0:00:00
         Ended : 2021年1月29日 10:30:55

/NJH    「処理前の情報」を表示しないようにする。
        -------------------------------------------------------------------------------
           ROBOCOPY     ::     Robust File Copy for Windows
        -------------------------------------------------------------------------------
          Started : 2021年1月29日 10:36:46
           Source : D:\APK\cc2_k_usami_APK\trunk\APK\Source\QAReport\public\
             Dest : D:\Project\SVN-Tools-Windows\QA\QAReport\ProjectUE4\
            Files : *.*
          Options : *.* /NJS /L /DCOPY:DA /COPY:DAT /XL /XO /R:1000000 /W:30
        ------------------------------------------------------------------------------
```
