### How should we handle text?

For this example consider a theoretical text "component" or node or content WHATEVER.
How do we want to handle normal text, colored text, text with variables? Do those variables
auto-update the UI?

Also, component system is not set in stone, consider the vbox stuff as pseudo. Here's some proposals, written in a psuedo-C++


```C++
auto favNumber = TextFragment<int>();
favNumber = 7; // overload assignment to call 

vbox({
    // when parsing, the favNumber text fragment must be aware of what components it
    // a member of so it can automatically update them when the value of favNumber is changed.
    30% text("My favorite number is " + favNumber + "!") | border(Border::ROUNDED),
    30% text("My favorite number times two is " + [](){ return 2 * favNumber() } + "!")
    // i'm not sure if this would work, we would have to dynamically handle lambda function in
    // the somewhere behind the scenes
})
```

```C++
auto text = TextComponent("My favorite number is {num}!");
text.set("num", 1);

vbox({
    30% text | border(Border::ROUNDED)
})

// has the benefit of being simpler to program, takes more memory
// (i assume bc backed by a hashmap?), and is called on the text component itself so already
// context aware. Component still needs to know where it is relative to the rest of the
// map of other guys but whatever
```
