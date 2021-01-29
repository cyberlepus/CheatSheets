## Robocopy
```
/COPY   コピーをする
/L      コピーしない
```
robocopyはログ表示が冗長。抑制のオプションは付けとくべき
```
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
