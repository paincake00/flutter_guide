
Routing относится к тому, как эндпоинты (URIs) реагируют на запросы клиентов.

HTTP-методы доступные в `chi.Router`:
```go
// HTTP-method routing along `pattern`
Connect(pattern string, h http.HandlerFunc)
Delete(pattern string, h http.HandlerFunc)
Get(pattern string, h http.HandlerFunc)
Head(pattern string, h http.HandlerFunc)
Options(pattern string, h http.HandlerFunc)
Patch(pattern string, h http.HandlerFunc)
Post(pattern string, h http.HandlerFunc)
Put(pattern string, h http.HandlerFunc)
Trace(pattern string, h http.HandlerFunc)
```

### Получение named params и wildcards

URL поддерживает именованные параметры (т.е. /users/{userID}) и подстановочные знаки (т.е. /admin/\*).

```go
package main

import (
    "fmt"
    "net/http"

    "github.com/go-chi/chi/v5"
    "github.com/go-chi/chi/v5/middleware"
)

func main() {
    r := chi.NewRouter()

    r.Use(middleware.Logger)

    // кастомное сообщение для 404 статуса
    r.NotFound((func(w http.ResponseWriter, r *http.Request) {
        w.WriteHeader(404)
        w.Write([]byte("not found"))
    }))

    r.Get("/", func(w http.ResponseWriter, r *http.Request) {
        w.Write([]byte("Hello, World!"))
    })

    r.Get("/articles/{date}-{slug}", getArticle)

    http.ListenAndServe(":3000", r)
}

func getArticle(w http.ResponseWriter, r *http.Request) {
    var database = map[string]string{"rrdd": "Article content for rrdd", "ddrr": "Sample article content"}

    dateParam := chi.URLParam(r, "date")
    slugParam := chi.URLParam(r, "slug")
    article, err := database[dateParam+slugParam]

    if !err {
        w.WriteHeader(422)
        w.Write([]byte(fmt.Sprintf("error fetching article %s-%s: %v", dateParam, slugParam, err)))
        return
    }

    if !err {
        w.WriteHeader(404)
        w.Write([]byte("article not found"))
        return
    }
    
	w.Write([]byte(article.Text()))
}
```

### Sub Routers

```go
func main(){
    r := chi.NewRouter()
    r.Get("/", func(w http.ResponseWriter, r *http.Request) {
        w.Write([]byte("Hello World!"))
    })

    // Creating a New Router
    apiRouter := chi.NewRouter()
    apiRouter.Get("/articles/{date}-{slug}", getArticle)

    // Mounting the new Sub Router on the main router
    r.Mount("/api", apiRouter)
}
```

Другой способ:

```go
r.Route("/articles", func(r chi.Router) {
    r.With(paginate).Get("/", listArticles) // GET /articles
    r.With(paginate).Get("/{month}-{day}-{year}", listArticlesByDate) // GET /articles/01-16-2017

    r.Post("/", createArticle) // POST /articles
    r.Get("/search", searchArticles) // GET /articles/search

    // Regexp url parameters:
    r.Get("/{articleSlug:[a-z-]+}", getArticleBySlug) // GET /articles/home-is-toronto

    // Subrouters:
    r.Route("/{articleID}", func(r chi.Router) {
      r.Use(ArticleCtx)
      r.Get("/", getArticle) // GET /articles/123
      r.Put("/", updateArticle) // PUT /articles/123
      r.Delete("/", deleteArticle) // DELETE /articles/123
    })
  })
```

### Routing Groups

Вы можете создавать группы в роутерах, чтобы разделять маршруты, которые используют middleware, от тех, которые не используют.

```go
func main(){
    r := chi.NewRouter()
    
    // Public Routes
    r.Group(func(r chi.Router) {
        r.Get("/", HelloWorld)
        r.Get("/{AssetUrl}", GetAsset)
        r.Get("/manage/url/{path}", FetchAssetDetailsByURL)
        r.Get("/manage/id/{path}", FetchAssetDetailsByID)
    })

    // Private Routes
    // Require Authentication
    r.Group(func(r chi.Router) {
        r.Use(AuthMiddleware)
        r.Post("/manage", CreateAsset)
    })

}
```