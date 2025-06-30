
Используется для ожидания завершения нескольких горутин, которые должны выполняться вместе как одна группа.

```run-go
package main

import (
    "fmt"
    "sync"
    "time"
)

func worker(id int) {
    fmt.Printf("Worker %d starting\n", id)

    time.Sleep(time.Second)
    fmt.Printf("Worker %d done\n", id)
}

func main() {

    var wg sync.WaitGroup

    for i := 1; i <= 5; i++ {
	    // инкрементируем счетчик горутин в группе (если в одной паника, то программа не зависнет)
        wg.Add(1)

        go func() {
	        // горутина начала выполняться -> уменьшаем счетчик (т.к. может случится паника и wait будет ждать бесконечно)
            defer wg.Done()
            worker(i)
        }()
    }

    wg.Wait()

}
```

`defer wg.Done()` не передаем внутрь функции, чтобы абстрагировать логику, но если надо передать WaitGroup, то надо использовать указатель как `*wg`.

### Практика: сетевые запросы с WaitGroup

```run-go
package main

import (
    "fmt"
    "io/ioutil"
    "net/http"
    "sync"
)

type Result struct {
    url    string
    status int
    err    error
    body   []byte
}

func worker(id int, urls <-chan string, res chan<- Result, wg *(sync.WaitGroup)) {
	defer wg.Done()
	for url := range urls {
		fmt.Println("Worker ", id, " got url: ", url)
		response, err := http.Get(url)
		if err != nil {
			res <- Result{url: url, err: err}
			continue
		}
		// Body - это поток, он читается по необходимости, надо закрывать
		defer response.Body.Close()
		
		body, _ := ioutil.ReadAll(response.Body)
		res <- Result{
			url: url,
			status: response.StatusCode,
			body: body,
		}
	}
}

func main() {
	urls := []string{
        "https://example.com", 
        "https://httpbin.org/get", 
        "https://jsonplaceholder.typicode.com/posts/1", 
        // Можно добавить больше...
    }
    
    const numWorkers = 3
    
    jobs := make(chan string, numWorkers)
    results := make(chan Result, numWorkers)
    
    var wg sync.WaitGroup
    
    for _, url := range urls {
	    jobs <- url
    }
    close(jobs)
    
    wg.Add(numWorkers)
    for i := 1; i <= numWorkers; i++ {
		go worker(i, jobs, results, &wg)
	}
	
	go func() {
		wg.Wait()
		close(results)
	} ()
	
	for res := range results {
		if res.err != nil {
			fmt.Printf("Error fetching %s: %v\n", res.url, res.err)
		} else {
			fmt.Printf("Fetched %s | Status: %d | Body length: %d\n", res.url, res.status, len(res.body))
		}
	}
	
	fmt.Println("All tasks completed.")
}
```
