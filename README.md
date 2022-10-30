# ktor_website

## フォルダ構成

### build.gradle.kts
→依存関係が記されているファイル

### main/resources
→設定ファイルが入っているディレクトリ

### main/kotlin
→ソースコードが入っているディレクトリ


使用しているプラグイン
- plugins/Routing.kt
- plugins/Templating.kt


### main/resouces/static
static配下にあるファイルはktorが自動でマッピングしてくれる。

```
fun Application.configureRouting() {

    routing {
        static("/static") {
            resources("files")
        }
    }
}

```


## FreeMaker

JAVA系のテンプレートエンジン。pluginで追加している。

plugins/Templating.kt

```
fun Application.configureTemplating() {
    install(FreeMarker) {
        templateLoader = ClassTemplateLoader(this::class.java.classLoader, "templates")
        outputFormat = HTMLOutputFormat.INSTANCE
    }
    routing {
    }
}
```


## exposed
ORマッパー。JetbrainsがKotlinで作ってる。


## DAO
デザインパターンの一つ。データベースの操作に関する部分をビジネスロジックから切り離す。


kotlin/com/example/DatabaseFactory.kt
```
package com.example.dao

import com.example.models.*
import kotlinx.coroutines.*
import org.jetbrains.exposed.sql.*
import org.jetbrains.exposed.sql.transactions.*
import org.jetbrains.exposed.sql.transactions.experimental.*

object DatabaseFactory {
    fun init() {
        val driverClassName = "org.h2.Driver"
        val jdbcURL = "jdbc:h2:file:./build/db"
        val database = Database.connect(jdbcURL, driverClassName)
    }
}

```

driverClassNameとjbdcURLをハードコーディングしてるが設定ファイルに記述することも可能
https://ktor.io/docs/configurations.html#configuration-file

### Facade
ファサード


```
package com.example.dao

import com.example.models.*

interface DAOFacade {
    suspend fun allArticles(): List<Article>
    suspend fun article(id: Int): Article?
    suspend fun addNewArticle(title: String, body: String): Article?
    suspend fun editArticle(id: Int, title: String, body: String): Boolean
    suspend fun deleteArticle(id: Int): Boolean
}
```


https://geechs-job.com/tips/details/42
