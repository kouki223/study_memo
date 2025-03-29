TypeScriptのメモ

- 学習ソース
    https://mosya.dev/react/dashboard
    https://recursionist.io/learn/languages/typescript/function/callback
    
- Type Scriptの概要
    - TypeScriptとは？
        - プログラミング言語
            - 静的型付け言語
                - JavaScriptに型という概念が追加された言語
    - 型の概念が追加されたJavaScript
        - プログラム全般
            - 安全である事
            - コードの改善、修正がしやすい事
                - が求められる
        - 大規模開発、複雑なアプリケーションなどにおいて特に求められる要素である
            - TypeScriptを言語として使う事が選択されている
        - 型のチェック
            - データの型をチェックする事で間違いを教えてくれる
                - 事前に間違いを防ぐ事が出来る
    - トランスパイル
        - 最終的にJavaScriptに変換されてブラウザ、アプリで表示される
            - TypeScriptは直接、ブラウザやアプリでは表示できない
                - TypeScriptのコードをトランスパイルしてJavaScriptへ変換される
                    - Reactで使うJSXもTypeScriptでサポートされている
    - エディターとの相性
        - データの型情報を事前に渡す事でエディターの補完機能を使う事が出来る
            - 開発効率も向上する
- TypeScriptの仕様
    - 型の指定
        - 型の種類と基本構文：
            - **number**（ナンバー）
                - 数字を扱う型
                    
                    ```tsx
                    const age: number = 20;
                    const score: number = 100;
                    ```
                    
            - **string**（ストリング）
                - 文字列を扱う型
                    - 変数に代入する文字列をダブルクウォーテーションorシングルクウォーテーションで囲む
                    
                    ```tsx
                    const name: string = "mosyabo";
                    ```
                    
            - **boolean**（ブーリアン）
                - true or false のどちらかの値を持つ型
                    - 条件式の評価を行う場合に使われる
                    
                    ```tsx
                    const isOpen: boolean = true;
                    
                    if (isOpen) {
                      console.log("ドアが開いています！");
                    } else {
                      console.log("ドアは閉まっています。");
                    }
                    ```
                    
            - **any**（エニー）
                - 何でもありの型
                    - 便利な型であるばTypeScriptの型チェックが適用されないため極力使わないようにする事が必要
                    
                    ```tsx
                    const message: any = "こんにちは";
                    message = 10;
                    message = true;
                    
                    ```
                    
            - **型のエイリアス**
                - typeというキーワードで既存の型に新しい名前をつける
                    
                    ```tsx
                    type Message = string;
                    
                    // Message型を使って変数を宣言
                    const greeting: Message = "Hello, TypeScript";
                    ```
                    
            - ユニオン
                - またはという意味を持つ型
                    - 複数の型のうちから一つの値を持つこと
                    
                    ```tsx
                    const 変数名: 型1 | 型2 |;
                    ```
                    
                    - 実際の例：
                    
                    ```tsx
                    //この関数ではinputはstring型、number型どちらも取りうる
                    function display(input: string | number) {
                     console.log(input);
                    }
                    //このinputにtrueなどを引数に渡すとエラーになってしまう
                    ```
                    
                - ユニオン型が必要になる場面
                    - 取得するデータが複数の型を持ちうる時
                        - サーバーからユーザ情報を取得する場合
                            - ユーザ情報を取得したい場合
                                - ユーザ側
                                - 管理者側
                                    - 名前
                                    - ID
                    
                    ```tsx
                    type User = {
                      id: number;
                      role: "user";
                      name: string;
                    };
                    
                    type Admin = {
                      id: number;
                      role: "admin";
                      name: string;
                    };
                    
                    // UserOrAdmin 型の変数が User 型または Admin 型のどちらかの値を取ることができる
                    type UserOrAdmin = User | Admin;
                    
                    function getAdmins(user: UserOrAdmin[]) {
                      return user.filter((user) => user.role === "admin");
                    }
                    
                    ```
                    
            - インターセクション(交差型)
                - 複数の型を組み合わせて新しい型を作成する
                    - ＆は型の交差（組み合わせる）を示す
                - インターセクションを使う事でDRYの原則で型の定義を行える
                    - DRY ＝＞　Don't Repeat Yourself
                        - 同じコードを繰り返し記載しない
                            - 再利用可能なコードを記載する
                    
                    ```tsx
                    type 新しい型 = 型1 & 型2;
                    ```
                    
                    - 基本構文
                    
                    ```tsx
                    type User = {
                      id: number;
                      role: "user";
                      name: string;
                    };
                    
                    type AdditionalField = {
                      tel: string;
                      address: string;
                    };
                    
                    type UserWithAdditionalField = User & AdditionalField;
                    ```
                    
            - unknown型
                - anyと同様にどのような型でも許容される型
                    - 型チェックを行わないと演算やメソッドを活用する事ができない
                        - エラー例：
                        
                        ```tsx
                        let a: unknown;
                        
                        a = 3;
                        
                        const b = a + 4; // aはunknown型なので演算に利用できない
                        ```
                        
                - typeof
                    - unknownはtypeofを使い型の指定をする事で演算やメソッドを活用する事ができる
                        - any型は型チェックを行わずに使う事が可能である
                            - unknown型はany型よりも安全な型
                        
                        ```tsx
                        let a: unknown;
                        
                        a = 3;
                        
                        if (typeof a === "number") {
                          const b = a + 4; // この中ではaはnumber型になる
                        }
                        ```
                        
        - タイプガード
            - ある変数の型が決まりきらない場合にとりあえず型をunknownに指定する
                - 指定したunknownの型を条件分岐に応じて決定する方法
                    - typeof 演算子
                        - if文と組み合わせて使う
                            - if文の条件式としてtypeof valueとする
                        - プログラムの実行時に値に応じて型が決まり処理が実行される
                        
                        ```tsx
                        let value: unknown = "こんにちは";
                        
                        if (typeof value === "string") {
                          console.log(value.length); // String型のプロパティにアクセスできる
                        } else if (typeof value === "number") {
                          console.log(value.toFixed(2)); // Number型のメソッドにアクセスできる
                        } else {
                          console.log("文字列でも数値でもありません");
                        }
                        ```
                        
                    - instanceof 演算子
                        - 特定のオブジェクトがクラスのインスタンスであるか判断する演算子
                    - カスタム型ガード関数
                        - typeof演算子を使って渡された引数に応じた処理を行う
                            - typescriptにおいてはisも使う
                                - isは関数がtrueを返す場合にはisで指定された型であるという事
                            
                            ```tsx
                            //isString関数は引数としてvalueを受け取る
                            //引数の型はunknownであり、isString関数がtrueの場合には
                            //valueの型はstringである事を示す
                            function isString(value: unknown): value is string {
                            	//typeof演算子でunknownの型をstringに指定する
                              return typeof value === "string";
                            }
                            
                            function isNumber(value: unknown): value is number {
                              return typeof value === "number";
                            }
                            
                            function printValue(value: unknown) {
                              if (isString(value)) {
                                console.log(`文字列: ${value}`); // value は string 型として扱われる
                              } else if (isNumber(value)) {
                                console.log(`数値: ${value}`); // value は number 型として扱われる
                              } else {
                                console.log("文字列でも数値でもない");
                              }
                            }
                            
                            ```
                            
        - 配列型
            - あらかじめ複数の値を持つことができる型
                - 要素に指定された型の後ろに[]とする事で配列の型を指定できる
                    - 実際の配列は[]で囲む
                    
                    ```tsx
                    // 数値の配列
                    const numbers: number[] = [1, 2, 3, 4, 5];
                    
                    // 文字列の配列
                    const fruits: string[] = ["apple", "banana", "cherry"];
                    
                    const numbers: number[] = [1, 2, 3,];
                    
                    a.push(4); 
                    a.push("5"); // 👈 コンパイルエラー
                    ```
                    
            - 配列にユニオン型を適用する事ができる
                
                ```tsx
                const 変数名: (型1 | 型2 )[];
                
                const mix (number | string)[] = [1, "two", 3, "four"];
                ```
                
            - タプル型
                - 配列の各々の型を指定する事ができる
                    - 要素の型の[]内に,で区切って型を記載する
                    
                    ```tsx
                    const 変数名: [型1, 型2, ...];
                    
                    const user: [string, number] = ["mosyabo", 20];
                    
                    適切な順番、型で操作しないとエラーになる
                    user.push("mosya"); // 👈 コンパイルエラー
                    user.push(30); // 👈 コンパイルエラー
                    ```
                    
                - useStateはタプル型を活用したものである
                    
                    ```tsx
                    const [count, setCount] = useState(0);
                    
                    このコードでuseState(0)を記載すると
                    初期値として0を持つ変数countと変数countを更新するための関数seCountが取得できる
                    [count, setCount]で[変数, その変数を変更するための関数]となる
                    ```
                    
    - 関数に型を定義する
        - 関数に型を適用する
            - 関数の受け取る引数
            - 戻り値
                - この2つに型を定義する
        
        ```tsx
        function 関数名(引数1: 型1, 引数2: 型2, ...): 戻り値の型 {
          // 関数の処理
          return 戻り値;
        }
        ```
        
        - アロー関数（functionが省略されて記載される関数コンポーネント）の型定義
            - Add型というエイリアスを作成する
            - 作成したAdd型をアロー関数のaddコンポーネントへ適用する
        
        ```tsx
        type 型名: (変数名: 引数1の型, 変数名: 引数2の型, ...) => 戻り値の型;
        ```
        
    - オブジェクトや配列などの複雑な型を示す方法
        - オブジェクトにtypeを使って型を示す方法
            - オブジェクトの各プロパティに対してデータの型を示す事が出来る
                - まずは型のエイリアスを作成してオブジェクトの型を指定する
                - その後、適切な値を入れてオブジェクトを定義する
                
                ```tsx
                type オブジェクト名 = {
                  プロパティ名1: 型1;
                  プロパティ名2: 型2;
                  // これを繰り返して、必要なプロパティを定義できます
                };
                
                具体例：
                 type User = {
                  name: string;
                  age: number;
                };
                
                型のエイリアスに応じて値を代入する
                const user: User = {
                  name: "mosyabo",
                  age: 20,
                };
                ```
                
            - オプショナル
                - オブジェクトのプロパティに値がない場合にエラーを起こさないようにする
                
                ```tsx
                type Human = {
                  name: string;
                  age: number;
                  birthday?: Date;
                };
                
                const human: Human = {
                  name: "daigo",
                  age: 30,
                  // birthdayがなくてもOK
                };
                ```
                
            - ＆演算子を使ってType同士を合体させる
                - 型と型同士を＆を使って組み合わせて使う事もできる
                
                ```tsx
                type TextMessage = {
                  text: string;
                };
                
                type ImageMessage = {
                  imageUrl: string;
                  alt: string;
                };
                
                type Message = TextMessage & ImageMessage;
                
                このMessageという型はtxetに加えて、imageUrl,altも持っている
                ```
                
            - Mapped Types
                - オブジェクトのkey側の取りうるプロパティが無数にある場合や変則的な場合に利用する
                
                ```tsx
                type Obj = {
                  [key: string]: any;
                };
                ```
                
                - この時のKeyは全てstring型であるオブジェクト型を定義している
                
                ```tsx
                type Human = {
                  name: string;
                  age: number;
                  birthday: Date;
                };
                
                type MaybeHuman = {
                  [key in keyof Human]?: Human[key];
                };
                ```
                
                - []内で別の型のキーを展開する事も可能
            - ORの表現
                - | を使って表現する
                
                ```tsx
                type Judge = "OK" | "NG";
                ```
                
        - interface
            - どのプロパティがどのようなデータの型を持つのか定義する事が出来る
                - プログラム内でプロパティを追加して記載する事ができる
                
                ```tsx
                interface User {
                  name: string;
                };
                
                interface User {
                  age: number;
                };
                
                // TypeScriptがこれらを自動で1つにまとめる
                const member: User = {
                  name: "mosyabo",
                  age: 20
                };
                ****
                ```
                
            - オプショナルの機能
                - Type同様の機能がある
            - Extendsを利用したinterface同士の組み合わせ
                - Typeでの＆演算子を使った合体
            - 上書き
                - のちに書いたinterfaceの型が追加される
            - クラスのinterfaceとして使用する方法
                
                ```tsx
                interface ICar {
                  speed: number;
                  x: number;
                  run(): void;
                }
                
                class Car implements ICar {
                  speed: number;
                  x: number;
                  constructor(speed: number) {
                    this.speed = speed;
                    this.x = 0;
                  }
                  run() {
                    this.x += this.speed;
                  }
                }
                ```
                
        - Typeとinterfaceの違い
            - どちらもオブジェクトの型を指定する役割を持つ
                - Type特有の機能
                    - オプショナル
                    - ＆演算子を使ったTypem同士の合体
                    - MappedTypes
                    - ORの表現（　｜　を使う）
                - Interface特有の機能
                    - そもそもinterfaceはClassとオブジェクトのための機能である
                        - オプショナル
                        - extendsを使ったinterface同士の組み合わせ
                        - 上書き
                        - クラスのinterfaceとして利用する方法
            - Typeにある機能でinterfaceになり機能、その逆も然り
                - チームの方針、コントリビュートしているプロジェクトに応じて使い分け
                    - 普段は type を使い、class の implements の時にだけ interface を使う
    - オブジェクトの任意プロパティ
        - 存在する場合と存在しない場合のどちらにも対応できるようにする
            - 通常、プロパティを定義した場合にはそのプロパティが存在しないとエラーが発生
                - しかし、プロパティが存在しない場合という事は状況に応じてある
                    - プロパティが存在しない場合もエラーが発生しないようにする
        - プロパティの後に？をつける
            
            ```tsx
            interface User {
              name: string;
              age: number;
              hobby?: string; // 👈 オプショナルプロパティ
            };
            
            function getHuman(member: User) {
              // 関数の中身
            }
            
            ```
            
        - オプショナルチェイニング
            
            ```tsx
            type Member = {
              name: string;
              age: number;
              hobby?: {
                getAll: () => string;
              };
            };
            
            console.log(member.hobby?.getAll());
            
            ```
            
        - Null合体演算子
            - デフォルトの値を定義する事ができる
            
            ```tsx
            type Member = {
              name: string;
              age: number;
              hobby?: {
                getAll: () => string;
              };
            };
            const member: Member = {
              name: "mosyabo",
              age: 20,
              // hobby: { getAll: () => "プログラミング" },
            };
            const myHobby = member.hobby?.getAll() ?? "趣味はありません";
            ```
            
    - class
        - オブジェクトの雛形
            - クラスからインスタンスを作成する
            
            ```tsx
            class Greeter {
              greeting: string;
              name: string;
            
              constructor(message: string, name: string) {
                this.greeting = message;
                this.name = name;
              }
            
              greet() {
                return this.greeting + this.name;
              }
            }
            
            let greeter = new Greeter("Hello, ", "Taro");
            console.log(greeter.greet()) // => Hello, Taro 
            ```
            
    - constructor
        - インスタンスの生成時に実行されるメソッド
            - Rubyでのattr_accessorのようなもの
            
            ```tsx
            class User {
                name: string;
                //インスタンス化するとこのcounstructor()が実行されてthis.nameにTaroが入る。
                constructor(name: string){
                    this.name = name;
                }
            }
            
            var taro = new User("Taro");
            console.log(taro.name); //Taroが出力される。
            
            ```
            
    - readOnly
        - 一度設定された値を変更できないようにする（読み取り専用）にするもの
            - プロパティ名の前にreadOnlyを記載する事で読み取り専用になる
        - データを安全に扱う事が出来る
            - **間違いを防止する** :
                - バグの原因となる意図しないデータの変更を防ぐことができます。
            - **コードの意図が明確になる** :
                - このデータは変更されることがないという開発者の意図を伝えることができるので、他の人もコードを理解しやすくなる。
            
            ```tsx
            interface オブジェクト名 {
              readonly プロパティ名: プロパティの型;
            };
            ```
            
            - 読み取り専用にプロパティをする
            
            ```tsx
            class Animal {
              readonly kind: string;
            
              constructor(kind: string) {
                this.kind = kind;
              }
            
              changeKind() {
                this.kind = "cat"; // エラー: 'kind' は読み取り専用プロパティです。
              }
            }
            ```
            
        - 配列にreadOnlyを使う
            - 配列の型の前にreadOnlyを記載する事で適用される
            
            ```tsx
            const numbers: readonly number[] = [1, 2, 3];
            
            number[0] = 5;←エラーになる（読み取り専用）
            ```
            
        - タプル型に使う
            
            ```tsx
            const pair: readonly [number, string] = [1, "apple"];
            pair[0] = 2; // エラー
            ```
            
    - ジェネリクス
        - 型の変数
            - <T>を使う事で様々な種類の型を同じ関数で使う事ができる
                - ＜＞の中に入るものは型パラメーターと呼ばれる
                    - Typeの頭文字でTが取られる事が多い
        - 使い方
            - 関数名の後に<T>を記載する
                - ＜T＞は様々な型に対応する事ができる
                - (引数: T)と指定する事でTとした型の値を受け取る
                - ：Tは返り値の値の型はTである事を示す
            
            ```tsx
            function 関数名<T>(引数: T): T {
              // 関数の中身
            };
            ```
            
        - 実際の使用例
            - 文字列でも数値でも関数が使うことができる
            
            ```tsx
            function findElement<T>(items: T[], target: T): T | undefined {
              return items.find((item) => item === target);
            };
            // 数値の配列から要素を探す
            const numbers = [1, 2, 3, 4, 5];
            const targetNumber = findElement(numbers, 3);
            console.log(targetNumber); // targetNumberの型はnumber | undefined
            
            // 文字列の配列から要素を探す
            const strings = ["apple", "banana", "cherry"];
            const targetString = findElement(strings, "banana");
            console.log(targetString); // targetStringの型はstring | undefined
            ```
            
    - callback関数に関して
        - コールバック関数とは
            - 関数の引数にその関数自身によって呼び出される関数が入ったもの
                - 関数の実行の後で実行される関数
        - シンプルなコード例：
            
            ```tsx
            function add(a: number, b: number, callback: (result: number) => void): void {
                const result = a + b;
                callback(result);
            }
            
            add(1, 2, (result) => {
                console.log(result); // 3
            });
            ```
            
            - この関数がとる引数は3つある
                - 2つの数値（a, b）
                - コールバック関数（callback）
                    - 計算結果を呼び出し元に返す
        - コールバック関数を使う事でUIの更新などもする事ができる
            
            ```tsx
            function processData(data: string[], callback: (result: string) => void): void {
                let result = "";
                for (const item of data) {
                    result += item.toUpperCase() + " ";
                }
                result = result.trimEnd();
                callback(result);
            }
            
            processData(["hello", "world"], (result) => {
                console.log(result); // "HELLO WORLD"
            });
            ```
            
            - 関数processDataは、文字列の配列（data）とコールバック関数（callback）を引数として受け取る
                - コールバック関数は (result: string) => void型
                - string型の引数を1つだけ取る、何も返さない
            - processData関数は、データ配列を繰り返し処理する。
                - 各文字列を大文字に連結して1つの結果文字列を生成
                - そして、コールバック関数を使用して、呼び出し元のコードに結果の文字列を返します。
            - この例では、processData関数のコールバックとして無名関数を渡しています。この無名関数は、1つの引数（result）を受け取り、コンソールにログを出力します。
        - setTimeoutを使った例：
            
            ```tsx
            function delayedGreeting(name: string, callback: (greeting: string) => void): void {
                setTimeout(() => {
                    const greeting = `Hello, ${name}!`;
                    callback(greeting);
                }, 1000);
            }
            
            delayedGreeting("John", (greeting) => {a
                console.log(greeting); // "Hello, John!"
            });
            ```
            
            - 関数delayedGreetingの受け取る引数
                - 文字列の引数（name）
                - コールバック関数（callback）
                    - コールバック関数は、(greeting: string) => void型
                        - string型の引数を一つ取り、何も返さない
            - 関数delayedGreetingは、setTimeoutを使って1秒後にコールバックを実行し、挨拶文を作成してコールバック関数の引数として渡しています。
                - 関数delayedGreetingへのコールバックとして、無名関数を渡しています。この無名関数は一つの引数（greeting）を受け取り、コンソールにログを出力する
            - この関数はsetTimeoutが終了した後にコールバックを使用してメッセージを出力する。これはデータの読み込みやアニメーション、ユーザーとのインタラクションなど、遅延が発生する可能性のある実世界のシナリオを扱う場合によくあるパターンです。
        - 少し複雑な例：
            
            ```tsx
            function getUserData(userId: number, callback: (user: { name: string; age: number }) => void): void {
                const users = [{ id: 1, name: "John", age: 30 }, { id: 2, name: "Jessica", age: 25 }];
                const user = users.find((u) => u.id === userId);
                callback(user);
            }
            
            let user;
            getUserData(1, (data) => {
                user = data;
            });
            
            console.log(user); // { id: 1, name: "John", age: 30 }
            ```
            
            - getUserData関数が受け取る引数
                - ユーザーID（userId）
                - コールバック関数（callback）
                    - コールバック関数は、(user: { name: string; age: number }) => void型
                        - { name: string; age: number }型の引数を1つだけ取る。
                            - 何も返さない
            - getUserData関数は、引数userIdと同じIDを持つユーザーをusers配列から見つけ、コールバック関数を使用してユーザーデータを呼び出し元のコードに返します。
            - また、コールバックを使ってUIを更新することもできる。
                - ユーザー情報を取得するリクエストを行う
                    - アプリに名前や年齢を表示したい場合
                        - データが返ってきた後にコールバックでUI変更を実行すること可能
        - UI更新のコールバック関数
            
            ```tsx
            getUserData(1, (data) => {
                const nameLabel = document.getElementById("name");
                const ageLabel = document.getElementById("age");
                nameLabel.innerText = data.name;
                ageLabel.innerText = data.age;
            });
            ```
            
    - 演習
        - 引数に型を指定する
            - 要件
                - yearToDate関数の引数に適切な型を示す
            - ユーザー体験
                - なし
            - 処理
                - yearToDate関数**は365日を一年として年数から日数を計算する関数**
                    - 引数として受け取るものは年数（数字）
                    - 呼び出し元に返す値は日数（数字）
        - 1番背が高い人を探す関数`getHighestPersON` 関数を作成する
            - 要件
                - `Person`型を定義する
            - ユーザー体験
                - 無し
            - 処理
                - Typeを使ってデータの型を示す
                - 配列の要素を一つずつ検証して1番数値が高いものを探す
                
                ```tsx
                export function getHighestPerson(people: Person[]) {
                  // ここに処理
                  let maxPerson: Person = people[0];
                  for (let i = 0; i < people.length; i++) {
                    if (maxPerson.height < people[i].height) {
                      maxPerson = people[i];
                    }
                  }
                  return maxPerson;
                }
                ```
                
                - 変数maxPersonを定義する
                    - データの型はPersonとする
                - peple[0]
                    - 受け取った配列になっている引数の0（最初の要素）を取得する
                - for分を使う
                    - 変数iを0と定義する
                    - 変数iから引数として受け取ったpepleの引数の要素の数まで
                        - if (maxPerson.height < people[i].height) {maxPerson = people[i];}}を実行する
                            - if文
                                - maxPerson.height < people[i].height
                                - maxPersonのheightプロパティとpeopleに対して変数iで定義したインデックス番号の要素のheigtを比較する
                                    - 条件に一致するのであれば変数maxPersonにpeople[i]を代入する
                            - 最終的にmaxPersonに代入される要素は1番背が高い要素になる
                - maxPerson**を戻す**
        - ユーザープロフィールを更新するWebアプリ開発（年齢の更新）
            - 要件
                - ユーザ情報を登録、更新する
                    - 年齢の入力をする際に
                        - 数値としての年齢
                        - 文字列としての年齢
                    - ２通りの登録方法ができてしまうという不整合がおきる
                        - 年齢を登録する関数normalizeAgeInputは引数valueを持つ
                            - valueの型はunknownとする
                            - valueが文字列であれば文字列を返す
                            - valueが数値であれば文字列に変換して返す
                            - それ以外はエラーとする
            - ユーザー体験
                - 文字列、数値どちらでも年齢の登録が可能になる
            - 処理
                - 用意されているコード
                
                ```tsx
                export function normalizeAgeInput() {}
                
                normalizeAgeInput(23); // -> "23"
                normalizeAgeInput("30"); // -> "30"
                ```
                
                - export function normalizeAgeInput()
                    - 引数
                        - value : unknown
                - {}
                    - if (typeof value === “string”){ return normalizeAgeInput(value); }; else (typeof === “number”){ String(value);  return normalizeAgeInput(value);}
                - 回答したコード
                
                ```tsx
                export function normalizeAgeInput( value : unknown) {
                  if (typeof value === "string") {
                   return normalizeAgeInput(value); }
                  else (typeof value === "number") {
                    return String(value); }
                }
                
                normalizeAgeInput(23); // -> "23"
                normalizeAgeInput("30"); // -> "30"
                
                //回答のコード
                //関数normalizeAgeInputの型も指定する
                //ageはstringとして全て登録するため型はstringで指定する
                export function normalizeAgeInput(value : unknown):string {
                  if (typeof value === "string") {
                   return value; }
                  else if (typeof value === "number") {
                    return String(value); }
                  throw new Error("年齢の形式が不正です。");
                }
                
                normalizeAgeInput(23); // -> "23"
                normalizeAgeInput("30"); // -> "30"
                ```
                
        - タイプガードを使う
            - 要件
                - チャットアプリを作成する
                    - メッセージがテキスト型か画像型か判断する関数を定義する
                    - メッセージの種類に応じて処理を変える
                        - テキストの場合
                        
                        ```tsx
                        <div class="text-message">
                          <p>テキストメッセージ</p>
                        </div>
                        ```
                        
                        - 画像の場合
                        
                        ```tsx
                        <div class="image-message">
                          <img src="https://example.com/image.png" />
                        </div>
                        
                        ```
                        
            - ユーザー体験
                - なし
            - 処理
                - 型の判別をする
                    - 対象となる型はMessage
                        - 内容：
                            
                            ```
                            type TextMessage = {
                              type: "text";
                              text: string;
                            };
                            
                            type ImageMessage = {
                              type: "image";
                              url: string;
                            };
                            ```
                            
                        - textタイプとimageタイプの２通りがある
                            - 2つのタイプを判別するis関数を作成する
                                - 引数としてmessageを受け取る
                                - messageの型はMessageになる
                            - タイプガードを実装する
                            
                            ```tsx
                            // タイプガードを実装してください
                            export function isTextMessage(message: Message): message is text {
                              // この関数の中身を実装してください
                              return typeof message === "text";
                            }
                            
                            // タイプガードを実装してください
                            export function isImageMessage(message: Message): message is image {
                              // この関数の中身を実装してください
                              return typeof message === "image";
                            }
                            ```
                            
                - メッセージの種類の応じて処理を変える
                    - typeof演算子を使ってif文とtypeof演算子で型に応じて処理を修正する
                    
                    ```tsx
                    export function showMessage(message: Message) {
                      // この関数の中身を実装してください
                      if (isTextMessage(message))
                    	  return {
                    	  <div class="text-message">
                    		  <p>テキストメッセージ</p>
                    		</div>
                    		};
                    	else if (isImageMessage(message))
                    		return {
                    		<div class="image-message">
                    		  <img src="https://example.com/image.png" />
                    		</div>
                    		};
                    }
                    
                    console.log(showMessage({ type: "text", text: "hello" }));
                    console.log(
                      showMessage({ type: "image", url: "https://example.com/image.png" })
                    );
                    ```
                    
        - 配列の要素（数値）を全て足して返すsum関数を作成する
            - 要件
                - 与えられた配列（文字列と数値が混ざっている）
                    - const array = [1, "2", 3, "4", 5];
                - 配列の要素（数値）を全て足して返すsum関数を作成する
            - ユーザー体験
                - なし
            - 処理
                - 配列内の要素が文字列と数値が混ざっている
                    - 要素を全て数値にして新しい配列itemsを作成する
                        - **.map(Number)**
                    - 新しく作成した配列itemsを引数に渡す
                        
                        ```
                        export function sum(items) {
                          // ここに実装してください
                          
                        }
                        ```
                        
                    - その数値を全て足して返す
                        
                        ```
                        
                        let sum: number = 0;
                        
                        for (let i = 0; i < numbers.length; i++) {
                          sum += numbers[i];
                        }
                        ```
                        
            - 自分で回答したコード
            
            ```tsx
            const array: [number, string, number, string, number] = [1, "2", 3, "4", 5];
            
            const items = array.map(Number);
            
            export function sum(items) {
              // ここに実装してください
              let sum: number = 0;
              for (let i = 0; i < items.length; i++){
                sum += items[i];
              }
            }
            
            console.log(sum(array));
            ```
            
            - 答えのコード
            
            ```tsx
            const array = [1, "2", 3, "4", 5];
            
            //配列の数値と文字列を全て合計するsum関数を定義する
            //引数として受け取った配列をitemsと定義する
            //itemsの型はstringもしくわnumberどちらも取りうる
            //sum関数の戻り値は数値とする
            export function sum(items: (string | number)[]): number {
            	変数resultを初期値が0として定義する
              let result = 0;
              //for文で変数iを0として定義する
              //変数iが配列の要素の数より小さい場合
              //iに1を加えて処理を実行する
              for (let i = 0; i < items.length; i++) {
            	  //itemをitems[i]としてコピーする
                const item = items[i];
                //if文でtypeof演算子を使いitemが文字列の場合には処理を行いそれ以外がelseの処理を行う
                if (typeof item === "string") {
            	    //変数resultに対してitems[i]がコピーされたitemを引数にとってparseIntを使い
            	    //文字列を推測される(数学的記数法の底)に基づいて数値に変換されてresultに追加される
                  result += parseInt(item);
                } else {
                  result += item;
                }
              }
              return result;
            }
            
            console.log(sum(array));
            
            //文字列を数値に変換してくれる
            //string単体で入れた場合には推測に応じた数学的記数法の底の数値に変換される
            parseInt(string)
            //文字列と指定した数学的記数法の底の数値に変換される
            parseInt(string, radix)
            ```
            
        - 任意プロパティを使ってゲームのキャラクター情報を管理するシステムを作る
            - 要件
                - キャラクター情報を管理する
                    - 用意されている配列
                    
                    ```tsx
                    const characters: Character[] = [
                      {
                        name: "アルド",
                        race: "人間",
                        job: "戦士",
                        level: 1,
                      },
                      {
                        name: "ミヤビ",
                        race: "エルフ",
                        job: "魔法使い",
                        level: 10,
                      },
                      {
                        name: "クラウディア",
                        race: "人間",
                        level: 5,
                      },
                      {
                        name: "ユウ",
                        race: "人間",
                        job: "僧侶",
                        level: 3,
                      },
                    ];
                    ```
                    
            - ユーザー体験
                - なし
            - 処理
                - キャラクター情報を管理する
                    - キャラクターの型、情報を管理する関数の型を定義する
                        - 型の定義
                            - Character型
                                - プロパティ
                                    - 名前・文字列：任意
                                    - 種族・文字列：任意
                                    - 職業・文字列：任意（職業は持っていないキャラクターもいる）
                                    - レベル
                                    
                                    ```tsx
                                    type Character = any;
                                    
                                    プロパティ
                                    name
                                    race
                                    job
                                    level
                                    ```
                                    
                            - Query型
                                - プロパティ
                                    - 名前・文字列：任意
                                    - 種族・文字列：任意
                                    - 職業・文字列：任意
                                    - レベル・数値：任意
                    - 情報を管理する関数を作成する
                        - findCharacter関数
                            - 引数として配列charactersと呼び出し元から受けとる型がQueryである値の変数queryを受け取る
                            - 戻り値の型はCharacter型とする
                        - if文を使って返す値を変更する
                    - 答え
                        
                        ```tsx
                        {
                        //filter関数を使い、配列charactersに対して引数をcharacterと取った時にif文を使って条件分岐を行う
                        return characters.filter((character) => {
                        	//変数queryのnameプロパティと変数characterのnameプロパティが
                        	//変数queryのnameプロパティと一致しない場合にはfalseを返す
                          if (query.name && character.name !== query.name) {
                            return false;
                          }
                          //変数queryのraceプロパティと変数characterのraceプロパティが
                        	//変数queryのraceプロパティと一致しない場合にはfalseを返す
                          if (query.race && character.race !== query.race) {
                            return false;
                          }
                          //変数queryのjobプロパティと変数characterのjobプロパティが
                        	//変数queryのjobプロパティと一致しない場合にはfalseを返す
                          if (query.job && character.job !== query.job) {
                            return false;
                          }
                          //変数queryのlevelプロパティと変数characterのlevelプロパティが
                        	//変数queryのlevelプロパティと一致しない場合にはfalseを返す
                          if (query.level && character.level !== query.level) {
                            return false;
                          }
                          //変数queryと変数characterの値が等しい場合にはtrueを返す
                          return true;
                        });
                        ```
                        
        - 人の名前を扱うPersonクラスを定義する（readOnly）
            - 要件
                - Personクラスを定義する
                    1. Personクラスを定義する
                    2. コンストラクタでPersonクラスのプロパティを引数として受け取る
                    3. 変更不可（読み取り専用に変更する）
                    4. メソッドを作成してフルネームを返す
            - ユーザー体験
                - なし
            - 処理
                - 
