#general #api #rest #json

# <font color="#ffc000">JSON</font> (JavaScript Object Notation)

JSON is a <font color="#e36c09">lightweight data interchange format</font> that is easy for humans to read and write and easy for machines to parse and generate. It is based on a subset of the JavaScript programming language, but it is <font color="#e36c09">language-independent</font> and can be used with any programming language.

## Syntax

JSON is a text format that uses a simple syntax to represent data as a collection of **key-value** pairs, arrays, and values. The syntax rules for JSON are as follows:

-   Data is represented in **name/value pairs**.
-   Data is separated by **commas**.
-   Curly braces `{ }` hold objects.
-   Square brackets `[ ]` hold arrays.
-   <u>Objects and arrays can be nested</u>.

Here's an example of a JSON object:

```json
{
    "name": "John Smith",
    "age": 30,
    "city": "New York",
    "email": "john.smith@example.com",
    "hobbies": ["reading", "traveling", "hiking"],
    "isMarried": false,
    "spouse": null
}
```

In this example, the object has several key-value pairs, including a string value for "name", a number value for "age", a boolean value for "isMarried", and a null value for "spouse". The "hobbies" key has an array value that contains three strings.

## Summary

JSON is a simple and widely-used format for representing data as key-value pairs, arrays, and values. Its syntax is easy to read and write, making it popular for exchanging data between web services and applications.