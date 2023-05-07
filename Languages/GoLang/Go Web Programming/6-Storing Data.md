#go #golang #web #book_summery 

It’s good practice to use `defer` to **close the file so that you won’t forget**. A `defer` statement **pushes a function call on a stack**. **The list of saved calls is then executed after the surrounding function returns**.

## <font color="#fac08f">In-memory storage</font>
**Gob** is a <font color="#fac08f">binary format that can be saved in a file</font>, providing a quick and effective means of **serializing** in-memory data to one or more files. 
It’s designed for **serialization** and transporting data but it **can also be used for persisting data**. 

**Encoders** and **decoders** wrap around writers and readers, which conveniently allows you to use them to *write* to and *read* from files.
```go
package main

import (
	"bytes"
	"encoding/gob"
	"fmt"
	"io/ioutil"
)

type Post struct {
	Id int
	Content string
	Author string
}

func store(data interface{}, filename string) {
	buffer := new(bytes.Buffer)
	encoder := gob.NewEncoder(buffer)
	err := encoder.Encode(data)
	if err != nil {
		panic(err)
	}
	err = ioutil.WriteFile(filename, buffer.Bytes(), 0600)
	if err != nil {
		panic(err)
	}
}

func load(data interface{}, filename string) {
	raw, err := ioutil.ReadFile(filename)
	if err != nil {
		panic(err)
	}
	buffer := bytes.NewBuffer(raw)
	dec := gob.NewDecoder(buffer)
	err = dec.Decode(data)
	if err != nil {
		panic(err)
	}
}

func main() {
	post := Post{Id: 1, Content: "Hello World!", Author: "Sau Sheong"}
	store(post, "post1")
	var postRead Post
	load(&postRead, "post1")
	fmt.Println(postRead)
}
```

`bytes.Buffer` struct, which is essentially a variable buffer of bytes that has both **Read** and **Write** methods. In other words, a `bytes.Buffer` can be <u>both a Reader and a Writer</u>.

* * *
## Go and <font color="#d99694">SQL</font>
### <font color="#ffc000">openning connection</font>
```go
import (
	"database/sql"
	"fmt"
	_ "github.com/lib/pq"
)

var Db *sql.DB
func init() {
	var err error
	Db, err = sql.Open("postgres", "user=gwp dbname=gwp 	password=gwp
	sslmode=disable")
	if err != nil {
		panic(err)
	}
}
```
The `sql.DB` struct is a handle to the database and represents a <u>pool of zero or database connections that’s maintained by the sql package</u>.

**Open function doesn’t connect to the database or even validate the parameters yet**—it simply sets up the necessary structs for connection to the database later.

**sql.DB doesn’t needed to be closed**, unlike files.

### <font color="#548dd4">importing Driver</font>
Notice that when you import the **database driver**, you set the name of the package to be an underscore `(_)`. **This is because you shouldn’t use the database driver directly**; you should use `database/sql`
only. This way, **if you upgrade the version of the driver or change the implementation of the driver, you don’t need to make changes to all your code**.

```go
func (post *Post) Create() (err error) {
	statement := "insert into posts (content, author) values ($1, $2) returning id "
	stmt, err := db.Prepare(statement)
	if err != nil {
		return
	}
	defer stmt.Close()
	err = stmt.QueryRow(post.Content, post.Author).Scan(&post.Id)
	if err != nil {
		return
	}
	return
}
```

### <font color="#c3d69b">prepare statment</font>
A **prepared** statement is an SQL statement template, where **you can replace certain values during execution**. **Prepared statements are often used to execute statements repeatedly**.
To create it as a prepared statement, let’s use the Prepare method from the `sql.DB` struct: execute the prepared statement using the `QueryRow` method on the statement, passing it the data from the receiver.

You use `QueryRow` here because you want to return only a **single reference** to an `sql.Row` struct. If more than one `sql.Row` is returned by the SQL statement, only the first is returned by QueryRow. **The rest are discarded**.

`QueryRow` is often used with the `Scan` method on the Row struct, which **copies the values in the row into its parameters**.

```go
func GetPost(id int) (post Post, err error) {
	post = Post{}
	err = Db.QueryRow("select id, content, author from posts where id = $1", id).Scan(&post.Id, &post.Content, &post.Author)
	return
}
```

```go
func (post *Post) Update() (err error) {
	_, err = Db.Exec("update posts set content = $2, author = $3 where id = $1", post.Id, post.Content, post.Author)
	return
}
```

*Unlike when creating a post, you won’t use a prepared statement*. Instead, you’ll jump right in with a call to the `Exec` method of the `sql.DB` **struct**. You no longer have to update the receiver, so you **don’t need to scan** the returned results. Therefore, using the Exec method on the global database variable Db, which returns `sql.Result` and an error, is much faster.

```go
func (post *Post) Delete() (err error) {
	_, err = Db.Exec("delete from posts where id = $1", post.Id)
	return
}
```

```go
func Posts(limit int) (posts []Post, err error) {
	rows, err := Db.Query("select id, content, author from posts limit $1", limit)
	if err != nil {
		return
	}
	for rows.Next() {
		post := Post{}
		err = rows.Scan(&post.Id, &post.Content, &post.Author)
		if err != nil {
			return
		}
		posts = append(posts, post)
	}
	rows.Close()
	return
}
```

The Rows interface is an **iterator**. You can call a `Next` method on it repeatedly and it’ll return `sql.Row` until it runs out of rows, when it returns `io.EOF`.

You can see why you’d want to store the relationship in the struct as a **pointer**.
>you don’t really want another copy of the same Post—you want to point to the same Post.

We can create the realtions manually like one to many can be created like this: 
```go
type Post struct {
	Id int
	Content string
	Author string
	Comments []Comment
}
type Comment struct {
	Id int
	Content string
	Author string
	Post *Post
}
```
