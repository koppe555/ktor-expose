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
# ktor-expose
