---
title: "Understanding Rust's Ownership Model"
author: 'Rohan Faiyaz Khan'
date: 2022-03-12
tags: ['rust', 'memory-management']
excerpt: 'This article looks at why Rust implements its ownership model, and how this model helps guarantee memory safety without the use of a garbage collector.'
---

_Rust_ has become a popular programming language for developers looking to write highly performant or low system level code. In a StackOverflow survey [^poplang], _Rust_ was voted the most "loved language" by users of the platform.

So why is Rust used? What advantages does it offer over other languages?

The primary selling point of Rust is that it is guaranteed to be memory-safe without requiring a garbage collector. Memory safety is defined as (1) an entity will never reference a memory outside of the allocated address space for that memory, (2) the program never executes instructions outside of the code area used by the compiler for the address space [^memsafety].

Traditionally the solution to memory safety has been garbage collection such as in languages such as Java, Javascript, Python, C# etc. A garbage collector automatically assigns memory to objects when they are created, and automatically clears unused memory when the object is no longer needed. However this extra utility comes at the cost of a runtime overhead. This is insignificant for most application level programs such as mobile apps and websites. However, lower system level programs, high performance apps such as games and video editing and embedded machine software, the cost of garbage collection can become too much to bear and more fine-tuned control of memory is desirable. Garbage collection also does not deal with concurrency. "Safe" languages that deal with concurrency such as Java do not offer any guarantees against data races. Hence managing memory in concurrent modes becomes once again the programmers' responsibility.

While this may call for programmers using systems level languages to become more proficient at memory management, this is not an easy task when dealing with massive codebases with a large team. Memory safety has to be guaranteed through stringent code review, and even so human errors can always creep in. According to Microsoft, 70% of its security issues are memory safety violations [^microsoft]. It is much more preferrable to offload the complexities of memory management to the language while the programmers can focus on the logic of the application.

This is where Rust's ownership model comes in. Through strict rules governing the access to memory, Rust is able to guarantee memory safety at _compile time_ thus removing the need for a runtime garbage collector. At the same time, this ownership model protects against data races during concurrent processes. Let's look at how it works.

## The Ownership Model

Memory in Rust is of two types - stack and heap. This model is shared among many programming languages, but Rust requires the user to be more involved in understand how each works. A value is stored on the stack in the order encountered in the program, and removed in the opposite order when the said value is no longer in scope. Data on the stack must have a known fixed size and be immutable. For example, integers, floats and string literals get stored on the stack.

```rust
fn main() {
    let x: u32 = 5;
    let y: f32 = 4.3;
    let z: &str = 'literal'

    println!("{}, {}, {}", x, y, z)
    // x, y and z are no longer accessed after this line
    // this is said to be the end of the lifetime of the variables
}
```

The example above indicates the point where the variables `x`, `y` and `z` are no longer accessed. In Rust, this is referred to as the end of the variable's lifetime and the compiler automatically calls `drop()` to remove the values from the stack.

In contrast heap memory is dedicated to values of variable length, such as mutable strings. When Rust comes across such a value, it allocates space on the heap and returns a pointer to that memory. The returned pointer is of a known fixed size. and hence gets stored on the stack. The variable storing the pointer is considered the "owner" of the value in memory. Ownership as a model in addition to memory safety, also keeps track of which parts of heap memory are currently in use, minimizes duplicate data and cleans up unused data to avoid memory leaks.

When a value is moved from one variable to another, Rust transfers ownership to the new variable. This is because Rust always ensures that any value stored in the heap has no more than one owner.

```rust
let s1 = String::from("hello");
let s2 = s1; // ownership transferred

println!("{}", s1) // will lead to error
```

Attempting to access `s1` after moving the value to `s2` will lead to an error because `s1` no longer has ownership over "hello". Note that this behavior is not the same when moving static values such as integers and string literals, which are simply copied.

```rust
let n1 = 5;
let n2 = n1; // copy value

println!("{}", n1); // still works
```

> Note that if we do need to copy the content of a string into a new variable, we can use the `string.copy()` method.

When the lifetime of the variable storing the pointer ends, i.e. it becomes out of scope, `drop()` is called on it and it is cleared off the stack. At the same time, when the owner of the allocated heap memory no longer exists, the memory is automatically de-allocated. This removes the need to garbage-collect this memory later.

## Ownership when passing data to functions

A hurdle when coming into Rust from other languages is understanding why values are passed into functions in a different manner as other languages. The reason the method in Rust is a little different is that due to the ownership model, we can also end up transfering ownership when calling a function. This is usually undesirable (although there are exceptions). Take the following example:

```rust
fn main(){
    let s1 = String::from("Hello");
    adds_world_to_str(s1); // ownership gets transferred to variable inside function

    println!({}, s1); // leads to error
}

fn add_world_to_str(s: String){
    s.push_str(" world");
}
```

When calling the function `add_world_to_str`, the value inside `s1` gets moved to the local variable inside the function, thus also transferring ownership. As a result, `s1` can no longer access the value. One way to fix this would be to return the value from the function, thus transferring ownership back.

