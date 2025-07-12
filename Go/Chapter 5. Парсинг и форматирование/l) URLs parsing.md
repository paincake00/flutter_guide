
#### [URLs are the Uniform Way to Locate Resources](https://adam.herokuapp.com/past/2010/3/30/urls_are_the_uniform_way_to_locate_resources/)

```run-go
package main

import (
	"fmt"
	"net"
	"net/url"
)

func main() {
	s := "postgres://user:pass@host.com:5432/path?k=v#f"
	
	u, err := url.Parse(s)
	if err != nil {
		panic(err)
	}
	
	fmt.Println(u.Scheme)
	
	fmt.Println(u.User)
	fmt.Println(u.User.Username())
	p, _ := u.User.Password()
	fmt.Println(p)
	
	fmt.Println(u.Host) // тут сам хост и порт вместе
	// для разделения стоит использовать это:
	host, port, _ := net.SplitHostPort(u.Host)
	fmt.Println(host)
	fmt.Println(port)
	
	fmt.Println(u.Path)
	fmt.Println(u.Fragment)
	
	fmt.Println(u.RawQuery)
	m, _ := url.ParseQuery(u.RawQuery)
	fmt.Println(m)
	fmt.Println(m["k"][0])
}
```
