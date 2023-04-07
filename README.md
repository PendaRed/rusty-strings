# rusty-strings
Getting to the bottom of strings in rust

## String first

A string is a Vec<u8>, so you can clone it all you want, but for immutable strings this seems mad.

## &str

This is a slice, and string can be deref coorced into a slice, so if you take this as a param
then your function can be called with a String or a slice.

## What about "a string"

This is actually a 'static str.  So its a slice with a static lifetime.

## So how to share strings

You will have to look at the code, in particular the map_of_strings_reused which shows how Rc or Arc are
just brilliant.  Or maybe even Cow for a cache which periodically changes.

## The output

```
====== A struct holding a string
This clones the strings : Fred is 42 years old.
[src\main.rs:18] &v1 = StructStrings {
    name: "Fred",
    age: 42,
}
Type of v1 is rusty_strings::string_examples::struct_strings::StructStrings
alloc::string::String
The name is at 0x8aec13f270 with len 4, cap 4
====== A struct holding an &str
[src\main.rs:27] &v2 = SliceStrings {
    name: "Fred",
    age: 42,
}
Type of v2 is rusty_strings::string_examples::slice_strings::SliceStrings
&str
The struct   name is at 0x8aec13f4d8 with len 4
The variable name is at 0x8aec13f4c0 with len 4
The struct   name string is at 0x1e1f32166c0,
The variable name string is at 0x1e1f32166c0
====== Using an Rc (or Arc, etc) to not copy the string over and over.
The original string is at 0x1e1f32166c0

The cached string is at 0x8aec13fa50
The o1.name string is at 0x8aec13f9a8,
The o2.name string is at 0x8aec13f9b8,
The variable o1.name string is at 0x1e1f32166c0
The variable o2.name string is at 0x1e1f32166c0
```

