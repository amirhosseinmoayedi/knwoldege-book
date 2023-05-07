#go #golang #book_summery #web

### We can look at scaling in two ways:
1.  *<font color="#d99694">Vertical</font> scaling*, or increasing the amount of CPUs or capacity in a **single machine**
2.  *<font color="#92cddc">Horizontal</font> scaling*, or **increasing the number of machines** to expand capacity
![[Pasted image 20230314144042.png]]

> Go web applications are **compiled as static binaries**, **without any dynamic dependencies**, and can be **distributed to systems that don’t have Go built in**.

### <font color="#95b3d7">web service</font> vs <font color="#92d050">web application</font>
* **web services** are consumed by other *software*
* *web applications* are used by **humans**.


*The term <font color="#00b050">handler</font>* *is often used for **callback functions triggered by an event***.

## <font color="#ffff00">SSI</font>
**server-side includes (SSI)**, which are directives you can include in an HTML file.

The eventual evolution of SSI was to include more complex code in the HTML and use more powerful interpreters. This pattern grew into highly successful engines for building sophisticated web applications such as <font color="#ffff00">PHP</font>, <font color="#ffff00">ASP</font>, JSP, and ColdFusion.

## <font color="#953734">MVC</font>
**In the MVC pattern the handler is the controller, but also the model.
In an ideal MVC pattern implementation, the controller would be thin, with only routing and HTTP message unpacking and packing logic. The models are fat, containing the application logic and data.

## <font color="#92d050">Template Engine</font>
**A template engine <u>generates the final HTML using templates and data</u>. template engines evolved from an earlier technology, <font color="#fac08f">SSI</font>.

### There are two types of templates with different design philosophies:
1.  **Static templates** or <font color="#fac08f">logic-less</font> templates are HTML interspersed with placeholder tokens. A static template engine will generate the HTML by replacing these tokens with the correct data. There’s little to no logic in the template itself. As you can see, this is similar to the concepts from <font color="#fac08f">SSI</font>.

2.  **Active templates** often contain HTML too, but in addition to placeholder tokens, <font color="#fac08f">they contain other programming language constructs like conditionals</font>, <font color="#fac08f">iterators</font>, and <font color="#fac08f">variables</font>. Examples of active template engines are Java ServerPages (<font color="#fac08f">JSP</font>), Active Server Pages (<font color="#fac08f">ASP</font>), and Embedded Ruby (ERB).

