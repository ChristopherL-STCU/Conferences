# KCDC 2017

Note:
A generic developer conference/but leans toward Microsoft
3 days, first workshop, second two sessions
Feel free to stop me and ask questions

---

### Docker hands-on Workshop

- Volume or copy: ideally volume, so everything is setup and consistent for developers.
- Cross origin requests: use separate domain (discoverable) or proxy through web server
- How do you update containers: you don't you rebuild them
- How do you access logs: you don't, you store them somewhere durable

+++

- Cloud Floundary: Continuous deployment outside of Docker (Garmin)
- Why so many ways: try to meet the person where they are at
    - [A comparison](https://robrich.org/slides/docker-in-azure/#/41)
    - [AKS](https://azure.microsoft.com/en-us/blog/introducing-azure-container-service-aks-managed-kubernetes-and-azure-container-registry-geo-replication/)

Note:
For my workshop I picked docker
As a department using docker is something we've been looking into lately
Helpful to have a discussion with someone and get some clarification
Helpful things to think about
Other options
Azure container service vs app service. Also Azure container instance.

---

### A practical approach to using graph databases and analytics

- Nodes & relationships form a path
- Figure out what goes with what
    - Example: Amazon's customers also bought (35% of sales)

+++

- Can change the model easily
    - Add labels to code, change the code, done
    - Relational DB: Can break when changes are made, like when new relationships are added
- Reduced complexity and faster queries

Note:
Want to see if this is something we could incorporate into our apps.
I found it was more relevant to the warehouse.
    - And even if as a CU we started using it we would probably perform an extract and load into the graph db initially, instead of writing directly to it.

---

### [Deconstructing TypeScript](https://gitpitch.com/schneidenbach/TypeScriptTypeSystem)

- Union types
- Type guards
- Intersection types
- Type capture
- Discriminated unions
- <a target="_blank" href='http://www.typescriptlang.org/play/#src=interface%20Square%20{%0A%20%20%20%20kind:%20"square";%0A%20%20%20%20size:%20number;%0A}%0Ainterface%20Rectangle%20{%0A%20%20%20%20kind:%20"rectangle";%0A%20%20%20%20width:%20number;%0A%20%20%20%20height:%20number;%0A}%0Ainterface%20Circle%20{%0A%20%20%20%20kind:%20"circle";%0A%20%20%20%20radius:%20number;%0A}%0A%0Atype%20Shape%20=%20Square%20|%20Rectangle%20|%20Circle;%0A%0Afunction%20assertNever(x:%20never):%20never%20{%0A%20%20%20%20throw%20new%20Error("Unexpected%20object:%20"%20+%20x);%0A}%0A%0Afunction%20area(shape:%20Shape)%20{%0A%20%20%20%20switch%20(shape.kind)%20{%0A%20%20%20%20%20%20%20%20case%20"square":%20return%20shape.size%20*%20shape.size;%0A%20%20%20%20%20%20%20%20case%20"rectangle":%20return%20shape.height%20*%20shape.width;%0A%20%20%20%20%20%20%20%20default:%20return%20assertNever(shape);%0A%20%20%20%20}%0A}'>Never type</a>

Note:
Was a time where dynamic languages were the rage, people have realized the drawbacks, momentum switching to static
- Tools can assist
- Tough to make changes and be aware of all the ramifications Useful since we use Angular, which uses TypeScript TypeScript much more sophisticated than I realized Nice conversation about compile to languages and web assembly, but they seem convinced.
- I still think people will using web assembly for this in the future

+++

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

function area(shape: Shape) {
    switch (shape.kind) {
        case "square": return shape.size * s.size;
        case "rectangle": return shape.height * s.width;
        default: return assertNever(shape);
    }
}
```

@[1-15]
@[16-20] (No type is a subtype of or assignable to never, except never itself)
@[21-73] (Compile error - Argument of type 'Circle' is not assignable to parameter of type 'never')


+++

<h3><a href='http://www.typescriptlang.org/play/#src=interface%20PersonPostRequest%20{%0A%20%20%20%20firstName:%20string;%0A%20%20%20%20lastName:%20string;%0A}%0A%0Ainterface%20PersonOkResponse%20{%0A%20%20%20%20id:%20number;%0A}%0A%0A//indexed%20type%20and%20keyof%0Ainterface%20BadRequestResponse<TRequest>%20{%0A%20%20%20%20message:%20string;%0A%20%20%20%20modelState:%20{%0A%20%20%20%20%20%20%20%20[K%20in%20keyof%20TRequest]:%20string[];%0A%20%20%20%20};%0A}%0A%0A//union%20type%0Atype%20PersonResponse%20=%20%0A%20%20%20%20PersonOkResponse%20|%20BadRequestResponse<PersonPostRequest>%0A%0A//type%20guard%0Afunction%20isErrorResponse(response:%20PersonResponse)%20%0A%20%20%20%20:%20response%20is%20BadRequestResponse<PersonPostRequest>%20{%0A%20%20%20%20return%20(response%20as%20BadRequestResponse<PersonPostRequest>).modelState%20!==%20undefined;%0A}%0A%0A//type%20guard%20compile%20time%20awesomeness!%0Afunction%20handleResponse(response:%20PersonResponse)%20{%0A%20%20%20%20if%20(isErrorResponse(response))%20{%0A%20%20%20%20%20%20%20%20console.log(response.modelState.firstName[0]);%20%20//or%20whatever...%0A%20%20%20%20}%20else%20{%0A%20%20%20%20%20%20%20%20console.log(response.id);%0A%20%20%20%20}%0A}%0A'>Use a type guard to handle correct response</a></h3>

```typescript
//type guard
function isErrorResponse(response: PersonResponse) 
    : response is BadRequestResponse<PersonPostRequest> {
    return (response as BadRequestResponse<PersonPostRequest>).modelState !== undefined;
}

//type guard compile time awesomeness!
function handleResponse(response: PersonResponse) {
    if (isErrorResponse(response)) {
        console.log(response.modelState.firstName[0]);  //or whatever…
    } else {
        console.log(response.id);
    }
}
```

Note:
The type of response is known, that there is a modelState and has firstName

+++

### Q's

- What about other typed languages which compile to JavaScript or WebAssembly?

---

### Doing DevOps

> "Union of people, process, and products to enable continuous delivery of value to our end users"

+++

### Detrimental deployment cycle

1. Deployments so far apart
2. Because many have to approve
3. Because so far apart

Example:

- 10 seconds: a mistake is made creating a problem
- 10 minutes: the solution to the problem is known
- 6 hours: to approve the execution of the solution

+++

- There is a company doing devops which has it harder than what you have it.
- Treat as a product, not a project. Projects have a date. Products have value which can continually be improved.
- It can get worse before better.

+++

- Managers have different goals. They need to report deadlines.
- Managers aren't going to change something they can't see.
- Make the business the bottleneck. Showing they are the reason stuff is not getting done.

Note:
Goal of continuous deployment The cycle
    - We are in this cycle, for at least internal projects
    - Something we are going through with our current work
        - How can we deliver business value quickly
            - Desire to deliver little bits at a time
            - We'll be discussing this more in our retro/planning meeting later today.  Points to consider
    - We have Phoenix or Alkami, we can't do this. No, others have it harder
    - Change mindset, product can be incrementally improved, deployed
    - Complications to figure out along the way, snags Make business bottleneck. We have this thing we want to deploy, deliver value, but can't Good conversation on what is necessary before you begin, tdd


### Q's

- Is automated testing a prerequisite for quicker deployment cycles?

---

### 12 factor application development

- Monoliths take a long time to deploy
- Not how to write the code but how to manage the code you run it in
    - Goal: isolate changes and deploy quickly
- The observed patterns deploying rapidly to cloud

Note:
Continuing the theme, the next session thoughts on being do deploy apps quickly
A methodology for doing so from learned lessons
    - Varying degrees of importance and acceptance across the points
    - Things to keep in mind as we move in this direction

---

### C# without null or exceptions

[Gist: reidev275/IMaybe.cs](https://gist.github.com/reidev275/dd57c807a8db6ac2189a672c8b32a348)

```csharp
public interface IMaybe<T>
{
    IMaybe<U> Map<U>(Func<T, U> func);
    IMaybe<U> Bind<U>(Func<T, IMaybe<U>> func);
    IMaybe<T> Filter(Func<T, bool> predicate);
    IMaybe<T> Do(Action<T> some, Action none);
}

class Just<T> : IMaybe<T> {…}

class Nothing<T> : IMaybe<T> {…}

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

@[1-7]
@[8-11]
@[12-21]
@[22-27]
+++

### Q's

- Is it wise to bend C# like this?
- Does it cause a mix of approaches in the code?

Note:
The question on how to handle nulls
    - Nullable values, null coalesce, null conditional, other considerations in c# 8
    - Always a hot topic
One approach inspired by the Maybe Monad in Haskell
But is it wise to bend a language to behave like another. I'm not convinced.
What is the real impact of null exceptions to the CU?

---

### Clojure spec: expressing data constraints without types

 Why not types?

- Expressivity > proofs
- Flexibility about when and where you verify
- Types have costs, specs put you in control

Note:
Speaking of types, another approach by a dynamic language Declarative.
Can be used to tell you more information about your program as you develop.
    - Can help you address the question of building the right thing, instead of a thing that is right.

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

Note:
Things to keep in mind to create a helpful service, instead of having to fight and be frustrated by it.
Considerations as we potentially have free standing services or serverless functions

---

### Abstraction: enough is a enough

- Don't abstract on the 2nd use case, wait until a *known* 3rd *distinct* use case then write *good enough* abstraction
- Can still isolate common pieces in a method, just wait to write a more generic abstraction

Note:
Over use of the DRY principle?
    - When to be okay with duplication. When is there benefit to or not to.
    - Something to consider when writing or reviewing code. Are we making work for ourselves?

---

### Continuous delivery in Azure: beyond right click deploy

- Visual Studio experience
    - Locks the site (can't redeploy)
    - Very limited in how the application can be deployed
- Continuous deployment |
    - How are you going to publish or migrate a db

+++

- Continuous integration and deployment pipeline
    - AppVeyor
    - Octopus Deploy
    - Pick the technology which focuses on what you want to do, not that does everything
- ARM Templates |
    - Build everything from scratch

Note:
Lots of ways to deploy an application to Azure
    - Easy way is what's usually shown, but this quickly breaks down
    - Demonstrated an increasingly sophisticated way to reliable deploy a non-trivial application
    - Pick the tech with the sweet spot for what you need, don't fight the tools

---

### Shaving the golden Yak


- Doing seemingly unrleated tasks to get some real task done
- Be deliberate of the ones you shave

+++?image=kcdc2017/assets/black-yak.png


+++?image=kcdc2017/assets/black-yak-no-text.png

<div style="text:align:left;width: 50%">
<h4 style="text-transform: none">Your task is 80% complete these yaks form the next 80%</h4>

<ul>
<li>Summon with care</li>
<li>Track the attacks</li>
<li>Clear the path</li>
</ul>
</div>

+++?image=kcdc2017/assets/imperial-yak.png

+++?image=kcdc2017/assets/imperial-yak-no-text.png
<div style="float: right; width: 45%">
<h4 style="text-transform: none">One problem which goes deeper and deeper to fix things before you can do what you want</h4>
</div>

+++?image=kcdc2017/assets/trim-yak.png

+++?image=kcdc2017/assets/trim-yak-no-text.png
<div style="float: right; width: 45%">
<h4 style="text-transform: none">Growing productivity broadly. Not a specific task.</h4>
</div>

+++?image=kcdc2017/assets/royal-yak.png

+++?image=kcdc2017/assets/royal-yak-no-text.png
<div style="width: 45%">
<h4 style="text-transform: none">Usually seen as a time waster, but relationship building is a productivity booster</h4>
</div>

+++?image=kcdc2017/assets/golden-yak.png

+++?image=kcdc2017/assets/golden-yak-no-text.png
<div style="background-color: #ADEFFC">
<h4 style="text-transform: none">If you solve you totally change how you work, or maybe even the industry</h4>
</div>

Note:
- Soft skills talk
- We all deal with it. She classified the different types and assigned them a yak from an yak enthusiasts site
- Attack: most typical: pause and decide, track and then clear
- Yak Stack: one things leads to another: benefit of pairing (is this needed).  Urgent task -> workaround. Dangerous -> more important current task (more about team than you)
- Hackhacking: change our environment in order to influence our own behavior, increasing our speed over time: set up a new box just so, automating things. Timebox. Stay curious. Pair, lean what they know.
- Royal: need to talk to people to figure things out. In the end it's a boost, know what people know, or don't know. 
- Golden: changes everything, the one that alters your reality, changes behavior, give us an option we didn't have before. Rarely a direct work-related reason to shave them well enough to find their deep secrets. To find them cultivate an awareness of what we’re doing while we do it.

---

### Beyond patterns: functional programming and the Nature of Order

- Christopher Alexander's Fifteen Properties
- Applied to computer science

+++

### [Not-Separateness](http://www.tkwa.com/fifteen-properties/not-separateness-2/)

> "…the degree of connectedness an element has with all that is around it. A thing which has this quality feels completely at peace, because it is so deeply interconnected with its world."

- Impacts real people
- Programs not separate from the environment they run in

Note:
From OO to Function to ???
An attempt at considering the future of software, a contemplative approach
    - To be honest, it wasn't a very good talk, but sort of fitting to end on
    - After spending so much time thinking about the details this ask you to look up and consider things at a high level
    - The impact on society
    - What's the future impact of our software to our member's, beyond addressing immediate needs. Is this important?
