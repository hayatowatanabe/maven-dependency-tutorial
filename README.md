# maven-dependency-tutorial

Mavenの依存関係をいろいろいじくる

- Maven: 3.x

## もくじ

- [ph0. プロジェクト作成](https://github.com/koizuss/maven-dependency-tutorial/tree/ph0)
- [ph1. 依存関係の解析](https://github.com/koizuss/maven-dependency-tutorial/tree/ph1)
- [ph2. 依存関係の追加](https://github.com/koizuss/maven-dependency-tutorial/tree/ph2)
- [ph3. ローカルリポジトリへインストール](https://github.com/koizuss/maven-dependency-tutorial/tree/ph3)
- [ph4. ローカルリポジトリを使って依存関係を解決](https://github.com/koizuss/maven-dependency-tutorial/tree/ph4)
- [ph5. 依存関係のスコープ](https://github.com/koizuss/maven-dependency-tutorial/tree/ph5)
- [ph6. 依存関係の重複](https://github.com/koizuss/maven-dependency-tutorial/tree/ph6)
- [ph7. 依存関係の排除](https://github.com/koizuss/maven-dependency-tutorial/tree/ph7)

## ph0. プロジェクト作成

依存関係のサンプルなので普通のJavaプロジェクトを作成

    mvn archetype:generate

- groupId: tutorial.maven.dependency
- artifactId: maven-dependency-tutorial
- version: 1.0-SNAPSHOT

## ph1. 依存関係の解析

以下のコマンドでそのプロジェクトが持つ依存関係をtree表示出来る

    mvn dependency:tree

- サンプルプロジェクトではデフォルトでjunitが設定されている
- maven central repositoryは明示的なリポジトリ定義不要
    - maven central repository: http://repo.maven.apache.org/maven2/
- 実際にWeb上にあるリポジトリの中も確認してみる
    - http://repo.maven.apache.org/maven2/junit/junit/3.8.1/


## ph2. 依存関係の追加

maven central repository以外のリポジトリにある依存関係を追加

- *s2-framework* を追加してみる

    - リポジトリ

        ```xml
          <repositories>
            <repository>
              <id>maven.seasar.org</id>
              <name>The Seasar Foundation Maven2 Repository</name>
              <url>http://maven.seasar.org/maven2</url>
            </repository>
          </repositories>
        ```
    - 依存関係

        ```xml
          <dependencies>
            ...
            <dependency>
              <groupId>org.seasar.container</groupId>
              <artifactId>s2-framework</artifactId>
              <version>2.4.46</version>
              <scope>compile</scope>
            </dependency>
          </dependencies>
        ```

- 実際にWeb上にあるリポジトリの中も確認してみる
    - http://maven.seasar.org/maven2/org/seasar/container/s2-framework/2.4.46


## ph3. ローカルリポジトリへインストール

インストール

    mvn install

- ローカルリポジトリ
    - ~/.m2/repository/
- ローカルリポジトリに追加されていることを確認

## ph4. ローカルリポジトリを使って依存関係を解決

artifactIdを変更してインストールし、ローカルリポジトリを使って依存関係を解決

- ローカルリポジトリも明示的なリポジトリ定義不要
- *git使ってbranchでプロジェクトを切り替えます*

1. git初期化とbranch切り替え

        git init
        echo target/ > .gitignore
        git add .
        git commit -a -m "first"
        git checkout -b tmp

2. artifactIdを変更してインストール

    - artifactId: maven-dependency-tutorial-tmp
    - gitは適宜コミットしてください

3. branch切り替え

        git checkout master

4. 依存関係を整理

    - 今までの依存関係削除
    - tmpを依存関係に追加

## ph5. 依存関係のスコープ

runtime, compile以外スコープは依存関係には加えられらない

- tmpプロジェクトのs2-frameworkスコープを切り替えて確認
- スコープは依存するかしないかを切り替えてる訳ではない
    - compile等他のプラグインでも使用
    - スコープの詳細
        - *TODO: http://~*

## ph6. 依存関係の重複

依存関係が重複した場合の優先度確認

1. メインプロジェクトと依存先

    s2-frameworkで使われている *commons-logging* をメインプロジェクトで定義してみる

    ```xml
    <dependency>
      <groupId>commons-logging</groupId>
      <artifactId>commons-logging</artifactId>
      <version>1.0</version>
    </dependency>
    ```

    - メインプロジェクトの依存関係を優先
    - version等を変更したりして確認
    - versionは関係ない（？？）

2. 依存先同士（階層違い）

    tmp2プロジェクトを作成し、依存先にあるartifactが重複するように定義してみる

    - 階層が浅い依存関係を優先

3. 依存先同士（同階層）

    tmpプロジェクトでtmp2依存関係を追加し同階層の依存関係が重複する状況を作成

    - 定義順序で依存関係が変化

- 依存関係の優先度は以下で決まる
    1. 階層（浅い方が優先）
    2. 同階層の場合、定義順序
    3. *versionは関係ない*

*TODO: mvn dependency:treeで表示されてないだけなのか確認*

## ph7. 依存関係の排除

不要な依存関係を排除する方法を確認

s2-frameworkのjunit
スコープtestが切られてない...
メインプロジェクトでjunitを定義するれば改善
junit-depを使おうとするとまた出てくる
excludeする

```xml
<dependency>
  <groupId>net.jp.kronos.hq</groupId>
  <artifactId>maven-tutorial-tmp</artifactId>
  <version>1.0-SNAPSHOT</version>
  <exclusions>
    <exclusion>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
    </exclusion>
  </exclusions>
</dependency>
```
