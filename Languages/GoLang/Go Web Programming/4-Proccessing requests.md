#go #golang #web #book_summery 
## <font color="#76923c">Request</font>
The URL field of a *Request* is a representation of the <font color="#4bacc6">URL</font> that’s sent as part of the request line (the first line of the HTTP request). 
The general form is:
`scheme://[userinfo@]host/path[?query][#fragment]`

URLs that don’t start with a slash after the scheme are interpreted as:
`scheme:opaque[?query][#fragment]`
### <font color="#b7dde8">Header</font>
A **header** is a map with keys that are strings and values that are slices of strings.
```go
...
h := r.Header //  r *http.Request
...
```

### <font color="#fac08f">ReadCloser</font>
Both *request* and *response* bodies are represented by the Body field, which is an `io.ReadCloser`  interface.
In turn the Body field consists of a `Reader` interface and a `Closer` interface. 
```go
func body(w http.ResponseWriter, r *http.Request) {
	len := r.ContentLength 
	body := make([]byte, len)
	r.Body.Read(body) // we need to know the lenght of message in order to create array for it 
	fmt.Fprintln(w, string(body))
}
```
Normally you **wouldn’t need to read the raw form of the body**, though, because Go provides methods such as `FormValue` and `FormFile` to extract the values from a POST form.

### <font color="#c3d69b">EncType</font>
The format of the name-value pairs sent through a POST request is specified by the <font color="#92cddc">content type of the HTML form</font>. This is defined using the ***enctype*** attribute
```html
<form action="/process" method="post" enctype="application/x-www-
➥ form-urlencoded">
	<input type="text" name="first_name"/>
	<input type="text" name="last_name"/>
	<input type="submit"/>
</form>
```
The default value for enctype is `application/x-www-form-urlencoded`. Browsers are required to support at least :
* `application/x-www-form-urlencoded`
* `multipart/form-data`.

the result of post if enctype is `x-www-form-urlencoded`:
`first_name=sau%20sheong&last_name=chang`

If you set enctype to `multipart/form-data`, each name-value pair will be converted into a <font color="#e36c09">MIME</font> message part, each with its own content type and content disposition.
Our form data will now look something like this:
```
------WebKitFormBoundaryMPNjKpeO9cLiocMw
Content-Disposition: form-data; name="first_name"
sau sheong
------WebKitFormBoundaryMPNjKpeO9cLiocMw
Content-Disposition: form-data; name="last_name"
chang
------WebKitFormBoundaryMPNjKpeO9cLiocMw--
```

#### <font color="#e36c09">When would you use one or the other</font>?
If you’re sending <font color="#ffff00">simple text data</font>, the U<font color="#ffff00">RL encoded form is better</font>, it’s <font color="#ffff00">simpler and more efficient and less processing is needed</font>.
If you’re sending <font color="#ffff00">large amounts of data</font>, such as uploading files, the <font color="#ffff00">multipart-MIME form is better</font>.

### <font color="#d99694">parsing data</font>
you need to first parse the request using `ParseForm`, and then
access the Form field, parsing a form:
```go
r.ParseForm()
```

>If the form and the URL have the same key, both of them will be placed in a slice, with the <u>form value always prioritized before the URL value</u>.


Instead of using the `ParseForm` method on the `Request` struct and then using the Form field on the request, you have to use the `ParseMultipartForm` method and then use the `MultipartForm` field on the request. 

The `ParseMultipartForm` method also calls the `ParseForm` method<u> when necessary</u>.
You need to tell the ParseMultipartForm method **how much data you want to extract from the multipart form**.
```go
r.ParseMultipartForm(1024)
```

The `FormValue` method lets you access the key-value pairs just
like in the Form field, except that it’s for a specific key and you don’t need to call the `ParseForm` or `ParseMultipartForm` methods beforehand—**the FormValue method does that for you**.

The `PostFormValue` method does the same thing, except that it’s for the PostForm field instead of the Form field. 
```go
fmt.Fprintln(w, r.PostFormValue("hello"))
```
Both the FormValue and `PostFormValue` methods call the `ParseMultipartForm` method for you.