```rust
fn main(){
    let s1 = String::from("Hello");
    let s2 = adds_world_to_str(s1); // takes ownership and returns it back

    println!({}, s2); // works now
}

fn add_world_to_str(s: String) -> String {
    s.push_str(" world");
    s
}
```

This works but it is inconvenient especially when the function is already returning something, in which case we would have to return a tuple. Rust provides a built in way to handle this, known as "borrowing". We can do this by passing a _reference_, which is like a pointer but one guaranteed to be pointing to valid memory. Passing a pointer to a function temporarily grants ownership to the function, and ownership is reclaimed when the local variable is no longer in scope.

```rust
fn main(){
    let s1 = String::from("Hello");
    some_func(&s1); // borrows the value

    println!({}, s1); // ownership is returned
}

fn some_func(s: &String) {
    println!({}, s)
}
```

One shortcoming here is that the reference to the value is immutable and attempts to mutate it will throw an error.

```rust
fn main(){
    let s1 = String::from("Hello");
    some_fadd_world_to_strunc(&s1);

    println!({}, s1);
}

fn add_world_to_str(s: &String) {
    s.push_str(" world"); // will throw error
}
```

To allow mutations we need to ensure that (1) the variable being passed is explicitly stated as being mutable and (2) the reference is a mutable reference. The former is done through the keyword `mut` and the later is done through the keyword `&mut`.

```rust
fn main(){
    let mut s1 = String::from("Hello");
    add_world_to_str(&mut s1);

    println!({}, s1);
}

fn add_world_to_str(s: &mut String) {
    s.push_str(" world"); // no longer throws error
}
```

There is yet another caveat to this. Firstly there can only be one mutable reference to a variable in scope at any one time. Secondly there cannot be any immutable references to the value if a mutable reference exists and is in scope. This is because users of the immutable reference are not expecting the value to suddenly change.

```rust
fn main(){
    let s1 = String::from("hello");

    let ref1 = &mut s1;
    let ref2 = &mut s1;
    let ref3 = &s1;

    print("{} {} {}", ref1, ref2, ref3);
    // Because of this print statement, ref1 is not yet out of scope.
    // There ref2 and ref3 cannot also borrow s1 in the same scope
}
```

A final restriction is that Rust disallows dangling references. This is when a function returns a reference to something no longer in scope. This means it is impossible to have a reference to nothing.

```rust
fn some_func(){
    s = String::from("hello");
    // we are returning a reference to s
    // even though s will no longer point to any value
    // this will lead to an error
    &s
}
```

## How this model leads to memory safety

Now that we have covered the ownership model of Rust, we can see how Rust enforces memory safety through it.

1. Rust informs the machine at compile time when allocated memory should be cleared by implicitly calling `drop()` function
2. Rust implements a type of reference that is guaranteed to point to a valid value in memory
3. For mutable data types, Rust ensures that only one variable is the owner of the object at any one time and that only one variable will be allowed to mutate it

These ensure some common memory safety bugs are handled out of the box. One such example is data races which occur when two or pointers exist for the same memory, and at least one of them is trying to write to the memory. Rust avoids this by forcing only one mutable reference for a variable at one time.

Another such example is the case of _double free error_. This occurs when two references to the same memory exist, and both are deallocated at the same time. Double de-allocation of memory can lead to memory corruption and unexpected behavior.

## Conclusion

We have gone over what Rust set out to achieve through its unique ownership model, and how the ownership model functions. We have also looked at how some common memory safety issues are dealt with out of the box by Rust. This should give a better position into appreciating what Rust's ownership model is and how to approach it. One thing I haven't discussed here is the potential downsides to the ownership model. There is no doubt that there is a significantly higher learning curve to Rust due to its unique approach, and this may inherently make adopting Rust for a development team more difficult. Furthermore, certain applications such as embedded systems commonly use shared memory accross systems. For such use cases, the ownership model may prove to be a hindrance than a help and there are quite a few developers who resort to using "unsafe" mode in Rust. If considering Rust for a project, one should ask themselves if the benefits of better performance and memory optimizations can justify the additional learning curve and restrictions Rust brings.

[^poplang]: StackOverflow, "Most Loved, Dreaded and Wanted Languages: 2020 Survey" Retrieved 21 March 2022 from https://insights.stackoverflow.com/survey/2020?utm_source=thenewstack&utm_medium=website&utm_campaign=platform#technology-most-loved-dreaded-and-wanted-languages-loved
[^memsafety]: D. Dhurjati, S. Kowshik, V. Adve and C. Lattner, "Memory safety without runtime checks or garbage collection," in Proceedings of the 2003 ACM SIGPLAN conference on Language, compiler, and tool for embedded systems, New York, NY, USA, 2003.
[^microsoft]: G. Thomas, "A proactive approach to more secure code," July 2019. Retrieved 8 March 2022 from https://msrc-blog.microsoft.com/2019/07/16/a-proactive-approach-to-more-secure-code/.
