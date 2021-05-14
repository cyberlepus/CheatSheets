<!--meta http-equiv="refresh" content="10"-->

C#メモ
================================================================================

非同期処理
----------------------------------------------------------------
最初は Thread クラスを使っていたが .NetFramework4 以降では Task クラスが実装されて
簡易に使えるようになったので、Thread クラスは使用しないで Task クラスを使う。

### 別スレッドでの処理を開始

Task を使ってワーカースレッド簡単に起動できるが、返り値を受け取るときに処理待ち(メインスレッドが停止)となる。
下記の様に返り値をメインスレッドで受け取る様にすると、同期処理と変わらないことになり、そもそもスレッド処理した意味が無くなる。

  ```C#
  using System.Threading;         // Thread.Sleep()に必要
  using System.Threading.Tasks;   // Taskクラスに必要
  void MainThread() {
      var task0 = Task.Run(() => { Thread.Sleep(5000); });                          // 即実行。返り値なし。
      var task1 = Task<string>.Run(() => { Thread.Sleep(3000); return "終了1"; });  // 即実行。返り値あり。
      var task2 = new Task<string>(() => { Thread.Sleep(5000); return "終了2"; });  // 後から実行。返り値あり。
      task2.Start();

      // 返り値を取得。終わるまで処理が待機される
      var result1 = task1.Result;
      var result2 = task2.Result;
  }
  ```

### 別スレッドの処理後にメインスレッドを呼び出す

非同期処理を無駄なくするためには、処理終了を待つのではなく、終了段階で返り値を処理する関数を呼び出すのが正しい。
他スレッドとの競合を防ぐ（スレッドセーフという）ため、WindowsFormのUIへの変更処理は、メインスレッド以外では禁止されている。
ワーカースレッドからメインスレッドを呼び出すには、Invoke()関数を使って呼び出す。<br>
Task の返り値は使わないので、Task<>の定義は必要なくなる。

  ```C#
  void MainThread() {
      Task task = Task.Run(() => {
          Thread.Sleep(5000);
          Invoke(new DelegateInvokeFunc(InvokeFunc), "終了");
      });
  }
  ```

Invoke()関数はデリゲートでメインスレッドを呼び出す為、呼び出す関数の型(delegate)を定義する。<br>

  ```C#
  delegate  void DelegateInvokeFunc(string result);

  // 呼び出されるメインスレッドの処理を作成
  void InvokeFunc(string result) {
      // メインスレッドの処理
  }
  ```

Invoke()関数の呼び出しを delegate型 にキャストすることでラムダ式で書ける。<br>
当たり前だが、EventHandler 等の既に定義されている delegate型なら定義の必要はない。<br>
また、返り値も引数もない汎用的な delegate型として MethodInvoker型が定義されている。

  ```C#
  void MainThread() {
      Task task = Task.Run(() => {
          Thread.Sleep(5000);
          Invoke((DelegateInvokeFunc)((string result) => {
              // メインスレッドの処理
          }), "終了" );
      });
  }
  ```

### async, await

async, await は C# 5.0から実装された機能。Task クラスと合わせて使用する。
機能的には await の Task の処理を待って値を取得する。async は await を使用する関数に必ずつける、というコンパイラの約束事と考えればよい。

await は Task<> 型の前につける。すると Task終了後に Task<> の返り値を取得して、その後の処理が実行される。
Task.Result の処理と違いが無いように思えるが、await は処理時点で関数を抜け出す。つまり return の処理が行われる。
ブレークポイントを置いて追跡してみると分かるが、await の後の処理はタスクの終了後にメインスレッドから別関数として呼び出される。
つまり、見た目上では1つの関数に見えるが、await 前と後で2つの関数に分かれている、という事になる。

感覚的に分かりにくいので、１つの関数に、タスク処理とタスク後のメイン処理セットで1つの関数とする、
という形式以外では使わない方が良いかもしれない。

  ```C#
  async void StartTask() {
      var task1 = Task<string>.Run(() => { Thread.Sleep(3000); return "終了1"; });
      var task2 = Task<string>.Run(() => { Thread.Sleep(5000); return "終了2"; });

      var result1 = task1.Result;   // ブロックされる
      var result2 = await task2;    // ブロックされない。この時点で関数を抜ける。

      // メインスレッドの処理
  }

  void MainThread() {
      StartTask();
      // await の時点で抜け出すので、「メインスレッドの処理」の前に実行される。
  }
  ```

### 参考

- http://gomocool.net/gomokulog/?p=762
- https://qiita.com/4_mio_11/items/f9b19c04509328b1e5c1


デリゲート
----------------------------------------------------------------

### 関数の型を入れる変数







ラムダ式
----------------------------------------------------------------

ラムダ式とは「無名の関数」を定義する書き方である。
下記の様に、ラムダ式は関数から「返り値型」と「関数名」を省いて、引数と処理の間に => を入れたような書式となる。
ラムダ式の返り値型は、処理内にある return 文から推測される。


  ```C#
  // 返り値型  関数名  引数                          処理
  // --------  ------- ----------------------------  -----------------------
     string     AddStr  ( string str1, string str2 )  { return str1 + str2; }    // 関数
                        ( string str1, string str2 )=>{ return str1 + str2; }    // ラムダ式
  ```

### 書式の省略




JSONシリアライズ
----------------------------------------------------------------

### 継承クラスをシリアライズ

    ListViewのアイテムに右クリックメニュー
----------------------------------------------------------------
https://qiita.com/kob58im/items/a5cb6cc89ad2e4ebdf27

    Enum 列挙
----------------------------------------------------------------
https://scrapbox.io/DevelopmentTips/%E3%80%90C%23%E3%80%91enum%E3%81%AE%E5%85%A8%E8%A6%81%E7%B4%A0%E3%81%A7%E3%83%AB%E3%83%BC%E3%83%97%E3%81%99%E3%82%8B