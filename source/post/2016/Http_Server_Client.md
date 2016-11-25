```toml
title = "Http_Server_Client"
slug = "Http_Server_Client"
desc = "Http_Server_Client"
date = "2016-11-26 01:55:32"
update_date = "2016-11-26 01:55:32"
author = ""
thumb = ""
tags = ["tag"]
```
  
Http_client:  
```go  
package main

import (
	"bytes"
	"fmt"
	"io/ioutil"
	"net/url"
	//	"time"
	"crypto/tls"
	"net/http"
)

func main() {

	tr := &http.Transport{
		TLSClientConfig: &tls.Config{InsecureSkipVerify: true},
	}
	client := &http.Client{Transport: tr}

	requestUrl := "https://127.0.0.1:8080/hello"
	postValue := url.Values{
		"order_id": {"My_order_id"},
		"info_id":  {"My_info_id"},
	}
	//	fmt.Println(postValue.Encode())info_id=My_info_id&order_id=My_order_id
	body := bytes.NewBufferString(postValue.Encode())
	resp, err := client.Post(requestUrl, "application/x-www-form-urlencoded", body)
	if err != nil {
		fmt.Println("err:", err)
	}
	defer resp.Body.Close()
	fmt.Println(resp.StatusCode)
	if resp.StatusCode == 200 {
		rs, err := ioutil.ReadAll(resp.Body)
		if err != nil {
			fmt.Println("read err:", err)
		}
		fmt.Println("response:", string(rs))
	}
	 return
}  
```  
** Http_Server:  

```go  
package main

import (
	"fmt"
	"io"
	"io/ioutil"
	"log"
	"net/http"
)

// hello world, the web server
func HelloServer(w http.ResponseWriter, req *http.Request) {
	req.ParseForm()
	//	orderId := req.Form.Get("order_id")
	orderId := req.PostFormValue("order_id")
	infoId := req.PostFormValue("info_id")
	fmt.Println("order_id= ", orderId, " info_id=", infoId)
	rs, err := ioutil.ReadAll(req.Body)
	if err != nil {
		fmt.Println("err:", err)
		return
	}
	fmt.Println("body", string(rs))

	io.WriteString(w, "hello, world!\n")
}

func main() {
	http.HandleFunc("/hello", HelloServer)
	log.Fatal(http.ListenAndServeTLS(":8080", "cert.pem", "key.pem", nil))
	//	log.Fatal(http.ListenAndServe(":8080", nil))
}  
```