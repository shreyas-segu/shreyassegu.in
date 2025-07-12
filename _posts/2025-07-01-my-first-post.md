---
title: "Markdown Kitchen Sink"
date: 2025-07-01
tags: ["markdown", "jekyll"]
description: "A sample post showcasing all the Markdown elements for styling purposes."
---

This post is a "kitchen sink" of all the possible Markdown elements you might use in a blog post. It's designed to help with styling and ensuring everything looks great.

## Headings

# Heading 1
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5
###### Heading 6

---

## Paragraphs and Text Styles

This is a standard paragraph. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.

Here is some **bold text**, some *italic text*, and some ***bold and italic text***. You can also use `_` for italics like _this_ and `__` for bold like __this__.

For inline code, you can use backticks: `const example = 'hello world';`.

Sometimes you want to strike through text, like ~~this~~.

---

## Blockquotes

> "The only way to do great work is to love what you do."
>
> -- Steve Jobs

Blockquotes can also be nested:
> This is the first level of quoting.
>
> > This is nested blockquote.
>
> Back to the first level.

---

## Lists

### Unordered List

*   Item 1
*   Item 2
    *   Nested Item 2.1
    *   Nested Item 2.2
*   Item 3

### Ordered List

1.  First item
2.  Second item
3.  Third item
    1.  Sub-item A
    2.  Sub-item B

### Task List (GitHub Flavored Markdown)

- [x] Complete task 1
- [x] Complete task 2
- [ ] Incomplete task 3

---

## Code Blocks

Syntax highlighting is handled by Rouge. Here are a few examples.

### JavaScript

```javascript
// A simple function to add two numbers
function add(a, b) {
  return a + b;
}

console.log(add(5, 10)); // Outputs: 15
```

### Python

```python
# A simple list comprehension
squares = [x**2 for x in range(10)]
print(squares)
```

### Shell/Bash

```shell
# List files in the current directory
ls -la

# Echo a message
echo "Hello from the shell!"
```

### Ruby (Jekyll's language)

```ruby
# A simple Ruby class
class Greeter
  def initialize(name)
    @name = name.capitalize
  end

  def salute
    puts "Hello #{@name}!"
  end
end

g = Greeter.new("world")
g.salute
```

---

## Horizontal Rule

You've seen them a few times already. They are created with three or more hyphens, asterisks, or underscores.
---

## Links and Images

### Links

Here is a link to the [Jekyll website](https://jekyllrb.com/). You can also have a link with a title: [Pico.css](https://picocss.com "A minimalist CSS framework").

### Images

To display an image from your project files, you first need to create a folder for them, like `assets/images/`. Place your image file in that directory. Then, you can reference it in your markdown like this:

![A sample image for the kitchen sink post]({{ site.baseurl }}/assets/images/kitchen-sink.png)

---

## Tables

Tables are great for structured data.

| Header 1     |   Header 2   |      Header 3 |
| :----------- | :----------: | ------------: |
| Left-aligned |   Centered   | Right-aligned |
| Cell content |  *this is*   |        `code` |
| Cell content | **centered** |      and this |

---

## Footnotes

Here is a sentence with a footnote.[^1] And here is another one.[^2]

[^1]: This is the first footnote. It's a kramdown feature.
[^2]: This is the second footnote. It can contain **formatted** text.

That's a wrap! This should cover most of the elements you'll need to style.
