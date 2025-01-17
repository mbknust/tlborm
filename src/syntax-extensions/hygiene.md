# Hygiene

Hygiene is an important concept for macros, it describes the ability for a macro to work in its own syntax context, not affecting nor being affected by its surroundings.
In other words this means that a syntax extension should be invocable anywhere without interfering with its surrounding context.

In a perfect world all syntax extensions in rust would be fully hygienic, unfortunately this isn't the case, so care should be taken to avoid writing syntax extensions that aren't fully hygienic.
We will go into general hygiene concepts here which will be touched upon in the corresponding hygiene chapters for the different syntax extensions rust has to offer.

&nbsp;

Hygiene mainly affects identifiers and paths emitted by syntax extensions.

This is best shown by example:

Let's assume we have some syntax extension `make_local` that expands to `let local = 0;`, then given the following snippet:
```rust,ignore
make_local!();
assert_eq!(local, 0);
```

For `make_local` to be considered fully hygienic this snippet should not compile.
On the other hand if this example was to compile, the macro couldn't be considered hygienic, as it impacted its surrounding context by introducing a new name into it.

Now let's assume we have some syntax extension `use_local` that expands to `local = 42;`, then given the following snippet:
```rust,ignore
let mut local = 0;
use_local!();
```

In this case for `use_local` to be considered fully hygienic, this snippet again should not compile as otherwise it would be affected by its surrounding context and also affect its surrounding context as well.

This is a rather short introduction to hygiene which will be explained in more depth in the corresponding [`macro_rules!` `hygiene`] and proc-macro `hygiene` chapters, mainly explaining how hygienic these syntax extensions can be, be it fully or only partially.
There also exists this [github gist](https://gist.github.com/Kestrer/8c05ebd4e0e9347eb05f265dfb7252e1) that explains how to write hygienic syntax extensions while going into a hygiene a bit overall.

[`macro_rules!` `hygiene`]: ../decl-macros/minutiae/hygiene.md
