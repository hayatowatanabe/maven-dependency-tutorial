# maven-dependency-tutorial

## ph1. 依存関係の解析

以下のコマンドでそのプロジェクトが持つ依存関係をtree表示出来る

    mvn dependency:tree

- サンプルプロジェクトではデフォルトでjunitが設定されている
- maven central repositoryは明示的なリポジトリ定義不要
    - maven central repository: http://repo.maven.apache.org/maven2/
- 実際にWeb上にあるリポジトリの中も確認してみる
    - http://repo.maven.apache.org/maven2/junit/junit/3.8.1/
