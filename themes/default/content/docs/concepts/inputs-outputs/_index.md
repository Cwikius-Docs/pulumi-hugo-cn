---
title_tag: "Inputs & Outputs | Pulumi Concepts"
meta_desc: Resource properties are treated specially in Pulumi, both for purposes of input and output. Learn how to work with inputs and outputs in this guide.
title: "Inputs & outputs"
h1: "Inputs & outputs"
meta_image: /images/docs/meta-images/docs-meta.png
menu:
  concepts:
    identifier: inputs-outputs
    weight: 5
aliases:
    - /docs/reference/inputs-outputs/
    - /docs/intro/concepts/inputs-outputs/
search:
    boost: true
    keywords:
        - output
        - input
---

## Inputs

All resources in Pulumi accept values that describe the way the resource behaves. We call these values *inputs*.

{{< chooser language "javascript,typescript,python,go,csharp,java,yaml" >}}

{{% choosable language javascript %}}

```javascript
const myId = new random.RandomId("mine", {
    byteLength: 8, // byteLength is an input
});
```

{{% /choosable %}}
{{% choosable language typescript %}}

```typescript
const myId = new random.RandomId("mine", {
    byteLength: 8, // byteLength is an input
});
```

{{% /choosable %}}
{{% choosable language python %}}

```python
my_id = random.RandomId("mine",
    byte_length=8 # byte_length is an input
)
```

{{% /choosable %}}
{{% choosable language go %}}

```go

myId, err := random.NewRandomId(ctx, "mine", &random.RandomIdArgs{
	ByteLength: pulumi.Int(8), // ByteLength is an input
})
if err != nil {
	return err
}

```

{{% /choosable %}}
{{% choosable language csharp %}}

```csharp
var myId = new Random.RandomId("mine", new()
{
    ByteLength = 8, // ByteLength is an input
});

```

{{% /choosable %}}
{{% choosable language java %}}

```java
var myId = new RandomId("mine", RandomIdArgs.builder()
    .byteLength(8) // byteLength is an input
    .build());

```

{{% /choosable %}}

{{% choosable language yaml %}}

```yaml
resources:
  myId:
    type: random:randomId
    properties:
      byteLength: 8 # byteLength is an input
```

{{% /choosable %}}
{{< /chooser >}}

_Inputs_ are generally representations of the parameters to the underlying API call of any resource that Pulumi is managing.

The simplest way to create a resource with its required _inputs_ is to use a _plain value_.

{{< chooser language "javascript,typescript,python,go,csharp,java,yaml" >}}

{{% choosable language javascript %}}

```javascript
const key = new tls.PrivateKey("my-private-key", {
    algorithm: "ECDSA", // ECDSA is a plain value
});

```

{{% /choosable %}}
{{% choosable language typescript %}}

```typescript
const key = new tls.PrivateKey("my-private-key", {
    algorithm: "ECDSA", // ECDSA is a plain value
});

```

{{% /choosable %}}
{{% choosable language python %}}

```python
key = tls.PrivateKey("my-private-key",
  algorithm="ECDSA", # ECDSA is a plain value
)

```

{{% /choosable %}}
{{% choosable language go %}}

```go

key, err := tls.NewPrivateKey(ctx, "my-private-key", &tls.PrivateKeyArgs{
	Algorithm:  pulumi.String("ECDSA"), // ECDSA is a plain value
})
if err != nil {
	return err
}


```

{{% /choosable %}}
{{% choosable language csharp %}}

```csharp
var key = new PrivateKey("my-private-key", new PrivateKeyArgs{
    Algorithm = "ECDSA", // ECDSA is a plain value
});
```

{{% /choosable %}}
{{% choosable language java %}}

```java
var key = new PrivateKey("my-private-key", PrivateKeyArgs.builder()
    .algorithm("ECDSA") // ECDSA is a plain value
    .build()
)
```

{{% /choosable %}}

{{% choosable language yaml %}}

```yaml
resources:
  key:
    type: tls:PrivateKey
    properties:
      algorithm: "ECDSA" # ECDSA is a plain value
```

{{% /choosable %}}
{{< /chooser >}}

{{% notes %}}
_Plain value_ in this document is used to describe a standard string, boolean, integer or other typed value or data structure in your language of choice. _Plain value_ is a way of differentiating these language specific values from Pulumi's asynchronous values.

