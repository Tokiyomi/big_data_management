# Designing Intensive Data Applications - 4
By toki
## Table of contents
- [Chapter 4: Encoding and Evolution](#chapter-4-encoding-and-evolution)
	- [Data Serialization](#data-serialization)
	- [Human-readable data interchange formats](#human-readable-data-interchange-formats)
	- [Binary Data Interchange Formats](#binary-data-interchange-formats)
  - [Data Flow](#data-flow)
- [References](#references)

## Chapter 4: Encoding and Evolution
### Data Serialization

#### 1. How do would you manage over time evolution applications?
> Applications evolve over time.
>
> When the application is server-side, you perform a rolling upgrade, taking down a few nodes in your deployment at a time for upgrades.
>
> When the application is client-side, updating is at the mercy of the user.
>
> To make this easier, on the data layer, you may have either or both of backwards compatibility and forwards compatibility. The latter is far trickier.

#### 2. How do we consider data in memory?
> In memory we consider data in terms of the data structures they live in.
> 
> On disc we work with memory which has been encoded into a sequence of bytes somehow.
>
> With a handful of exceptions for memory maps or memory-mapped files, which are direct representations of on-disc packing in in-memory data structure terms.

#### 3. What is serialization? 
> The process of transliteration between these two formats is known as serialization, encoding, or marshalling. A specific format is a data serialization format.
>
> Most languages include language-specific formats, like pickle, which can be used to store language object.
>
> These formats are easy to use when working with a specific language, but how serializability boundaries and are not easily compatible with other languages.
>
> Still, if your data always stays inside of your application boundary, these formats are fine.

### Human-readable data interchange formats

#### 4. Which are common data interchange formats?
> JSON and XML (and CSV, and the other usual suspects) are common data interchange formats, meant to be moved between application boundaries.
>
> These formats are considered to be lowest common denominators

#### 5. What are their parsing problems?
> They have parsing problems. For example, it's often impossible to difficult to determine the type of an object.
>
> Being human-readable, they are also inefficient in resource terms when performing network transfers.
>
> Still, for simple use cases these formats are usually sufficient.

### Binary data interchange formats

#### 6. Mention two examples of binary data serialization stated  in the book
> The two examples the book uses are Google Protobufs and Apache Thrift (both of which are still in good use in the ecosystem today).

#### 7. How this binary serialization deals with APIs?
> These APIs are naturally verbose and do not match very well against common language patterns, becuase they are machine-written and not human-written, but they work well enough.
>
> These advanced binary data interchange formats are especially neat in that they provide forward and backwards compatibility built-in.
>
> In the context of data interchange formats this is known as schema evolution.

#### 8. What is the approaching of Apache Thrift and Google Protocol Buffers when talking about data interchange?
> Fields in Protobuf (and in Thrift) are identified by field IDs.
>
> These field IDs can be omitted if the field is not required, in which case they will simply be removed from the encoded byte sequence.
>
> But once assigned, they cannot be changed, as doing so would change the meaning of past data encodings.
>
> This provides backwards compatibility. There is one catch however. You cannot mark new fields required, for the obvious reason that doing so would cause all old encodings to fail.
> How about forward compatibility? Every tag is provided a type annotation. When an old reader reads new data, and finds a type it doesn't know about, it skips it, using the type annotation to determine how many bytes to skip. Easy!

#### 9. What about Apache Avro?
> Aother perspective, used by Apache Avro, is that these formats must be resiliant to differences between the reader schema and the writer schema. The challenge is to have a reader that understands every possible version of the writer.
>
> Avro requires you provide version information on read, which the other two formats do not require. This is additional overhead, as you must either encode that information in the file or provide it through some other means (the former is better if the file is big, and the latter if the file(s) are small).
>
> This allows Avro to omit data tags. This in turn makes Avro much easier to use with a dynamic schema, e.g. one that is changing all the time.
>
> This use case is what motivated Avro in the first case. And this is a tradeoff! Avro is more dynamic, Buffers and Thrift are more static but less work.

#### 10. What do these 3 formats have in common?
> All three are equipped with interface description languages. These allow you to perform code generation and get a machine-written API for your data
>
> However, code generation is mainly useful for statically typed languages, which benefit from explicit type checking. Dynamically typed languages, like Python, do not get much benefit.

### Data Flow

#### 11. What is Data Flow?
> Data flow is how data flows through your system. It involves thinking about data usage patterns, application boundaries, and similar such things.

#### 12. What are its types?
> Databases
>
> Service Communication
>
> Message Bus Pattern

#### 13. Describe Databases Data Flow
> Both backwards and forward compatibility matters in a database. Backwards compatibility is important because a database is fundamentally messages to your future self. Forward compatibility matters because when you perform rolling updates, your database will be accessed concurrently by both newer and older versions of software.
>
> When you perform a database migration, the format of the underlying data is actually left unchanged, in those cases where no new information is stored (e.g. adding a new column full of null values). Only when you touch the newly created columns will the database figure out where to store the data so that it has space to include the additional information.
>
> This is in recognition that the data that gets stored typically outlives the code that stores it, and your database may have values in it that have not been touched in five years or more! Moving all of that at once is expensive, so databases migrate lazily when they can.
>
> An approach that deals data principally through databases is using what is known as an integration database. Heavy-on-database data flow is an architectural pattern commonly associated with monolithic architecture.

#### 14. Describe Service Communication
> In service communication you have services that talk to one another using some kind of agreed-upon interchange format. The web is a great example.
>
> Often these services are organized in terms of clients and servers, with clients talking to the servers on behalf of end users.
>
> API calls over a network of some kind have a long lineage.
>
> On the web, this is where REST and SOAP live.
>
> This is where service-oriented architecture and microservices matter (the latter is a more recently coined and more specific subset of the former).
>
> REST is a design philosophy that opines on how well-designed services built over HTTP and HTTPS should look like. SOAP, by contrast, is an XML-based and technically HTTP-independent (but usually HTTP-using) design philosophy. The two compete for mindshare.
>
> REST is winning over SOAP, at least in part due to the decline of XML.

### 15. Describe Message Bus Pattern?
> Your applications can communicate with one another using streams of messages that get passed through a message broker.
>
> Example message brokers are ZeroMQ, Apache Kafka, and Google PubSub.
>
> When using this pattern, your application parts no longer need to be aware of one another. Message emitters can emit messages without caring about who's listening, and message listeners can consume messages without being aware of who's emitting them.
>
> This pattern is good for separation of concerns. The trade-off is that there is one more service you have to maintain. Message brokers are not a lightweight solution overhead-wise.

## References

Kleppmann, M. (2017). Designing Data Intensive Applications: Part 1. Foundations of Data Systems: Encoding and Evolution. O'Reilly, 1st Edition. Pp. 111-139
