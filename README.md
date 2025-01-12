# gradle-library-sample

## 概要

`gradle`の`source dependencies`機能を使ってGitHubにライブラリを公開してそれを使用するまでの手順

## ライブラリ作成例

以下のgradleで作成した

### gradleのバージョン

```
$ gradle --version

------------------------------------------------------------
Gradle 8.7
------------------------------------------------------------

Build time:   2024-03-22 15:52:46 UTC
Revision:     650af14d7653aa949fce5e886e685efc9cf97c10

Kotlin:       1.9.22
Groovy:       3.0.17
Ant:          Apache Ant(TM) version 1.10.13 compiled on January 4 2023
JVM:          17.0.13 (Ubuntu 17.0.13+11-Ubuntu-2ubuntu122.04)
OS:           Linux 6.8.0-51-generic amd64
```

[!NOTE]
- `gradle init`で作成されるプロジェクト構成がバージョンによって変わりうるので記載している


### 作成方法

```
mkdir gadle-library-sample
cd gradle-library-sample
gradle init
```

#### `gradle init`時の選択内容

```
Select type of build to generate:
  1: Application
  2: Library
  3: Gradle plugin
  4: Basic (build structure only)
Enter selection (default: Application) [1..4] 2

Select implementation language:
  1: Java
  2: Kotlin
  3: Groovy
  4: Scala
  5: C++
  6: Swift
Enter selection (default: Java) [1..6] 2

Enter target Java version (min: 7, default: 21): 17

Project name (default: gradle-library-sample):

Select build script DSL:
  1: Kotlin
  2: Groovy
Enter selection (default: Kotlin) [1..2] 1

Generate build using new APIs and behavior (some features may change in the next minor release)? (default: no) [yes, no]


> Task :init
To learn more about Gradle by exploring our Samples at https://docs.gradle.org/8.7/samples/sample_building_kotlin_libraries.html

BUILD SUCCESSFUL in 37s
1 actionable task: 1 executed
```

## 作成したライブラリを使用したい側で導入する

### `settings.gradle.kts`

```
sourceControl {
    gitRepository(java.net.URI.create("https://github.com/kawagh/gradle-library-sample.git")) {
        producesModule("org.example.kawagh:lib")
    }
}
```

### `build.gradle.kts`

#### gitブランチ指定の場合

ブランチは`main`とする

```
dependencies {
    implementation("org.example.kawagh:lib") {
        version {
            branch = "main"
        }
    }
}
```

[!NOTE]
- `org.example.kawagh` は`lib/build.gradle.kts`に記載された`group`に一致している
- `lib`はサブプロジェクト名。ディレクトリ名の`lib/`に一致している

#### gitタグ指定の場合

タグ名は`exp2`とする

```
dependencies {
    implementation("org.example.kawagh:lib:exp2")
}
```

## 導入後の期待結果

packageが`org.example` なので `org.example.Library().someLibraryMethod()` のように呼び出すことが出来る

## 参考

- https://blog.gradle.org/introducing-source-dependencies
- https://docs.gradle.org/current/javadoc/org/gradle/vcs/SourceControl.html
- https://docs.gradle.org/current/samples/sample_building_kotlin_libraries.html
