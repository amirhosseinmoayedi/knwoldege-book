#DDD #go #golang #book_summery  #architecture 

## <font color="#92cddc">Domain</font>
The domain **is the central entity in DDD**.
<u>it is what we will model our entire language and system around</u>. 
Deciding on domains is a challenging problem and not always as obvious as in our example.

## <font color="#548dd4">Sub-Domains</font>
**<font color="#548dd4">Domains</font> and <font color="#548dd4">sub-domains</font> can be used almost interchangeably**. We **tend to use a sub-domain to signal that the domain we are talking about is a child of a higher-level domain**. In our example, we know that our payment and subscription domains are sub-domains of a much larger business domain. Therefore, we may refer to them as sub-domains, <u>depending on the context of our conversation</u>.

## <font color="#d99694">Ubiquitous language</font>
Ubiquitous language **is the overlap of the language that domain experts and technical experts use**.
![[Figure_2.1_B19042.jpg]]
<u>This language should be used when discussing requirements and system design and should even be used in the source code itself</u>. 
Plus, it should <font color="#ffff00">evolve</font>; therefore, you should spend time evaluating and updating it regularly.

There are **no shortcuts to building a robust**, ubiquitous language; it takes time. <u>Spending lots of time with domain experts is the best way to ensure you capture all important languages</u>.

It can be tempting to try to<font color="#ffff00"> apply a ubiquitous language across multiple projects</font>, teams, and even across an entire company. However, <font color="#ffff00">if you do this, you are setting yourself up for failure</font>.
> Evans advises that ubiquitous language should only apply to a single <font color="#e36c09">bounded context </font>

## <font color="#938953">Bounded Contexts</font>
Bounded contexts are all about **dividing large models into smaller, easier-to-understand chunks** and being explicit about how they relate to each other, <u>Another way to think of them is a boundary</u>.


we often apply patterns to ensure our models can **maintain integrity**. because  as <u>systems evolve and gain complexity</u>, defining boundaries makes more and more sense. The three main patterns are as follows:
1. Open Host Service
2. Published language
3. Anti-corruption layer


### <font color="#92d050">Open Host Service</font>
An Open Host Service is a means of <font color="#92d050">giving other systems (or sub-systems) access to ours</font>, its implementation depends on your team’s skill sets and other constraints.
Typically, an Open Host Service is an **RPC**. Some choices for RPCs might be to build a RESTful API, implement gRPC, or perhaps even an XML API.
![[Pasted image 20230226133332.png]]

### <font color="#ffc000">Published language</font>
A <u>ubiquitous language</u> is our team’s internal formally defined language,  <u>a published language is the opposite</u>.  If our team is going to expose some of our systems to other teams via an Open Host Service, **we need to ensure the definition of what we expose to other teams in different bounded contexts is clear**.
Two popular ways to present published language are via <font color="#00b050">OpenAPI</font> or <font color="#00b050">gRPC</font>.

### <font color="#0070c0">Anti-corruption layer</font>
Sometimes called an <u>adapter layer</u>, an anti-corruption layer **can be used to translate models from different systems**.It is a complementary pattern that works well with the Open Host Service.

It’s worth noting that in more complex systems, anti-corruption layers could be entire services. This can be useful when you intend to migrate from an old system to a new system in multiple stages. However, it adds another point of latency and failure.