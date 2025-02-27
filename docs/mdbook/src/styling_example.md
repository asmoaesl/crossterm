# Example

_setup the basics_
```rust
extern crate crossterm;

use crossterm::{Colored, Color, Attribute, Styler, Colorize};

fn main() {
    /* your code here */
}
```

There are a couple of ways to style the terminal output with crossterm. The most important part of the styling module is `StyledObject`.

A `StyledObject` is just a wrapper crossterm uses to store the text and style together. 
A `StyledObject` implements `Display` and thus you could use it inside `print!`, `println!` etc.

Without further ado let's get straight into it.

## Coloring

There are a few ways to do the coloring, the first one is by using the `Colored` enum. 

### Using Enum
```rust
println!("{} Red foreground color", Colored::Fg(Color::Red));
println!("{} Blue background color", Colored::Bg(Color::Blue));
```
`Colored::Bg` will set the background color, and `Colored::Fg` will set the foreground color to the provided color. 
The provided color is of type `Color` and has a bunch of enum values you could choose out.  

Because `Colored` implements `Display` you are able to use it inside any write statement.

### Using Methods
You can do the same as the above in a slightly different way. Instead of enabling it for all text you could also color the only piece of text.
(Make sure to include the `crossterm::Coloring` trait).

```rust
let styled_text = "Red forground color on blue background.".red().on_blue();
println!("{}", styled_text);
```

As you see in the above example you could call coloring methods on a string. How is this possible you might ask..? 
Well, the trait `Coloring`, who you need to include, is implemented for `&'static str`. 
When calling a method on this string crossterm transforms it into a `StyledObject` who you could use in your write statements.


### RGB
Most UNIX terminals and all Windows 10 consoles are supporting [True color(24-bit)](https://en.wikipedia.org/wiki/Color_depth#True_color_(24-bit)) coloring scheme.
You can set the color of the terminal by using `Color::RGB(r,g,b)`.

```    
// custom rgb value (Windows 10 and UNIX systems)
println!("{}{} 'Light green' text on 'Black' background", Colored::Fg(Color::Rgb { r: 0, g: 255, b: 128 }), Colored::Bg(Color::Rgb {r: 0, g: 0, b: 0}));
```
This will print some light green text on black background.

### Custom ANSI color value
When working on UNIX or Windows 10 you could also specify a custom ANSI value ranging up from 0 to 256.
See [256 (Xterm, 8-bit) colors](https://jonasjacek.github.io/colors/) for more information.

```
// custom ansi color value (Windows 10 and UNIX systems)
println!("{} some colored text", Colored::Fg(Color::AnsiValue(10)));
```

## Attributes
When working with UNIX or Windows 10 terminals you could also use attributes to style your text. For example, you could cross your text with a line and make it bold.
See [this](styling.md#Attributes) for more information.

### Using Enum
You could use the `Attribute` enum for styling text with attributes. 
`Attribute` implements `Display`, thus crossterm will enable the attribute style when using it in any writing operation.

```
println!(
    "{} Underlined {} No Underline",
    Attribute::Underlined,
    Attribute::NoUnderline
);
```

### Using Method

You can do the same as the above in a slightly different way. Instead of enabling it for all text you could also style only one piece of text.
(Make sure to include the `crossterm::Styler` trait).

```
println!("{}", "Bold text".bold();
println!("{}", "Underlined text".underlined();
println!("{}", "Negative text".negative();
```

As you see in the above example you could call attributes methods on a string. How is this possible you might ask..? 
Well, the trait `Styling`, who you need to include, is implemented for `&'static str`. 
When calling a method on any string crossterm transforms will transform it into a `StyledObject` who you could use in your write statements.

---------------------------------------------------------------------------------------------------------------------------------------------
More examples could be found at this [link](https://github.com/TimonPost/crossterm/blob/master/examples/style.rs).
