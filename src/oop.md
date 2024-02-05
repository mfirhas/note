# Object-Oriented Programming

source: ~~trust me~~My opinions about OOP

So, What is OOP?, There are many explanations about OOP out there, and I myself not sure which is the most "official" one. 

Here are some of the definitions of OOP(after reading some sources; books, blogs/articles, forums, chat groups):
1. OOP is when you combine data and behaviours/methods/operations into one entity. So anything with data and methods, could be sign OOP*-ness*.
2. OOP is when you use particular languages with OOP constructs, e.g: Java, C#, Javascript, PHP, Python, etc.
3. OOP is just a paradigm, the language doesn't matter, and you can apply the paradigm in any languages.
4. OOP is the ones having 4 features of OOP: Encapsulation, Inheritance, Polymorphism, and Abstraction.
5. OOP is when you apply the Clean Arch/SOLID design patterns and GoF design patterns in your code.
6. OOP is when you do message passing and late-binding.(from original term of OOP itself)

...and many more...

All of those definitions are correct and seen in real-life OOP implementations. But if we give too much definitions on a thing, it's weakening the meaning of it and might give us multiple implementations with its own understanding and claim it OOP.

Along with above definitions, I want to add some more things on them, which really define the OOP-ness of the languages. Again, this is just opinion and might be wrong.

Most languages with OOP as their main paradigm, made *Objects* as reference types(Java, C#, Javascript, etc). This means that, most of the times, the underlying implementations of them are using some kind of pointers(hence you get NPE error in Java for null objects). Using reference make up the late bindings, and most probably dynamically dispatched. Those references are not known their data's exact size until got initiated at runtime. The references communicate eachother through message-passing, which mostly implemented via methods. This is what definitions number 1 & 6 are all about.

In OOP you design your program in term of objects, and an object is encapsulation of data and behaviours. Most languages implemented this via a construct called `Class` like in Java, C#, PHP, Python, etc. While some others implemented it via prototype-object like in Javascript. Class is just Abstract Data Type for your object, and mostly they are dynamic. These classes will get instantiate at runtime. What it means to be instantiated?, From users(humans) perspective it means creating an object with or without initial values become an instance on its own. But, from machine perspective it means *boxing* the *things* inside those classes into somewhere else, mostly heap, because of dynamic-ness of object. What is boxing means?, to me, it's just allocating memory(mostly heap) for the data it contains along with behaviour that might be stored inside a container called vtable.

With that additional words about OOP, Object is just something *boxed* and *referenced* by others throughout the program lifecycle. The box could be empty and when you invoke this empty box, you got Null Pointer Exception(atleast in Java). This means that the box is just somekind of pointer to the actual data stored in other region of memory(mostly heap). Are these data the box *contained* are gonna be referenced forever?, what if nobody referencing them again?, For this, most OOP languages have GC to manage the lifetime of Objects and their references eachother. GC take care of allocating, de-allocating, moving, and tracking relations between those objects. It's separate program running alongside your main program, tracking things on your behalf.

Does that mean all OOP languages require GC in order to work?, not exactly. Language like Swift doesn't require GC, instead it employs a mechanism called Automatic Reference Counting to track and manage how many references left for objects. C++ employ a concept called RAII to manage "objects lifecycle and lifetime.

Another thing about OOP is that, they have hierarchical types, means that there's some kind of object which is root of all objects defined, and all other objects derived/exteded from this "root object".
- In Java the root object is [*Object*](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html) class.
- In C# the root object is [*Object*](https://learn.microsoft.com/en-us/dotnet/api/system.object?view=net-8.0) class
- In Python it's *object* class.
- In Ruby it's [*Object*](https://ruby-doc.org/3.2.2/Object.html) class.
- In Javascript it's [*Object*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object) object.

This is what make OOP languages differ from others, they have *tree* of objects related to eachother in relation of inheritance/extension. I'm not sure if this is exist in languages like C, C++, Rust, Go, etc.

Hierarchical structure of "types" in OOP provide us what we called *inheritance*, *polymorphism* and *abstraction*, 3 of 4 core tenets in OOP.

For *polymorphism*, OOP specifically talk about subtype polymorphism.

So, OOP is just where objects are boxing things and got referenced and communicate eachother(message-passing/methods), and these objects have relations(subtyping) to eachothers, dynamically dispatched and/or late binding.

Despite all above definitions, any language can simulate OOP paradigm and still call it OOP, even though it's not its main paradigm.

Here are some example of OOP-ness of some languages I know:
- Go:
  - "Object" is represented via pointer types along with methods and `interface` type.
  - Encapsulate data and behaviours with method receivers and package/directory access level.
  - Has no inheritance:
    - instead prefer compositions where you can declare struct inside struct to derive its attributes and functionalities.
    - Code reuse?, it's okay not too DRY in Go.
  - Polymorphism with `interface{...}`
  - Im not sure how exactly the concept of *abstraction* will work in Go.

- Java:
  - "Object" is represented via Class in form of concrete class, enum class, abstract class, and interface class.
  - Encapsulate data and behaviours inside class.
  - Inheritance: type inheritance(class inherit other class), and behaviour inheritance(interface class).
  - Polymorphism: subtype polymorphism with abstract class(single inheritance) and interface class(multiple inheritance).
  - Abstraction: through abstract class.

- Rust
  - "Object" is represented via smart pointer: 
    - `Box<T>`, base for owned object.
    - `RefCell<T>`, mutable reference object.
    - `Rc<T>`, single-threaded reference object.
    - `Arc<T>`, multi-threaded reference object.
    - `Rc<RefCell<T>>`, single-threaded mutable reference object, it's not OOP if you cannot mutate things, since OOP is stateful and handling states.
    - `Arc<Mutex<T>>`, multi-threaded mutable reference object.
    - ...and many more.
  - Encapsulate data and behaviours through inherent methods associated with `struct` or `enum`.
  - No inheritance in Rust, so how about reusability?, you can use `trait` for that with associated types and default behaviours.
  - Polymorphism:
    - no subtype polymorphism in Rust(AFAIK), except lifetime, instead it uses Parametric Polymorphism.
    - for *interface*-like polymorphism, use trait object(`dyn`).
  - Abstraction: same with inheritance, use trait for this.

All 3 above languages represent their own main paradigm: Procedural Programming(Go), OOP(Java), FP(Rust), eventhough you can mix the paradigms.