- React＋TypeScriptの概要
    - React
        - UIコンポーネントを作成するJavaScriptのフレームワーク
    - TypeScript
        - JavaScriptに型の概念を追加した静的型付け言語
    - Reatに型の概念を使っているものがReact＋TypeScript
- React+TypeScriptの仕様
    - コンポーネントに型を追加する
        - コンポーネントの型を定義する事で
            - どんな情報を受け取るか明確になる
            - どんな情報を返すのか明確になる
        - Propsに型の定義をする
            
            ```tsx
            type Props = {
              name: string;
            };
            ```
            
        - 定義したPropsを受け取るコンポーネントで型の定義をする
            
            ```tsx
            import React from "react";
            type Props = {
              name: string;
            };
            function Hello(props: Props) {
              return <div>Hello, {props.name}</div>;
            };
            ```
            
    - React.ReactNode
        - React要素の型
            - Reactでレンダリングできるあらゆるものを表現できる
        - コンポーネントを入れ子構造にする
            - テキスト
            - 画像
                - など、複数の型が必要になる場合に使われる
                    - 様々なReactの要素を扱う事が必要な場合に必要になる
        - 使用例：
            
            ```tsx
            import React from "react";
            import type { ReactNode } from "react";
            
            // Propsの型定義で、childrenプロパティがReact.ReactNode型であることを宣言
            type Props = {
              children: ReactNode;
            };
            
            // Boxコンポーネントは、どんなReact要素も子要素として受け取れる
            function Box({ children }: Props) {
              return (
                <div className="box">
                  <div className="box-content">{children}</div>
                </div>
              );
            };
            
            // Appコンポーネントでは、Boxの中にテキストを入れている
            function App() {
              return <Box>ここに内容</Box>;
            };
            ```
            
            現状のコードではPropsとしてここに内容という要素しか受け取っていないが{children}として他の要素を受け取る事も可能
            
    - イベントハンドラーに型の指定をする
        - Reactのイベントに型を指定する事でハンドラーから受け取る値を明確にする
            - クリックイベント
                - handleClick関数に型をつける
                    - React.MouseEvent型を使う
                        - MouseEventのインポート
                            - マウスに関連するイベント（クリック、マウスオーバー）
                        
                        ```tsx
                        import React from "react";
                        import type { MouseEvent } from "react";
                        ```
                        
                    - HTMLButtonElement
                        - クリックイベントが発生したHTML要素を渡す
                    - event.targetとevent.currentTargetの違い
                        - event.targetはイベントバブリングの影響を受ける可能性がある
                            - 子要素から親要素へ順に伝わっていく事
                                - buttun内のsapan要素の場合
                                    - buttonを指定したいのにspanが指定されてしまうなどの可能性がある
                        - event.currentTargetはイベントハンドラーが発生した要素を取得する
                            - イベントが発生したHTML要素を取得したい場合にはevent.currentTargetを使うのが安全
            - 入力された要素に型を指定する
                - ChangeEvent型を使う
                    - ChangeEventのインポート
                    
                    ```tsx
                    import React from "react";
                    import type { ChangeEvent } from "react";
                    ```
                    
                - 入力フィールドを指定するためにHTMLInputElementをchangeイベントに渡す
                    
                    ```tsx
                    import React from "react";
                    import type { ChangeEvent } from "react";
                    
                    function App() {
                      // ChangeEvent<HTMLInputElement>型を使って、イベントオブジェクトの型を正確に指定
                      const handleChange = (event: ChangeEvent<HTMLInputElement>) => {
                        console.log(event.target.value); //ここでevent.target.valueの型がstringであることが保証される
                      };
                    
                      return <input onChange={handleChange} />;
                    };
                    ```
                    
                    - event.target.value
                        - event.targetはイベントが発生したHTML要素の事を示す
                        - イベントが発生したvalueプロパティを指定している
                            - input要素のvalueプロパティは標準でstringのため型の指定はなし
    - useStateの型を定義する
        - useStateで扱う変数の値の型をジェネリクス＜T＞を使って定義する
            - useStateには型の定義が必要な場合と不要な場合の２通りがある
                - useStateは初期値を元に変数の型を推論する能力がある
                    - 必要な場合：
                        - 空の配列やnullが初期値となっている場合
                            
                            ```tsx
                            import { useState } from "react";
                            const [users, setUsers] = useState([]);
                            // 👆 初期値が空の配列のため推論が効かない
                            ```
                            
                    - 不要（必要ない）場合：
                        - 単純なuseStateで初期値が設定されている場合
                            
                            ```tsx
                            import { useState } from "react";
                            const [count, setCount] = useState(0);
                            ```
                            
                            ```tsx
                            import { useState } from "react";
                            const [users, setUsers] = useState([
                              { name: "John", age: 20 },
                              { name: "Mike", age: 30 },
                            ]);
                            ```
                            
                - 型の指定方法
                    
                    ```tsx
                    import { useState } from "react";
                    const [users, setUsers] = useState<{
                    	 name: string; age: number 
                     }[]>([]);
                    ```
                    
    - useReducerと型定義
        - useReducer複数の状態管理を1つにしてまとめる事ができるHook
            - 状態（`State`）とアクション（`Action`）の型を定義する事が一般的
        - カウンターアプリの例：
            - 状態の型の定義
                
                ```tsx
                type State = {
                  count: number; //カウンターの値
                };
                ```
                
            - アクションの型定義（Action）
                - actionは通常、型プロパティを持つ
                    - 複数のアクションを管理するためにユニオン型にする事が多い
                
                ```tsx
                type Action =
                  | { type: "INCREMENT" } // カウントを1増やす
                  | { type: "DECREMENT" } // カウントを1減らす
                ```
                
            - レデューサー関数の定義（Reducer）
                - レデューサーから受け取った情報を元に現在の状態とアクションの差分で新しい状態を返す。
                - Reducer関数のactionとtypeに定義した型を適用させる
                
                ```tsx
                type State = {
                  count: number; //カウンターの値
                };
                
                type Action =
                  | { type: "INCREMENT" } // カウントを1増やす
                  | { type: "DECREMENT" } // カウントを1減らす
                
                function counterReducer(state: State, action: Action): State {
                  switch (action.type) {
                    case "INCREMENT":
                      return { count: state.count + 1 };
                    case "DECREMENT":
                      return { count: state.count - 1 };
                    default:
                      throw new Error("未知のアクションタイプ");
                  }
                };
                ```
                
            - useReducerフックの定義
                - action.typeの型を定義しているため"INCREMENT"と"DECREMENT"以外の値はエラーが発生するようになる
                
                ```tsx
                import { useReducer } from "react";
                
                type State = {
                  count: number; //カウンターの値
                };
                
                type Action =
                  | { type: "INCREMENT" } // カウントを1増やす
                  | { type: "DECREMENT" } // カウントを1減らす
                
                function counterReducer(state: State, action: Action): State {
                  switch (action.type) {
                    case "INCREMENT":
                      return { count: state.count + 1 };
                    case "DECREMENT":
                      return { count: state.count - 1 };
                    default:
                      throw new Error("未知のアクションタイプ");
                  }
                };
                
                const initialState: State = { count: 0 };
                
                const [state, dispatch] = useReducer(counterReducer, initialState);
                
                ```
                
    - 演習
        - useRef
            - 要件
                
                **1.** `useSliderRef`という名前のカスタムフックを作成しましょう！
                
                このフックは、スライダーの動きをコントロールするために、DOM要素への直接参照を作成し、管理する役割を持ちます。
                
                **2.** useRef フックを使って、DOM要素を参照してください。
                
                **3.** useRefに適切な型を指定してください。
                
            - ユーザー体験
                - スライダーで画像がスムーズに移動する
            - 処理
                - `useSliderRef`関数を定義する
                    - useRef
                        
                        ```tsx
                        function useSliderRef() {
                          // useRefを使ってこの関数を実装してください
                          const sliderRef = useRef<HTMLInputElement>();
                          return sliderRef;
                        }
                        ```
                        
                    - buttonと紐付ける
                        - 関数コンポーネントなので関数を返すためにreturnが必要
                - bottonタグにrefをつける
                - useRefに型をHTMLInputElementとして定義する
                    
                    ```tsx
                    onClick={() => {
                              const container = sliderRef.current;
                              if (container) {
                                container.scrollLeft -= scrollAmount;
                              }
                    ```
                    
        - useReducer
            - 要件
                - クイズアプリの状態管理部分を作成する
                    - 4 択のクイズを 1 問ずつ表示する
                    - 回答すると、正解か不正解かを表示する
                    - 「次に進む」ボタンを押すと、次のクイズを表示する
                    - すべてのクイズに回答したら、結果を表示する
                        - 上記の条件を満たすようなReducer関数を作成する
            - ユーザー体験
                - 4択のクイズが回答できる
                - 次に進むボタンをクリックする事で次のクイズを表示できる
            - 処理
                - Reducer関数を作成する