Once an {{< pulumi-output >}} value has returned (generally from an API) to Pulumi, it can be used as a _plain value_. For more information, see [apply](#apply).
{{% /notes %}}

However, in most Pulumi programs, the inputs to a resource will reference values from another resource:

{{< chooser language "javascript,typescript,python,go,csharp,java" >}}

{{% choosable language javascript %}}

```javascript
let password = new random.RandomPassword("password", {
    length: 16,
    special: true,
    overrideSpecial: "!#$%&*()-_=+[]{}<>:?",
});
let example = new aws.rds.Instance("example", {
    instanceClass: "db.t3.micro",
    allocatedStorage: 64,
    engine: "mysql",
    username: "someone",
    password: password.result, // We pass the output from password as an input
});
```

{{% /choosable %}}
{{% choosable language typescript %}}

```typescript
const password = new random.RandomPassword("password", {
    length: 16,
    special: true,
    overrideSpecial: "!#$%&*()-_=+[]{}<>:?",
});
const example = new aws.rds.Instance("example", {
    instanceClass: "db.t3.micro",
    allocatedStorage: 64,
    engine: "mysql",
    username: "someone",
    password: password.result, // We pass the output from password as an input
});
```

{{% /choosable %}}
{{% choosable language python %}}

```python
password = random.RandomPassword(
    "password",
    length=16,
    special=True,
    override_special="!#$%&*()-_=+[]{}<>:?"
)
example = aws.rds.Instance(
    "example",
    instance_class="db.t3.micro",
    allocated_storage=64,
    engine="mysql",
    username="someone",
    password=password.result, # We pass the output from password as an input
)
```

{{% /choosable %}}
{{% choosable language go %}}

```go
password, err := random.NewRandomPassword(ctx, "password", &random.RandomPasswordArgs{
	Length:          pulumi.Int(16),
	Special:         pulumi.Bool(true),
	OverrideSpecial: pulumi.String("!#$%&*()-_=+[]{}<>:?"),
})
if err != nil {
	return err
}
_, err = rds.NewInstance(ctx, "example", &rds.InstanceArgs{
	InstanceClass:    pulumi.String("db.t3.micro"),
	AllocatedStorage: pulumi.Int(64),
	Engine:           pulumi.String("mysql"),
	Username:         pulumi.String("someone"),
	Password:         password.Result, // We pass the output from password as an input
})
if err != nil {
	return err
}
```

{{% /choosable %}}
{{% choosable language csharp %}}

```csharp
var password = new Random.RandomPassword("password", new()
{
    Length = 16,
    Special = true,
    OverrideSpecial = "!#$%&*()-_=+[]{}<>:?",
});

var example = new Aws.Rds.Instance("example", new()
{
    InstanceClass = "db.t3.micro",
    AllocatedStorage = 64,
    Engine = "mysql",
    Username = "someone",
    Password = password.Result, // We pass the output from password as an input
});
```

{{% /choosable %}}
{{% choosable language java %}}

```java
var password = new RandomPassword("password", RandomPasswordArgs.builder()
    .length(16)
    .special(true)
    .overrideSpecial("!#$%&*()-_=+[]{}<>:?")
    .build());

var example = new Instance("example", InstanceArgs.builder()
    .instanceClass("db.t3.micro")
    .allocatedStorage(64)
    .engine("mysql")
    .username("someone")
    .password(password.result()) // We pass the output from password as an input
    .build());
```

{{% /choosable %}}
{{< /chooser >}}

In this case, Pulumi is taking the _output_ from one resource and using it as the _input_ to another resource.

## Outputs

All resources created by Pulumi will have properties which are returned from the cloud provider API. These values are *outputs*.

Outputs are values of type {{< pulumi-output >}}, which behave very much like [promises](https://en.wikipedia.org/wiki/Futures_and_promises) or [monads](https://en.wikipedia.org/wiki/Monad_(functional_programming)). This is necessary because outputs are not fully known until the infrastructure resource has actually completed provisioning, which happens *asynchronously*. Outputs are also how Pulumi tracks dependencies between resources. When an output from one resource has been returned from the cloud provider API, Pulumi can link the two resources together and pass it as the input to another resource.

Pulumi automatically captures dependencies when you pass an output from one resource as an input to another resource. Capturing these dependencies ensures that the physical infrastructure resources are not created or updated until all their dependencies are available and up-to-date.

Because outputs are asynchronous, their actual plain values are not immediately available. If you need to access an output’s plain value—for example, to compute a derived, new value, or because you want to log it—you have these options:

- [Apply](#apply): a callback that receives the plain value, and computes a new output
- [Lifting](#lifting): directly read properties off an output value
- [Interpolation](#outputs-and-strings): concatenate string outputs with other strings directly

## Apply

To access the _plain_ (or returned) value of an output, use {{< pulumi-apply >}}. This method accepts a callback that will be invoked with the plain value, once that value is available.

This example will wait for the value to be returned from the API and print it to stdout

{{< chooser language "javascript,typescript,python,go,csharp,java" >}}

{{% choosable language javascript %}}

```javascript
let myPet = new random.RandomPet("my-pet")
myPet.id.apply(id => console.log(`Hello, {id}!`))
```

{{% /choosable %}}
{{% choosable language typescript %}}

```typescript
const myPet = new random.RandomPet("my-pet")
myPet.id.apply(id => console.log(`Hello, {id}!`))
```

{{% /choosable %}}
{{% choosable language python %}}

```python
my_pet = random.RandomPet("my-pet")
my_pet.id.apply(lambda id: print(f"Hello, {id}!"))
```

{{% /choosable %}}
{{% choosable language go %}}

```go
myPet, err := random.NewRandomPet(ctx, "my-pet", &random.RandomPetArgs{})
if err != nil {
	return err
}

myPet.ID().ApplyT(func(id string) error {
    fmt.Printf("Hello, %s!", id)
	return nil
})
```

{{% /choosable %}}
{{% choosable language csharp %}}

```csharp
var myPet = new Pulumi.Random.RandomPet("my-pet", new(){});
myPet.Id.Apply(id => { Console.WriteLine($"Hello, {id}!"); return id; });
```

{{% /choosable %}}
{{% choosable language java %}}

```java
var myId = new RandomPet("my-pet", RandomPetArgs.builder()
    .build());

myId.id().applyValue(i -> {
    System.out.println("Hello," + i + "!");
    return null;
});
```

{{% /choosable %}}

{{< /chooser >}}

You can use this same process to create new output values to pass as inputs to another resource, for example, the following code creates an HTTPS URL from the DNS name (the plain value) of a virtual machine:

{{< chooser language "javascript,typescript,python,go,csharp,java,yaml" >}}

{{% choosable language javascript %}}

```javascript
let url = virtualmachine.dnsName.apply(dnsName => "https://" + dnsName);
```

{{% /choosable %}}
{{% choosable language typescript %}}

```typescript
let url = virtualmachine.dnsName.apply(dnsName => "https://" + dnsName);
```

{{% /choosable %}}
{{% choosable language python %}}

```python
url = virtual_machine.dns_name.apply(
    lambda dns_name: "https://" + dns_name
)
```

{{% /choosable %}}
{{% choosable language go %}}

```go
url := vpc.DnsName.ApplyT(func(dnsName string) string {
    return "https://" + dnsName
}).(pulumi.StringOutput)
```

{{% /choosable %}}
{{% choosable language csharp %}}

```csharp
var url = virtualmachine.DnsName.Apply(dnsName => "https://" + dnsName);
```

{{% /choosable %}}
{{% choosable language java %}}

```java
var url = virtualmachine.dnsName().applyValue(dnsName -> "https://" + dnsName);
```

{{% /choosable %}}
{{% choosable language yaml %}}

```yaml
variables:
  url: https://${virtualmachine.DnsName}
```

{{% /choosable %}}

{{< /chooser >}}

The result of the call to {{< pulumi-apply >}} is a new Output<T>. So in this example, the url variable is also an {{< pulumi-output >}}. It will wait for the new value to be returned from the callback, and carries the dependencies of the original Output<T>. If the callback itself returns an Output<T>, the dependencies of that output are also kept in the resulting Output<T>.

{{% notes %}}
During some program executions, `apply` doesn’t run. For example, it won’t run during a preview, when resource output values may be unknown. Therefore, you should avoid side-effects within the callbacks. For this reason, you should not allocate new resources inside of your callbacks either, as it could lead to `pulumi preview` being wrong.
{{% /notes %}}

## All { search.keywords="pulumi.all" }

If you have multiple outputs and need to use them together, the `all` function acts like an `apply` over many resources, allowing you to use multiple outputs when creating a new output. `all` waits for all output values to become available and then provides them as _plain values_ to the supplied callback. This function can be used to compute an entirely new output value, such as by adding or concatenating outputs from two different resources together, or by creating a new data structure that uses them. Just like with `apply`, the result of [Output.all](/docs/reference/pkg/python/pulumi#pulumi.Output.all) is itself an Output<T>.

For example, let’s use a server and a database name to create a database connection string:

{{< chooser language "javascript,typescript,python,go,csharp,java,yaml" >}}

{{% choosable language javascript %}}

```javascript
var pulumi = require("@pulumi/pulumi");
// ...
let connectionString = pulumi.all([sqlServer.name, database.name])
    .apply(([server, db]) => `Server=tcp:${server}.database.windows.net;initial catalog=${db}...`);
```

{{% /choosable %}}
{{% choosable language typescript %}}

```typescript
import * as pulumi from "@pulumi/pulumi";
// ...
let connectionString = pulumi.all([sqlServer.name, database.name])
    .apply(([server, db]) => `Server=tcp:${server}.database.windows.net;initial catalog=${db}...`);
```

{{% /choosable %}}
{{% choosable language python %}}

In python, you can pass in unnamed arguments to `Output.all` to create an Output list, for example:

```python
from pulumi import Output
# ...
connection_string = Output.all(sql_server.name, database.name) \
    .apply(lambda args: f"Server=tcp:{args[0]}.database.windows.net;initial catalog={args[1]}...")
```

Or, you can pass in named (keyword) arguments to `Output.all` to create an Output dictionary, for example:

```python
from pulumi import Output
# ...
connection_string = Output.all(server=sql_server.name, db=database.name) \
    .apply(lambda args: f"Server=tcp:{args['server']}.database.windows.net;initial catalog={args['db']}...")
```

{{% /choosable %}}
{{% choosable language go %}}

```go

connectionString := pulumi.All(sqlServer.Name, database.Name).ApplyT(
    func (args []interface{}) pulumi.StringOutput  {
        server := args[0]
        db := args[1]
        return pulumi.Sprintf("Server=tcp:%s.database.windows.net;initial catalog=%s...", server, db)
    },
)
```

{{% notes %}}
**A note on error handling** The function `ApplyT` spawns a Goroutine to await the availability of the implicated dependencies. This function accepts a `T` or `(T, error)` signature; the latter accomodates for error handling. Alternatively, one may use the `ApplyTWithContext` function in which the provided context can be used to reject the output as canceled. Error handling may also be achieved using an `error` `chan`.
{{% /notes %}}
{{% /choosable %}}
{{% choosable language csharp %}}

```csharp
// When all the input values have the same type, Output.All can be used and produces an ImmutableArray.
var connectionString = Output.All(sqlServer.name, database.name)
    .Apply(t => $"Server=tcp:{t[0]}.database.windows.net;initial catalog={t[1]}...");

// For more flexibility, 'Output.Tuple' is used so that each unwrapped value will preserve their distinct type.
var connectionString2 = Output.Tuple(sqlServer.name, database.name)
    .Apply(t => $"Server=tcp:{t.Item1}.database.windows.net;initial catalog={t.Item2}...");

// Or using a more natural Tuple syntax and a statement lambda expression.
var connectionString2 = Output.Tuple(sqlServer.name, database.name).Apply(t =>
{
    var (serverName, databaseName) = t;
    return $"Server=tcp:{serverName}.database.windows.net;initial catalog={databaseName}...";
});
```

{{% /choosable %}}
{{% choosable language java %}}

```java
// When all the input values have the same type, Output.all can be used
var connectionString = Output.all(sqlServer.name(), database.name())
        .applyValue(t -> String.format("Server=tcp:%s.database.windows.net;initial catalog=%s...", t.get(0), t.get(1));

// For more flexibility, 'Output.tuple' is used so that each unwrapped value will preserve their distinct type.
var connectionString2 = Output.tuple(sqlServer.name, database.name)
        .applyValue(t -> String.format("Server=tcp:%s.database.windows.net;initial catalog=%s...", t.t1, t.t2));
```

{{% /choosable %}}
{{% choosable language yaml %}}

```yaml
variables:
  connectionString: Server=tcp:${sqlServer.name}.database.windows.net;initial catalog=${database.name}...
```

{{% /choosable %}}

{{< /chooser >}}

Notice that [Output.all](/docs/reference/pkg/python/pulumi#pulumi.Output.all) works by returning an output that represents the combination of multiple outputs so that, within the callback, the plain values are available inside of a tuple.

## Accessing Nested Properties of an Output by Lifting {#lifting}

While often, Outputs return asynchronous values that wrap primitive types like strings or integers, sometimes an output has an object with deeply nested values. These properties need to be passed to other inputs as well.

For example, to read a domain record from an ACM certificate, you need to access the domain validation options, which returns an array. Because that value is an output, we would normally need to use {{< pulumi-apply >}}:

{{< chooser language "javascript,typescript,python,go,csharp,java" >}}

{{% choosable language javascript %}}

```javascript
let certCertificate = new aws.acm.Certificate("cert", {
    domainName: "example.com",
    validationMethod: "DNS",
});
let certValidation = new aws.route53.Record("cert_validation", {
    records: [
        // Need to pass along a deep subproperty of this Output
        certCertificate.domainValidationOptions.apply(
            domainValidationOptions => domainValidationOptions[0].resourceRecordValue),
    ],
    ...
});
```

{{% /choosable %}}
{{% choosable language typescript %}}

```typescript
let certCertificate = new aws.acm.Certificate("cert", {
    domainName: "example.com",
    validationMethod: "DNS",
});
let certValidation = new aws.route53.Record("cert_validation", {
    records: [
        // Need to pass along a deep subproperty of this Output
        certCertificate.domainValidationOptions.apply(
            domainValidationOptions => domainValidationOptions[0].resourceRecordValue),
    ],
    ...
});
```

{{% /choosable %}}
{{% choosable language python %}}

```python
certificate = aws.acm.Certificate('cert',
    domain_name='example.com',
    validation_method='DNS'
)

record = aws.route53.Record('validation',
    records=[
        # Need to pass along a deep subproperty of this Output
        certificate.domain_validation_options.apply(
            lambda domain_validation_options: domain_validation_options[0]['resourceRecordValue']
        )
    ],
    ...
)
```

{{% /choosable %}}
{{% choosable language go %}}

```go
cert, err := acm.NewCertificate(ctx, "cert", &acm.CertificateArgs{
    DomainName:       pulumi.String("example"),
    ValidationMethod: pulumi.String("DNS"),
})
if err != nil {
    return err
}

record, err := route53.NewRecord(ctx, "validation", &route53.RecordArgs{
    Records: pulumi.StringArray{
        cert.DomainValidationOptions.ApplyT(func(opts []acm.CertificateDomainValidationOption) string {
            return *opts[0].ResourceRecordValue
        }).(pulumi.StringOutput),
    },
    ...
})
if err != nil {
    return err
}
```

{{% /choosable %}}
{{% choosable language csharp %}}

```csharp
var cert = new Certificate("cert", new CertificateArgs
{
    DomainName = "example",
    ValidationMethod = "DNS",
});

var record = new Record("validation", new RecordArgs
{
    Records = {
        cert.DomainValidationOptions.Apply(opts => opts[0].ResourceRecordValue!)
    },
    ...
});
```

{{% /choosable %}}
{{% choosable language java %}}

```java
var cert = new Certificate("cert",
    CertificateArgs.builder()
        .domainName("example")
        .validationMethod("DNS")
        .build());

var record = new Record("validation",
    RecordArgs.builder()
        .records(
            cert.domainValidationOptions()
            .applyValue(opts -> opts.get(0).resourceRecordValue().get())
            .applyValue(String::valueOf)
            .applyValue(List::of))
        .build());
```

{{% /choosable %}}

{{< /chooser >}}

Instead, to make it easier to access simple property and array elements, an {{< pulumi-output >}} lifts the properties of the underlying value, behaving very much like an instance of it. Lift allows you to access properties and elements directly from the {{< pulumi-output >}} itself without needing {{< pulumi-apply >}}. If we return to the above example, we can now simplify it:

{{< chooser language "javascript,typescript,python,go,csharp,java,yaml" >}}

{{% choosable language javascript %}}

```javascript
let certCertificate = new aws.acm.Certificate("cert", {
    domainName: "example.com",
    validationMethod: "DNS",
});
let certValidation = new aws.route53.Record("cert_validation", {
    records: [
        certCertificate.domainValidationOptions[0].resourceRecordValue
    ],
...
```

{{% /choosable %}}
{{% choosable language typescript %}}

```typescript
let certCertificate = new aws.acm.Certificate("cert", {
    domainName: "example.com",
    validationMethod: "DNS",
});
let certValidation = new aws.route53.Record("cert_validation", {
    records: [
        certCertificate.domainValidationOptions[0].resourceRecordValue
    ],
...
```

{{% /choosable %}}
{{% choosable language python %}}

```python
certificate = aws.acm.Certificate('cert',
    domain_name='example.com',
    validation_method='DNS'
)

record = aws.route53.Record('validation',
    records=[
        certificate.domain_validation_options[0].resource_record_value
    ],
...
```

{{% /choosable %}}
{{% choosable language go %}}

```go
cert, err := acm.NewCertificate(ctx, "cert", &acm.CertificateArgs{
    DomainName:       pulumi.String("example"),
    ValidationMethod: pulumi.String("DNS"),
})
if err != nil {
    return err
}

record, err := route53.NewRecord(ctx, "validation", &route53.RecordArgs{
    Records: pulumi.StringArray{
        // Notes:
        // * `Index` looks up an index in an `ArrayOutput` and returns a new `Output`.
        // * Accessor methods like `ResourceRecordValue` lookup properties of a custom struct `Output` and return a new `Output`.
        // * `Elem` dereferences a `PtrOutput` to an `Output`, equivalent to `*`.
        cert.DomainValidationOptions.Index(pulumi.Int(0)).ResourceRecordValue().Elem(),
    },
    ...
})
if err != nil {
    return err
}
```

{{% /choosable %}}
{{% choosable language csharp %}}

```csharp
var cert = new Certificate("cert", new CertificateArgs
{
    DomainName = "example",
    ValidationMethod = "DNS",
});

var record = new Record("validation", new RecordArgs
{
    // Notes:
    // * `GetAt` looks up an index in an `Output<ImmutableArray<T>>` and returns a new `Output<T>`
    // * There are not yet accessor methods for referencing properties like `ResourceRecordValue` on an `Output<T>` directly,
    //   so the `Apply` is still needed for the property access.
    Records = cert.DomainValidationOptions.GetAt(0).Apply(opt => opt.ResourceRecordValue!),
});
```

{{% /choosable %}}
{{% choosable language java %}}

```java
// Lifting is currently not supported in Java.
```

{{% /choosable %}}
{{% choosable language yaml %}}

```yaml
resources:
  cert:
    type: aws:acm:Certificate
    properties:
      domainName: example
      validationMethod: DNS
  record:
    type: aws:route53:Record
    properties:
      records:
        # YAML handles inputs and outputs transparently.
        - ${cert.domainValidationOptions[0].resourceRecordValue}
```

{{% /choosable %}}

{{< /chooser >}}

This approach is easier to read and write and does not lose any important dependency information that is needed to properly create and maintain the stack. This approach doesn’t work in all cases, but when it does, it can be a great help.

In JavaScript and TypeScript, a ‘lifted’ property access on an `Output<T>` that wraps undefined produces another `Output<T>` with the undefined value instead of throwing or producing a ‘faulted’ `Output<T>`. In other words, lifted property accesses behave like the [`?.` (optional chaining operator)](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-7.html#optional-chaining) in JavaScript and TypeScript. This behavior makes it much easier to form a chain of property accesses on an `Output<T>`.

{{< chooser language "javascript,typescript" >}}

{{% choosable language javascript %}}

```javascript
let certValidation = new aws.route53.Record("cert_validation", {
  records: [certCertificate.domainValidationOptions[0].resourceRecordValue],

// instead of

let certValidation = new aws.route53.Record("cert_validation", {
  records: [certCertificate.apply(cc => cc ? cc.domainValidationOptions : undefined)
                           .apply(dvo => dvo ? dvo[0] : undefined)
                           .apply(o => o ? o.resourceRecordValue : undefined)],
```

{{% /choosable %}}

{{% choosable language typescript %}}

```typescript
let certValidation = new aws.route53.Record("cert_validation", {
  records: [certCertificate.domainValidationOptions[0].resourceRecordValue],

// instead of

let certValidation = new aws.route53.Record("cert_validation", {
  records: [certCertificate.apply(cc => cc ? cc.domainValidationOptions : undefined)
                           .apply(dvo => dvo ? dvo[0] : undefined)
                           .apply(o => o ? o.resourceRecordValue : undefined)],
```

{{% /choosable %}}

{{< /chooser >}}

## Working with Outputs and Strings {#outputs-and-strings}

Outputs that return to the engine as strings cannot be used directly in operations such as string concatenation until the output has returned to Pulumi. In these scenarios, you'll need to wait for the value to return using {{< pulumi-apply >}}.

For example, say you want to create a URL from `hostname` and `port` output values. You can do this using `apply` and `all`.

{{< chooser language "javascript,typescript,python,go,csharp,java,yaml" >}}

{{% choosable language javascript %}}

```javascript
let hostname = res.hostname;
let port = res.port;

// Would like to produce a string equivalent to: http://${hostname}:${port}/
let url = pulumi.all([hostname, port]).
    apply(([hostname, port]) => `http://${hostname}:${port}/`);
```

{{% /choosable %}}
{{% choosable language typescript %}}

```typescript
let hostname: Output<string>;
let port: Output<number>;

// Would like to produce a string equivalent to: http://${hostname}:${port}/
let url = pulumi.all([hostname, port]).
    apply(([hostname, port]) => `http://${hostname}:${port}/`);
```

{{% /choosable %}}
{{% choosable language python %}}

```python
hostname: Output[str]
port: Output[int]

# Would like to produce a string equivalent to: http://${hostname}:${port}/
url = Output.all(hostname, port).apply(lambda l: f"http://{l[0]}:{l[1]}/")
```

{{% /choosable %}}
{{% choosable language go %}}

```go
hostname := pulumi.String("localhost").ToStringOutput()
port := pulumi.Int(8080).ToIntOutput()

// Would like to produce a string equivalent to: http://${hostname}:${port}/
url := pulumi.All(hostname, port).ApplyT(func (args []interface{}) string {
    return fmt.Sprintf("http://%s:%d/", args[0], args[1])
})
```

{{% /choosable %}}
{{% choosable language csharp %}}

```csharp
Output<string> hostname = // get some Output
Output<int> port = // get some Output

// Would like to produce a string equivalent to: http://{hostname}:{port}/
var url = Output.Tuple(hostname, port).Apply(t => $"http://{t.Item1}:{t.Item2}/");
```

{{% /choosable %}}
{{% choosable language java %}}

```java
Output<String> hostname = Output.of("localhost"); // get some Output
Output<Integer> port = Output.of(8080); // get some Output

// Would like to produce a string equivalent to: http://{hostname}:{port}/
var url = Output.tuple(hostname, port)
        .applyValue(t -> String.format("http://%s:%s/", t.t1, t.t2));
```

{{% /choosable %}}

{{< /chooser >}}

However, this approach is verbose and unwieldy. To make this common task easier, Pulumi exposes interpolation helpers that allow you to create strings that contain outputs. These interpolation methods wrap [all](#all) and [apply](#apply) with an interface that resembles your language's native string formatting functions.

{{% choosable language javascript %}}

```javascript
// concat takes a list of args and concatenates all of them into a single output:
const url1 = pulumi.concat("http://", hostname, ":", port, "/");
// interpolate takes a JavaScript "template literal" and expands outputs correctly:
const url2 = pulumi.interpolate `http://${hostname}:${port}/`;
```

{{% /choosable %}}
{{% choosable language typescript %}}

```typescript
// concat takes a list of args and concatenates all of them into a single output:
const url1: Output<string> = pulumi.concat("http://", hostname, ":", port, "/");
// interpolate takes a JavaScript "template literal" and expands outputs correctly:
const url2: Output<string> = pulumi.interpolate `http://${hostname}:${port}/`;
```

{{% /choosable %}}
{{% choosable language python %}}

```python
# concat takes a list of args and concatenates all of them into a single output:
url = Output.concat("http://", hostname, ":", port, "/")
# format takes a template string and a list of args or keyword args and formats the string, expanding outputs correctly:
url2 = Output.format("http://{0}:{1}/", hostname, port);
```

{{% /choosable %}}
{{% choosable language go %}}

```go
url := pulumi.Sprintf("http://%s:%d/", hostname, port)
```

{{% /choosable %}}
{{% choosable language csharp %}}

```csharp
// Format takes a FormattableString and expands outputs correctly:
var url = Output.Format($"http://{hostname}:{port}/");
```

{{% /choosable %}}
{{% choosable language java %}}

```java
// Format takes a FormattableString and expands outputs correctly:
var url = Output.format("http://%s:%s/", hostname, port);
```

{{% /choosable %}}
{{% choosable language yaml %}}

```yaml
variables:
  url: https://${hostname}:${port}
```

{{% /choosable %}}

You can use string interpolation to export a stack output, provide a dynamically computed string as a new resource argument, or even for diagnostic purposes.

## Working with Outputs and JSON {#outputs-and-json}

Often in the course of working with web technologies, you encounter JavaScript Object Notation (JSON) which is a popular specification for representing data. In many scenarios, you'll need to embed resource outputs into a JSON string. In these scenarios, you need to first _wait for the returned_ output, _then_ build the JSON string:

{{< chooser language "typescript,python,go,csharp" >}}
{{% choosable language typescript %}}

```typescript
const contentBucket = new aws.s3.Bucket("content-bucket", {
  acl: "private",
  website: {
    indexDocument: "index.html",
    errorDocument: "index.html",
  },
  forceDestroy: true,
});

const originAccessIdentity = new aws.cloudfront.OriginAccessIdentity(
  "cloudfront",
  {
    comment: pulumi.interpolate`OAI-${contentBucket.bucketDomainName}`,
  }
);

// apply method
new aws.s3.BucketPolicy("cloudfront-bucket-policy", {
  bucket: contentBucket.bucket,
  policy: pulumi
    .all([contentBucket.bucket, originAccessIdentity.iamArn])
    .apply(([bucketName, iamArn]) =>
      JSON.stringify({
        Version: "2012-10-17",
        Statement: [
          {
            Sid: "CloudfrontAllow",
            Effect: "Allow",
            Principal: {
              AWS: iamArn,
            },
            Action: "s3:GetObject",
            Resource: `arn:aws:s3:::${bucketName}/*`,
          },
        ],
      })
    ),
});
```

{{% /choosable %}}
{{% choosable language python %}}

```python
bucket = aws.s3.Bucket(
    "content-bucket",
    acl="private",
    website=aws.s3.BucketWebsiteArgs(
        index_document="index.html", error_document="404.html"
    ),
)

origin_access_identity = aws.cloudfront.OriginAccessIdentity(
    "cloudfront",
    comment=pulumi.Output.concat("OAI-", bucket.id),
)

bucket_policy = aws.s3.BucketPolicy(
    "cloudfront-bucket-policy",
    bucket=bucket.bucket,
    policy=pulumi.Output.all(
        cloudfront_iam_arn=origin_access_identity.iam_arn,
        bucket_arn=bucket.arn
    ).apply(
        lambda args: json.dumps(
            {
                "Version": "2012-10-17",
                "Statement": [
                    {
                        "Sid": "CloudfrontAllow",
                        "Effect": "Allow",
                        "Principal": {
                            "AWS": args["cloudfront_iam_arn"],
                        },
                        "Action": "s3:GetObject",
                        "Resource": f"{args['bucket_arn']}/*",
                    }
                ],
            }
        )
    ),
    opts=pulumi.ResourceOptions(parent=bucket)
)
```

{{% /choosable %}}
{{% choosable language go %}}

{{% notes type="info" %}}
The Pulumi Go SDK does not currently support serializing or deserializing maps with unknown values.
It is tracked [here](https://github.com/pulumi/pulumi/issues/12460).

The following is a simplified example of using `pulumi.JSONMarshal` in Go.
{{% /notes %}}

```go
bucket, err := s3.NewBucket(ctx, "content-bucket", &s3.BucketArgs{
	Acl: pulumi.String("private"),
	Website: &s3.BucketWebsiteArgs{
		IndexDocument: pulumi.String("index.html"),
		ErrorDocument: pulumi.String("404.html"),
	},
})
if err != nil {
	return err
}

originAccessIdentity, err := cloudfront.NewOriginAccessIdentity(ctx, "cloudfront", &cloudfront.OriginAccessIdentityArgs{
		Comment: pulumi.Sprintf("OAI-%s", bucket.ID()),
})
if err != nil {
	return err
}

_, err = s3.NewBucketPolicy(ctx, "cloudfront-bucket-policy", &s3.BucketPolicyArgs{
	Bucket: bucket.ID(),
	Policy: pulumi.All(bucket.Arn, originAccessIdentity.IamArn).ApplyT(
		func(args []interface{}) (pulumi.StringOutput, error) {
			bucketArn := args[0].(string)
			iamArn := args[1].(string)

			policy, err := json.Marshal(map[string]interface{}{
				"Version": "2012-10-17",
				"Statement": []map[string]interface{}{
					{
						"Sid":    "CloudfrontAllow",
						"Effect": "Allow",
						"Principal": map[string]interface{}{
							"AWS": iamArn,
						},
						"Action":   "s3:GetObject",
						"Resource": bucketArn + "/*",
					},
				},
			})

			if err != nil {
				return pulumi.StringOutput{}, err
			}
			return pulumi.String(policy).ToStringOutput(), nil
		}).(pulumi.StringOutput),
}, pulumi.Parent(bucket))
if err != nil {
	return err
}
```

{{% /choosable %}}
{{% choosable language csharp %}}

```csharp
var bucket = new Bucket("content-bucket", new BucketArgs
{
    Acl = "private",
    Website = new BucketWebsiteArgs
    {
        IndexDocument = "index.html",
        ErrorDocument = "404.html",
    },
});

var originAccessIdentity = new OriginAccessIdentity("cloudfront", new OriginAccessIdentityArgs
{
    Comment = Output.Format($"OAI-{bucket.Id}"),
});

var bucketPolicy = new BucketPolicy("cloudfront-bucket-policy", new BucketPolicyArgs
{
    Bucket = bucket.Bucket,
    Policy = Output.Tuple(bucket.Arn, originAccessIdentity.IamArn)
    .Apply(t =>
    {
        string bucketArn = t.Item1;
        string cloudfrontIamArn = t.Item2;

        var policy = new
        {
            Version = "2012-10-17",
            Statement = new object[]
            {
                new
                {
                    Sid = "CloudfrontAllow",
                    Effect = "Allow",
                    Principal = new { AWS = cloudfrontIamArn },
                    Action = "s3:GetObject",
                    Resource = $"{bucketArn}/*",
                },
            },
        };

        return JsonSerializer.Serialize(policy);
    }),
}, new CustomResourceOptions { Parent = bucket });
```

{{% /choosable %}}
{{< /chooser >}}

This operation is so common, Pulumi provides first-class helper functions for deserializing JSON string outputs into your language's native objects and serializing your language's native objects to JSON string outputs. These helper functions are designed to remove the process of manually resolving the output inside a {{< pulumi-apply >}}.

### Converting Outputs to JSON

You can natively represent the definition of an AWS Step Function State Machine and embed outputs from other resources then convert it to a JSON string.

{{< chooser language "javascript,typescript,python,go,csharp" >}}
{{% choosable language javascript %}}

```javascript
const stateMachine = new awsnative.stepfunctions.StateMachine("stateMachine", {
    roleArn: sfnRole.arn,
    stateMachineType: "EXPRESS",
    definitionString: pulumi.jsonStringify({
        "Comment": "A Hello World example of the Amazon States Language using two AWS Lambda Functions",
        "StartAt": "Hello",
        "States": {
            "Hello": {
                "Type": "Task",
                "Resource": helloFunction.arn, // Pulumi Resource Output
                "Next": "World",
            },
            "World": {
                "Type": "Task",
                "Resource": worldFunction.arn, // Pulumi Resource Output
                "End": true,
            },
        },
    })
});
```

{{% /choosable %}}
{{% choosable language typescript %}}

```typescript
const stateMachine = new awsnative.stepfunctions.StateMachine("stateMachine", {
    roleArn: sfnRole.arn,
    stateMachineType: "EXPRESS",
    definitionString: pulumi.jsonStringify({
        "Comment": "A Hello World example of the Amazon States Language using two AWS Lambda Functions",
        "StartAt": "Hello",
        "States": {
            "Hello": {
                "Type": "Task",
                "Resource": helloFunction.arn, // Pulumi Resource Output
                "Next": "World",
            },
            "World": {
                "Type": "Task",
                "Resource": worldFunction.arn, // Pulumi Resource Output
                "End": true,
            },
        },
    })
});
```

{{% /choosable %}}
{{% choosable language python %}}

```python
state_machine = aws_native.stepfunctions.StateMachine("stateMachine",
    role_arn=sfn_role.arn,
    state_machine_type="EXPRESS",
    definition_string=pulumi.Output.json_dumps({
        "Comment": "A Hello World example of the Amazon States Language using two AWS Lambda Functions",
        "StartAt": "Hello",
        "States": {
            "Hello": {
                "Type": "Task",
                "Resource": hello_function.arn, # Pulumi Resource Output
                "Next": "World",
            },
            "World": {
                "Type": "Task",
                "Resource": world_function.arn, # Pulumi Resource Output
                "End": True,
            },
        },
    })
)
```

{{% /choosable %}}
{{% choosable language go %}}

{{% notes type="info" %}}
The Pulumi Go SDK does not currently support serializing or deserializing maps with unknown values.
It is tracked [here](https://github.com/pulumi/pulumi/issues/12460).

The following is a simplified example of using `pulumi.JSONMarshal` in Go.
{{% /notes %}}

```go
pulumi.JSONMarshal(pulumi.ToMapOutput(map[string]pulumi.Output{
    "bool": pulumi.ToOutput(true),
    "int":  pulumi.ToOutput(1),
    "str":  pulumi.ToOutput("hello"),
    "arr": pulumi.ToArrayOutput([]pulumi.Output{
        pulumi.ToOutput(false),
        pulumi.ToOutput(1.0),
        pulumi.ToOutput(""),
        pulumi.ToMapOutput(map[string]pulumi.Output{
            "key": pulumi.ToOutput("value"),
        }),
    }),
    "map": pulumi.ToMapOutput(map[string]pulumi.Output{
        "key": pulumi.ToOutput("value"),
    }),
    // The following functionality is currently unsupported as myResource
    // is an unknown value.
    "unknown": myResource.ApplyT(func(res interface{}) (interface{}, error) {
        return "Hello World!", nil
    }),
}))
```

{{% /choosable %}}
{{% choosable language csharp %}}

```csharp
var stateMachine = Pulumi.Output.JsonSerialize(Output.Create(new {
        Comment = "A Hello World example of the Amazon States Language using two AWS Lambda Functions",
        StartAt = "Hello",
        States = new Dictionary<string, object?>{
        ["Hello"] = new {
            Type = "Task",
            Resource = helloFunction.Arn, // Pulumi Resource Output
            Next = "World",
        },
        ["World"] = new {
            Type = "Task",
            Resource = worldFunction.Arn, // Pulumi Resource Output
            End = true,
        },
    },
}));
```

{{% /choosable %}}
{{< /chooser >}}

### Parsing JSON Outputs to Objects

You can parse a JSON string into an object and then, inside of an Apply, manipulate the object to remove all of the policy statements.

{{< chooser language "javascript,typescript,python,go,csharp" >}}
{{% choosable language javascript %}}

```javascript
const jsonIAMPolicy = pulumi.output(`{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "s3:ListAllMyBuckets",
                "s3:GetBucketLocation"
            ],
            "Resource": "*"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": "s3:*",
            "Resource": "arn:aws:s3:::my-bucket"
        }
    ]
}`);

const policyWithNoStatements = pulumi.jsonParse(jsonIAMPolicy).apply(policy => {
    // delete the policy statements
    policy.Statement = [];
    return policy;
});
```

For more details [view the NodeJS documentation](/docs/reference/pkg/nodejs/pulumi/pulumi/).

{{% /choosable %}}
{{% choosable language typescript %}}

```typescript
const jsonIAMPolicy = pulumi.output(`{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "s3:ListAllMyBuckets",
                "s3:GetBucketLocation"
            ],
            "Resource": "*"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": "s3:*",
            "Resource": "arn:aws:s3:::my-bucket"
        }
    ]
}`);

const policyWithNoStatements: Output<object> = pulumi.jsonParse(jsonIAMPolicy).apply(policy => {
    // delete the policy statements
    policy.Statement = [];
    return policy;
});
```

For more details [view the NodeJS documentation](/docs/reference/pkg/nodejs/pulumi/pulumi/).

{{% /choosable %}}
{{% choosable language python %}}

```python
json_iam_policy = pulumi.Output.from_input('''
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "s3:ListAllMyBuckets",
                "s3:GetBucketLocation"
            ],
            "Resource": "*"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": "s3:*",
            "Resource": "arn:aws:s3:::my-bucket"
        }
    ]
}
''')

def update_policy(policy):
    # delete the policy statements
    policy.update({'Statement': []})
    return policy

policy_with_no_statements = \
    pulumi.Output.json_loads(json_IAM_policy).apply(update_policy)
```

For more details [view the Python documentation](/docs/reference/pkg/python/pulumi/).

{{% /choosable %}}
{{% choosable language go %}}

```go
jsonIAMPolicy := pulumi.ToOutput(`{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "s3:ListAllMyBuckets",
                "s3:GetBucketLocation"
            ],
            "Resource": "*"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": "s3:*",
            "Resource": "arn:aws:s3:::my-bucket"
        }
    ]
}`).(pulumi.StringInput)

policyWithNoStatements := pulumi.JSONUnmarshal(jsonIAMPolicy).ApplyT(
    func(v interface{}) (interface{}, error) {

        // delete the policy statements
        v.(map[string]interface{})["Statement"] = []pulumi.ArrayOutput{}
        return v, nil
    })
```

For more details [view the Go documentation](https://pkg.go.dev/github.com/pulumi/pulumi/sdk/v3/go/pulumi).

{{% /choosable %}}
{{% choosable language csharp %}}

```csharp
var jsonIAMPolicy = Output.Create(@"
        {
            ""Version"": ""2012-10-17"",
            ""Statement"": [
                {
                    ""Sid"": ""VisualEditor0"",
                    ""Effect"": ""Allow"",
                    ""Action"": [
                        ""s3:ListAllMyBuckets"",
                        ""s3:GetBucketLocation""
                    ],
                    ""Resource"": ""*""
                },
                {
                    ""Sid"": ""VisualEditor1"",
                    ""Effect"": ""Allow"",
                    ""Action"": [
                        ""s3:*""
                    ],
                    ""Resource"": ""arn:aws:s3:::my-bucket""
                }
            ]
        }
    ");

var policyWithNoStatements = Pulumi.Output.JsonDeserialize<IAMPolicy>(jsonIAMPolicy).Apply(policy =>
{
    // delete the policy statements.
    policy.Statement = Pulumi.Output.Create(new List<StatementEntry> { });
    return policy;
})
```

For more details [view the .NET documentation](/docs/reference/pkg/dotnet/Pulumi/Pulumi.Output.html)

{{% /choosable %}}
{{< /chooser >}}

## Convert Input to Output through Interpolation

It is possible to turn an {{< pulumi-input >}} into an {{< pulumi-output >}} value. Resource arguments already accept outputs as input values however, in some cases you need to know that a value is definitely an {{< pulumi-output >}} at runtime. Knowing this can be helpful because, since {{< pulumi-input >}} values have many possible representations—a plain value, a promise, or an output—you would normally need to handle all possible cases. By first transforming that value into an {{< pulumi-output >}}, you can treat it uniformly instead.

For example, this code transforms an {{< pulumi-input >}} into an {{< pulumi-output >}} so that it can use the `apply` function:

{{< chooser language "javascript,typescript,python,go,csharp,java" >}}

{{% choosable language javascript %}}

```javascript
function split(input) {
    let output = pulumi.output(input);
    return output.apply(v => v.split());
}
```

{{% /choosable %}}
{{% choosable language typescript %}}

```typescript
function split(input: pulumi.Input<string>): pulumi.Output<string[]> {
    let output = pulumi.output(input);
    return output.apply(v => v.split());
}
```

{{% /choosable %}}
{{% choosable language python %}}

```python
def split(input):
    output = Output.from_input(input)
    return output.apply(lambda v: v.split())
```

{{% /choosable %}}
{{% choosable language go %}}

```go
func split(input pulumi.StringInput) pulumi.StringArrayOutput {
    return input.ToStringOutput().ApplyT(func(s string) []string {
        return strings.Split(s, ",")
    }).(pulumi.StringArrayOutput)
}
```

{{% /choosable %}}
{{% choosable language csharp %}}

```csharp
Output<string[]> Split(Input<string> input)
{
    var output = input.ToOutput();
    return output.Apply(v => v.Split(","));
}
```

{{% /choosable %}}
{{% choosable language java %}}

```java
public static Output<List<String>> split(Output<String> str) {
    return str.applyValue(v -> List.of(v.split(",")));
}
```

{{% /choosable %}}

{{< /chooser >}}

## Common Pitfalls

### String Interpolation

When working with outputs and apply, you may see an error message like so:

```
Calling __str__ on an Output[T] is not supported. To get the value of an Output[T] as an Output[str] consider: 1. o.apply(lambda v: f"prefix{v}suffix") See https://pulumi.io/help/outputs for more details.
```

The reason this is happening is because the _Output_ value is being used before it has returned and is available from the API.

A concrete example of this can be seen in the following code:

{{% notes %}}
The following code is an example of what NOT to do. Please do not copy this code into your Pulumi program
{{% /notes %}}

{{< chooser language "javascript,typescript,python,go,csharp,java" >}}

{{% choosable language javascript %}}

```javascript
// NOTE: This example is not correct
let contentBucket = new aws.s3.Bucket("content-bucket", {
  acl: "private",
  website: {
    indexDocument: "index.html",
    errorDocument: "index.html",
  },
  forceDestroy: true,
});

let bucketPolicy = new aws.s3.BucketPolicy("cloudfront-bucket-policy", {
  bucket: contentBucket.bucket,
  policy: JSON.stringify({
        Version: "2012-10-17",
        Statement: [
          {
            Sid: "CloudfrontAllow",
            Effect: "Allow",
            Principal: {
              AWS: iamArn,
            },
            Action: "s3:GetObject",
            Resource: bucket.arn.apply(arn => arn),
          },
        ],
      })
});
```

{{% /choosable %}}
{{% choosable language typescript %}}

```typescript
// NOTE: This example is not correct
const contentBucket = new aws.s3.Bucket("content-bucket", {
  acl: "private",
  website: {
    indexDocument: "index.html",
    errorDocument: "index.html",
  },
  forceDestroy: true,
});

const bucketPolicy = new aws.s3.BucketPolicy("cloudfront-bucket-policy", {
  bucket: contentBucket.bucket,
  policy: JSON.stringify({
        Version: "2012-10-17",
        Statement: [
          {
            Sid: "CloudfrontAllow",
            Effect: "Allow",
            Principal: {
              AWS: iamArn,
            },
            Action: "s3:GetObject",
            Resource: bucket.arn.apply(arn => arn),
          },
        ],
      })
});
```

{{% /choosable %}}
{{% choosable language python %}}

```python
# NOTE: This example is not correct
bucket = aws.s3.Bucket(
    "cloudfront",
    acl="private",
    website=aws.s3.BucketWebsiteArgs(
        index_document="index.html", error_document="404.html"
    ),
)

bucket_policy = aws.s3.BucketPolicy(
    "cloudfrontAccess",
    bucket=bucket.bucket,
    policy=json.dumps(
            {
                "Version": "2012-10-17",
                "Statement": [
                    {
                        "Sid": "CloudfrontAllow",
                        "Effect": "Allow",
                        "Principal": {
                            "AWS": "*",
                        },
                        "Action": "s3:GetObject",
                        "Resource": bucket.arn.apply(lambda arn: arn),
                    }
                ],
            }
        )
    ),
    opts=pulumi.ResourceOptions(parent=bucket)
)
```

{{% /choosable %}}
{{% choosable language go %}}

```go

// NOTE: This example is not correct
bucket, err := s3.NewBucket(ctx, "content-bucket", &s3.BucketArgs{
	Acl: pulumi.String("private"),
	Website: &s3.BucketWebsiteArgs{
		IndexDocument: pulumi.String("index.html"),
		ErrorDocument: pulumi.String("404.html"),
	},
})
if err != nil {
	return err
}

_, err = s3.NewBucketPolicy(ctx, "cloudfront-bucket-policy", &s3.BucketPolicyArgs{
	Bucket: bucket.ID(),
	Policy: json.Marshal(map[string]interface{}{
				"Version": "2012-10-17",
				"Statement": []map[string]interface{}{
					{
						"Sid":    "CloudfrontAllow",
						"Effect": "Allow",
						"Principal": map[string]interface{}{
							"AWS": iamArn,
						},
						"Action":   "s3:GetObject",
						"Resource": bucket.Arn.ApplyT(arn []interface{} (pulumi.StringOutput)),
					},
				},
			})
		}),
if err != nil {
	return err
}
```

{{% /choosable %}}
{{% choosable language csharp %}}

```csharp
// NOTE: this example is not correct
var bucket = new Bucket("content-bucket", new BucketArgs
{
    Acl = "private",
    Website = new BucketWebsiteArgs
    {
        IndexDocument = "index.html",
        ErrorDocument = "404.html",
    },
});

 var bucketPolicy = new BucketPolicy("cloudfront-bucket-policy", new BucketPolicyArgs
    {
        Bucket = bucket.Id,
        Policy = JsonSerializer.Serialize(new
        {
            Version = "2012-10-17",
            Statement = new[]
             {
                new
                {
                    Sid = "CloudfrontAllow",
                    Effect = "Allow",
                    Principal = new
                    {
                        AWS = "*"
                    },
                    Action = "s3:GetObject",
                    Resource = bucket.Arn.Apply(arn => $"{arn}/*"),
                }
            }
        })
    });
```

{{% /choosable %}}
{{% choosable language java %}}

```java
// NOTE: this example is not correct
var bucket = new Bucket("my-bucket", BucketArgs.builder()
    .acl("private")
    .website(BucketWebsiteArgs.builder()
        .indexDocument("index.html")
        .errorDocument("404.html")
        .build())
    .build());

var policy = new JSONObject();
            policy.put("Version", "2012-10-17");
            policy.put("Statement", new JSONArray(new JSONObject[] {
                new JSONObject()
                    .put("Effect", "Allow")
                    .put("Principal", "*")
                    .put("Action", "s3:GetObject")
                    .put("Resource", bucket.apply.arn(arn -> arn) + "/*")
            }));

            var bucketPolicy = new BucketPolicy("cloudfront-bucket-policy", BucketPolicyArgs.builder()
                    .bucket(bucket.id())
                    .policy(policy)
                    .build());
```

{{% /choosable %}}

{{< /chooser >}}

Notice how the `apply` call is being used inside the JSON string. In this scenario, the `apply` call is happening during the build of the JSON string, which is too early in the Pulumi lifecycle. The value of the bucket ARN has not yet been returned from the cloud provider API, so the JSON string cannot be built yet.

The correct way of handling this scenario is to wait for the bucket ARN to return, _then_ build the JSON string. You can see an example of this in the (#outputs-and-json) section.

Notice in that example how the JSON string is being built _inside_ the `apply` call to Pulumi. In logical order, this happens as:

- Wait for the bucket ARN to be returned from the cloud provider
- Then, build the JSON string with the "plain" value.

### Resource Names

When creating multiple resources, it can be tempting to use the output of one resource as the name to another:

{{< chooser language "javascript,typescript,python,go,csharp,java" >}}

{{% choosable language javascript %}}

```javascript
const bucket = new aws.s3.Bucket("my-bucket", {});

const bucketPolicy = new aws.s3.BucketPolicy(bucket.name, {
  // rest of bucket policy arguments go here
});
```

{{% /choosable %}}
{{% choosable language typescript %}}

```typescript
// NOTE: This example is not correct
const bucket = new aws.s3.Bucket("my-bucket", {});

const bucketPolicy = new aws.s3.BucketPolicy(bucket.name, {
  // rest of bucket policy arguments go here
});
```

{{% /choosable %}}
{{% choosable language python %}}

```python
# NOTE: This example is not correct
bucket = aws.s3.Bucket("my-bucket",)

bucket_policy = aws.s3.BucketPolicy(
    bucket.name,
   # rest of bucket policy arguments go here
)
```

{{% /choosable %}}
{{% choosable language go %}}

```go

// NOTE: This example is not correct
bucket, err := s3.NewBucket(ctx, "my-bucket", &s3.BucketArgs{})
if err != nil {
	return err
}

_, err = s3.NewBucketPolicy(ctx, bucket.Name, &s3.BucketPolicyArgs{
	Bucket: bucket.ID(),
	// rest of bucket policy arguments go here
})
if err != nil {
	return err
}
```

{{% /choosable %}}
{{% choosable language csharp %}}

```csharp
var bucket = new Bucket("my-bucket", new BucketArgs{});

var bucketPolicy = new BucketPolicy(bucket.Name, new BucketPolicyArgs
{
    Bucket = bucket.Id,
    // rest of the bucket policy arguments go here
})
```

{{% /choosable %}}
{{% choosable language java %}}

```java
var bucket = new Bucket("my-bucket")
var bucketPolicy = new BucketPolicy(bucket.bucket(),
    BucketPolicyArgs.builder()
    // the rest of the bucket policy arguments go here);
```

{{% /choosable %}}
{{< /chooser >}}

However, this pattern isn't possible because resource names need to be available and known at runtime in order to construct the URN of the resource.

If you really wish to pass an output from one resource as the resource name of another resource, you will need to use a {{< pulumi-apply >}}.

{{% notes %}}
This will lead to previews being inaccurate. Resources created inside an apply will only appear in the Pulumi output after an `up` operation is run, and is therefore strongly discouraged.
{{% /notes %}}

### JavaScript Promises

If you're familiar with JavaScript, you might already be familiar with the mechanism of [Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise).

Pulumi Output values are similar in some ways to JavaScript Promises, in that they are values that are not necessarily known until a future event. With that in mind, it can be tempting to try and use `Promise.resolve()` and to think of Outputs like Promises.

However, Pulumi Outputs do not behave in the same way as JavaScript Promises. As such, it's not possible to block execution of a Pulumi program until a promise is resolved and you should not use JavaScript Promise mechanisms to handle Pulumi Outputs.
