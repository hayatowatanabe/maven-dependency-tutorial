# maven-dependency-tutorial

Mavenの依存関係をいろいろいじくる

- Maven: 3.x

## ph0. プロジェクト作成

依存関係のサンプルなので普通のJavaプロジェクトを作成

    mvn archetype:generate

- groupId: tutorial.maven.dependency
- artifactId: sample1
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
    - リポジトリ内のpomとかも確認

## ph3. ローカルリポジトリへインストール

    mvn install

- *mvn install*するとローカルリポジトリに追加される
    - ローカルリポジトリ: ~/.m2/repository/

## ph4. ローカルリポジトリを使って依存関係を解決

artifactIdを変更してインストールし、ローカルリポジトリを使って依存関係を解決

1. simple1をコピー

    ``cp -r sample1/ sample2``

2. sample2: artifactIdを変更してインストール
    - artifactId: sample2

3. sample1: 依存関係を整理

    1. 今までの依存関係削除
    2. sample2を依存関係に追加
    - ローカルリポジトリも明示的なリポジトリ定義不要