the most common use for `multipart/form-data` is for **uploading files**.
reading multiple file:
```go
func process(w http.ResponseWriter, r *http.Request) {
	r.ParseMultipartForm(1024)
	fileHeader := r.MultipartForm.File["uploaded"][0]
	file, err := fileHeader.Open()
	if err == nil {
		data, err := ioutil.ReadAll(file)
		if err == nil {
			fmt.Fprintln(w, string(data))
		}
	}
}	
```
As with the `FormValue` and `PostFormValue` methods, there’s a shorter way of getting an uploaded file using the `FormFile` method, shown in the following listing. The `FormFile` method returns the first value given the key, so this approach is normally faster if you have only one file to be uploaded.
reading single file:
```go
func process(w http.ResponseWriter, r *http.Request) {
		file, _, err := r.FormFile("uploaded")
		if err == nil {
			data, err := ioutil.ReadAll(file)
			if err == nil {
				fmt.Fprintln(w, string(data))
		}
	}
}
```

Go’s `ParseForm` method only parses forms and so doesn’t accept `application/json`. If you call the ParseForm method, you won’t get any data at all!
* * *
## <font color="#366092">Response</font>
The actual struct backing up `ResponseWriter` is the nonexported struct
http.response. Because it’s ***nonexported***, you can’t use it directly; you can only use it through the ResponseWriter interface.

>both the parameters are passed in by `reference`; it’s just that the method signature takes a `ResponseWriter` <u>that’s an interface to a pointer to a struct, so it looks as if it’s passed in by value</u>.

The `ResponseWriter` interface has three methods:
1.  <font color="#c3d69b">Write</font>
2.  <font color="#c3d69b">WriteHeader</font>
3.  <font color="#c3d69b">Header</font>

### <font color="#92cddc">Write</font>
The Write method takes in an array of bytes, and this <font color="#92cddc">gets written into the body of the HTTP response</font>. <u>If the header doesn’t have a content type by the time Write is called, the first **512** bytes of the data are used to detect the content type</u>.
```go
w.Write([]byte(str))
```
### <font color="#c3d69b">WriteHeader</font>
The **WriteHeader** method’s name is a bit misleading. It doesn’t allow you to write any headers (you use Header for that), <font color="#c3d69b">but it takes an integer that represents the status code of the HTTP response and writes it as the return status code for the HTTP response</font>. 
by default when you call the Write method, **200 OK** will be sent as the response code.
```go
w.WriteHeader(501)
```

example:
```go
...
func jsonExample(w http.ResponseWriter, r *http.Request) {
	w.Header().Set("Content-Type", "application/json")
	post := &Post{
		User: "Sau Sheong",
		Threads: []string{"first", "second", "third"},
	}
	json, _ := json.Marshal(post)
	w.Write(json)
}
...
```
* * *
## <font color="#ffff00">cookie</font>
<u>Every time the client sends an HTTP request to the server, the <font color="#ffff00">cookie</font> is sent along with it</u>.
Cookies are designed to overcome the stateless-ness of HTTP.

generally there are only two classes of cookies:
1. <font color="#95b3d7">session</font> cookies
2. <font color="#95b3d7">persistent</font> cookies


If the **Expires** field isn’t set, then the cookie is a session or *<font color="#95b3d7">temporary cookie</font>*. Session cookies <font color="#95b3d7">are removed from the browser when the browser is closed</font>. Otherwise, the cookie is a persistent cookie that’ll last as long as it isn’t expired or removed.

`Expires` tells us exactly when the cookie will expire, and `MaxAge` tells us how long the cookie should last (in seconds), starting from the time it’s created in the browser.

Cookie has a `String` method that <u>returns a serialized version of the cookie</u> for use in a Set-Cookie response header. 
```go
c := http.Cookie{
	Name: "first_cookie",
	Value: "Go Web Programming",
	HttpOnly: true,
}
w.Header().Set("Set-Cookie", c.String())
```
You use the `Set` method to add the **first** cookie and then the `Add` method to add a **second** cookie.

Go provides a simpler shortcut to setting the cookie: using the `SetCookie` function in the **net/http** library.
```go
c1 := http.Cookie{
	Name: "first_cookie",
	Value: "Go Web Programming",
	HttpOnly: true,
}
http.SetCookie(w, &c1)
```

extract cookie:
```go
c1, err := r.Cookie("first_cookie") // If the cookie doesn’t exist, it’ll throw an error.
cs := r.Cookies()
```
