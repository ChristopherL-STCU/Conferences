# KCDC 2017

---?image=assets/black-yak.png

---?image=assets/md/assets/black-yak.png

---

### Docker hands-on Workshop

put on by rob rich

- Volume or copy. Ideally volume, so everything is setup and consistent for developers.
- cross origin requests: use separate domain (discoverable) or proxy
- How do you update containers: you don't you rebuild them
- How do you access logs: you don't, you store them elsewhere which is durable
- How to run containers: https://robrich.org/slides/docker-in-azure/#/41
- Why so many ways: try to meet the person where they are at, instead of making them adapt their current method
- Cloud Floundary: discussion with person from Garmin.

---

### A practical approach to using graph databases and analytics

- nodes & relationships form a path
- figure out what goes with what
    - like amazon, customers also bought: 35% of sales

+++

- Can break relational dbs when changes are made, net relationships are added
- Graph db: add labels to code, change the code
- Can change the model easily
- Reduced complexity and faster queries

---

### Deconstructing TypeScript

https://gitpitch.com/schneidenbach/TypeScriptTypeSystem

- Are using now with Angular
- Union types, type guards, intersection types, type capture, discriminated unions, never type

+++
 
http://www.typescriptlang.org/play/#src=interface%20Square%20{%0A%20%20%20%20kind:%20"square";%0A%20%20%20%20size:%20number;%0A}%0Ainterface%20Rectangle%20{%0A%20%20%20%20kind:%20"rectangle";%0A%20%20%20%20width:%20number;%0A%20%20%20%20height:%20number;%0A}%0Ainterface%20Circle%20{%0A%20%20%20%20kind:%20"circle";%0A%20%20%20%20radius:%20number;%0A}%0A%0Atype%20Shape%20=%20Square%20|%20Rectangle%20|%20Circle;%0A%0Afunction%20assertNever(x:%20never):%20never%20{%0A%20%20%20%20throw%20new%20Error("Unexpected%20object:%20"%20+%20x);%0A}%0A%0Afunction%20area(s:%20Shape)%20{%0A%20%20%20%20switch%20(s.kind)%20{%0A%20%20%20%20%20%20%20%20case%20"square":%20return%20s.size%20*%20s.size;%0A%20%20%20%20%20%20%20%20case%20"rectangle":%20return%20s.height%20*%20s.width;%0A%20%20%20%20%20%20%20%20default:%20return%20assertNever(s);%0A%20%20%20%20}%0A%20%20%20%20//error%20-%20didn't%20handle%20Triangle!%0A}%0A
```typescript

interface Square {
    kind: "square";
    size: number;
}
interface Rectangle {
    kind: "rectangle";
    width: number;
    height: number;
}
interface Circle {
    kind: "circle";
    radius: number;
}

type Shape = Square | Rectangle | Circle;

function assertNever(x: never): never {
    throw new Error("Unexpected object: " + x);
}

function area(s: Shape) {
    switch (s.kind) {
        case "square": return s.size * s.size;
        case "rectangle": return s.height * s.width;
        default: return assertNever(s); //error - Argument of type 'Circle' is not assignable to parameter of type 'never'
    }
}
```
+++

What about other typed languages which compile to JavaScript or WebAssembly?

+++

###  Use a type guard to handle correct response

```typescript
//type guard
function isErrorResponse(response: PersonResponse) 
    : response is BadRequestResponse<PersonPostRequest> {
    return (response as BadRequestResponse<PersonPostRequest>).modelState !== undefined;
}

//type guard compile time awesomeness!
function handleResponse(response: PersonResponse) {
    if (isErrorResponse(response)) {
        console.log(response.modelState.firstName[0]);  //or whatever...
    } else {
        console.log(response.id);
    }
}
```
---

### Doing DevOps

"Union of people, process, and products to enable continuous delivery of value to our end users" Damian ~ Brady (perhaps)

+++

### Detrimental deployment cycle