- 概念
    - 型という概念
        - 変数の宣言時に宣言する変数が持っているデータの種類を示す方法
            - 型の種類：
                - **number**（ナンバー）
                    - 数字を扱う型
                        
                        ```tsx
                        const age: number = 20;
                        const score: number = 100;
                        ```
                        
                - **string**（ストリング）
                    - 文字列を扱う型
                        - 変数に代入する文字列をダブルクウォーテーションorシングルクウォーテーションで囲む
                        
                        ```tsx
                        const name: string = "mosyabo";
                        ```
                        
                - **boolean**（ブーリアン）
                    - true or false のどちらかの値を持つ型
                        - 条件式の評価を行う場合に使われる
                        
                        ```tsx
                        const isOpen: boolean = true;
                        
                        if (isOpen) {
                          console.log("ドアが開いています！");
                        } else {
                          console.log("ドアは閉まっています。");
                        }
                        ```
                        
                - **any**（エニー）
                    - 何でもありの型
                        - 便利な型であるばTypeScriptの型チェックが適用されないため極力使わないようにする事が必要
                        
                        ```tsx
                        const message: any = "こんにちは";
                        message = 10;
                        message = true;
                        
                        ```
                        
                - **型のエイリアス**
                    - typeというキーワードで既存の型に新しい名前をつける
    - プログラミングにおいてプログラムの重要な概念
        - 安全である事
        - 修正・改善などコードの保守がしやすい事