#go #golang #DDD #architecture #book_summery 

**None of the factories, repositories or services are unique to DDD and are often used in projects not using the DDD approach**.

## <font color="#31859b">factory</font> pattern
object with the <font color="#31859b">primary responsibility of creating other objects</font>.

Factories are a great <font color="#31859b">way to standardize the creation of complex structs</font> and can be <u>useful as your application grows in complexity</u>. Factories also <font color="#31859b">provide encapsulation</font> (that is, hiding the internal details of an object from the caller and only exposing the minimal interface they need). Finally, factories can help ensure<font color="#31859b"> business invariants are enforced at the time of object creation</font>, which <u>can dramatically simplify our domain model</u>.

### <font color="#e36c09">Entity</font> factories
entities have identities and they have a <font color="#e36c09">minimum set of requirements necessary to instantiate them</font>. We should, therefore, ensure we create entities that satisfy this minimum set of requirements when we create them via a factory.

When designing an entity factory function, we need to <u>decide</u> whether we <u>want the factory function to be responsible for generating the identity for our struct</u> or whether we <u>want to pass one as a parameter</u>. Both ways are fine, but I <u>tend to lean toward letting the factory function generate it unless you have a good reason not to</u>.

## <font color="#76923c">repository</font> pattern
Repositories are the parts of our code that <font color="#76923c">contain the logic necessary to access data sources</font>.A data source can be a wide variety of things, such as a file on disk, a spreadsheet, or an AWS S3 bucket, but <font color="#76923c">in most projects, it is a database</font>.
By using a repository layer, <font color="#76923c">you can centralize common data access code and make your system more maintainable by decoupling from a specific database technology</font>.

we can use [[CQRS]] pattern for this.

<u>One mistake we often make with repository layers is to make one struct per database table</u>. This should be avoided; instead, <u>aim to make one struct per aggregate</u>.a repository layer can write to multiple tables.

## <font color="#d99694">services</font>
 A Service <font color="#d99694">defines behaviors that don't really belong to a single Entity in your domain</font>.

### <font color="#ffc000">Domain services</font>
Domain services are <font color="#ffc000">stateless operations within a domain that complete a certain activity</font> . Sometimes, we will come across processes we cannot find a good way to model in an entity or value object; in these cases, it’s a good idea to use a domain service.
* The code you are about to write performs a significant piece of business logic within one domain
* You are transforming one domain object into another
* You are taking the properties of two or more domain objects to calculate a value

### <font color="#ffff00">Application services</font>
<font color="#ffff00">Application services are used to compose other services and repositories</font>, They should not contain domain logic (this belongs in the domain service).

Application services are **usually very thin**. They are used only for coordination, and all the other logic should be pushed down into the layers underneath the application layer.

### <font color="#0070c0">infrastructure service</font>
Used to <u>abstract technical concerns (e.g. MSMQ, email provider, etc).</u>
like sending Email via an email provider throw their [[API]].
