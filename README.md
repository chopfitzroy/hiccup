# Hiccup

A super strict yet probably too lenient methodology for writing maintainable CSS.

If you want to see it in action demo [here](http://codepen.io/crashy/pen/vKVWZP) (intentionally ugly to make it easy to follow).

### Intention

To encourage minimal markup and help write concise and maintainable css.

### Why

Why not use just another CSS naming convention like BEM or OOCSS. I found that they were either too dictating, too restrictive, or just made my class names rediculosly huge like `.some-class__child-class--class-modifier` really??? O.o who wants that?

### How

So how does it work? Please bear in mind that this is a living document and that it is subject to change.

There are five kinds of classes used with the hiccup methodology:

* Components
* Globals - `!`
* Children - `_`
* Modifiers - `~`
* States - `+`

Note: we explicity avoid the use of the `-` symbol as a prefix so we can use it in our classnames `.class-name` whithout it getting confusing.

We differentiate these class types using prefixes as follows:

**Globals**:

```scss
.\!font-large {
  font-size: 30px;
}
```

Globals are the only selector not typecast and that do not require the direct descendant selector as they can be used globally.

**Components**:

```scss
div.side-menu {
  background-color: lime;
  display: inline-block;
  min-width: 350px;
  height: 100vh;
  float: left;
}
```

All classes **excluding globals** should be typecast to a certain element like `div.alert` why is this?
When we write CSS we assume certain properties about the element we are going to apply the class to (for example that a `div` is `display: block;`). This means despite how CSS is usually written there is an intended HTML structure for said CSS.
To strictly enforce the intended structure we require all classes be element typecast to force the correct semantics.

As an added bonus this also speeds up the browsers CSS selector engine.

**Children**:

```scss
div.side-menu {
  //...
  > ul._nav {
  //...  
  }
}
```

Unlike BEM components children can have children as long as all children remain within said component.
This means your CSS will not be as flat as BEM but it will let you move up and down the DOM with more ease.

```scss
div.side-menu {
  //...
  > ul._nav {
    li {
      > a._link {
        // Child of child
      }
    }
  }
}
```

Children should also always be targeted using the direct descendant slector.
This is intentional so if you have a component nested within another component and both components have children with the same name the nested componets child will **not** inherit the parent components child properties.
Hiccup however does allow for up to 3 levels of non-classed selectors like so:

```scss
div.side-menu {
  //...
  > ul._nav {
    li { // Generic selector
      > a._link {
        //...
      }
    }
  }
}
```

This allows a level of flexibility within the hiccup framework, as there are (not usually, but occaisonally) scenarios where you will want to use targeting of the HTML tags directly to help your CSS understand how your HTML is structured.
You can target HTML tags only to mirror your HMTL structure in your CSS if you want add custom style attributes to an element it becomes a child, this way as you read through your mark up you will quickly be able to identify which elements have custom styling. This will also prevent any nested components acciedently getting styled by generic HTML tag selectors, hence why any custom styling must be assigned to a child.

**Modifiers**:

```scss
div.side-menu {
  //...
  > h3._title {
    //...
  }
  > ul._nav {
    li {
      > a._link {
        //..
        &.\~active {
          background-color: purple;
        }
      }
    }
  }
}
```

Modifiers will never by typecast because they will always be applied with another class whether it is a component or a child.

**States**:

```scss
div.side-menu {
  //...
  > ul._nav {
    li {
      > a._link {
        //...
        &.\~active {
          //...
        }
      }
    }
  }
  &.\+shrunk {
    //...
    > ul._nav {
      li {
        > a._link {
          //...
        }
      }
    }
  }
}
```

A state works similar to a modifier but it is top level and instead of just modifying the element it is applied to it allows you to modify its children and their modifiers. In most cases states will be applied/toggled with javascript (as seen in [demo](http://codepen.io/crashy/pen/vKVWZP))

### Conclusion

Hiccup is still in it's very early days and there is still plenty of room to improve so any suggetions / edits please fire through a PR.