1. deployments so far apart
2. because many have to approve
3. because so far apart

Example:

10 seconds: a mistake is made creating a problem
10 minutes: the solution to the problem is known
6 hours: to approve execute of the solution

+++

There is a company doing devops which has it harder than what you have it

Treat as a product, not a project. Projects have a date. Products have value which can continually be improved.

It can get worse before better

+++

- Managers have different goals. They need to report deadlines.
- Managers aren't going to change something they can't see.
- Make the business the bottleneck. Showing they are the reason stuff is not getting done.

+++

Do you need automated testing in place?

---

### 12 factor application development

- Monoliths: long time to deploy
- Not how to write the code but how to manage the code you run it in
    - Goal: isolate changes and deploy quickly
- The observed patterns deploying rapidly to cloud

---

### C# without null or exceptions

https://gist.github.com/reidev275/dd57c807a8db6ac2189a672c8b32a348

```csharp
public interface IMaybe<T>
{
    IMaybe<U> Map<U>(Func<T, U> func);
    IMaybe<U> Bind<U>(Func<T, IMaybe<U>> func);
    IMaybe<T> Filter(Func<T, bool> predicate);
    IMaybe<T> Do(Action<T> some, Action none);
}

public static class IEnumerableExtensions
{
  public static IMaybe<T> Head<T>(this IEnumerable<T> list)
  {
    var result = list.FirstOrDefault();
    if (result == null) return new Nothing<T>();
    return new Just(result);
  }
}

new List<string>()
  .Head()
  .Do(
    just => Console.WriteLine("The head was " + just),
() => Console.WriteLine("Cannot take the head of an empty list"));
```
+++

- Is it wise to bend c# like this?
- Does it cause a mix of approaches is the code?

---

### Clojure spec: expressing data constraints without types

 Why not types?

- Expressivity > proofs
- Flexibility about when and where you verify
- Types have costs, specs put you in control

---

### Never RESTing: API design best practices using ASP.Net Web API

- Focus was public APIs
- Developers have the power of choice
- API design is UX for Developers

+++

### A good API should

- Get you started quickly
- Help you learn stuff, make decisions, and manage exceptions
- Be consistent
- Be static

---

### Abstraction: enough is a enough

- Don't abstract on the 2nd use case, wait until a *known* 3rd *distinct* use case then write *good enough* abstraction
- Can still isolate common pieces in a method, just wait to write a more generic abstraction

---

### Continuous deliver in Azure: beyond right click deploy

- Visual Studio experience
    - Locks the site
    - Very limited in how the application can be deployed
- Continuous deployment
    - How to publish a db
- Continuous integration and deployment pipeline
    - AppVeyor
    - Octopus Deploy
    - Pick the technology which focuses on what you want to do, not that does everything
- ARM Templates
    - Build everything from scratch

---

### Shaving the golden Yak

- Doing seemingly unrleated tasks to get some real task done
- Be deliberate of the ones you shave

+++

### Black yak/attack yak

Your task is 80% complete these yaks form the next 80%

- Summon with care
- Track the attacks
- Clear the path

+++

### Imperial yak/Yak stack

One problem which goes deeper and deeper to fix things before you can do what you want

+++

### Trim yak/Hackhacking yak

Growing productivity broadly. Not a specific task.

+++

### Royal yaks/ Yakkity yaks

Usually seen as a time waster, but relationship building is a productivity booster

+++

### Golden yaks

If you solve you totally change how you work, or maybe even the industry.

---

### Beyond patterns: functional programming and the Nature of Order

- Christopher Alexander's Fifteen Properties
- Applied to computer science

+++

### Not-Separateness

http://www.tkwa.com/fifteen-properties/not-separateness-2/
"the degree of connectedness an element has with all that is around it. A thing which has this quality feels completely at peace, because it is so deeply interconnected with its world."

- Impacts real people
- Programs not separate from the environment they run in

