# hiccup

A super strict yet probably too lenient methodology for writing maintainable CSS.

### Intention

To encourage minimal markup and help write concise and maintainable css.

### Why

Why not use just another CSS naming convention like BEM or OOCSS. I found that they were either too dictating, too restrictive, or just made my class names rediculosly huge like `.some-class__child-class--class-modifier` really??? O.o who wants that?

### How

So how does it work? Please bear in mind that this is a living document and that it is subject to change.

There are five kinds of classes used with the hiccup methodology:

* Globals - `!`
* Components
* Children - `_`
* Modifiers - `~`
* States - `+`

Note: we explicity avoid the use of the `-` symbol as a prefix so we can use it in our classnames `.class-name` whithout it getting confusing.

We differentiate these class types using prefixes as follows:

**Globals**:

```scss
.\!font-white {
  color: white;
}
```

Globals are the only selector not typecast and that do not require the direct descendant selector as they can be used globally.

**Components**:

```scss
div.alert {
  padding: 20px;
  border: 2px solid black;
}
```

All classes **excluding globals** should be typecast to a certain element like `div.alert` why is this? When we write CSS we assume certain properties about the element we are going to apply the class to. This means despite how css is usually written there is an intended structure for the classes we write. To strictly enforce this we require all classes be element typecast to force the correct semantics.

As an added bonus this also speeds up the browsers CSS selector engine.

**Children**:

```scss
div.alert {
  //...
  > h4._title {
    font-size: 30px;
  }
}
```

Children should always be targeted using the direct descendant slector. Hiccup however does allow for up to 3 levels of non-classed selectors so if you do need to drop down 2 or 3 elements to reach your child selector you would use the following code:

```scss
div.alert {
  //...
  div {
    div {
      > h4._title {
        font-size: 30px;
      }
    }
  }
}
```

This allows a level of flexibility within the hiccup framework, there are (not usually, but occaisonally) scenarios where the best option will be to target the HTML tags directly.

Note: in cases where you do have some overlap with yoru non classed selectors (this will be when one of your childrens typecasting matches the generic HTML tag) I would recommend using the `:not()` selector.

```scss
div.alert {
  //...
  div:not([class]) {
    div:not(.component) {
      > h4._title {
        font-size: 30px;
      }
    }
  }
}
```

Why do we use the direct descendant selector?

Becuase if we have nested components with children of the same name, the second children with the same name will inherit styles from the first child class.

For example.

```scss
div.component-one {
  div._child {
    // Has it's own styles
  }
  div.component-two {
    div._child {
      // Would have it's styles + the styles from the ._child above it,
      // if the direct descendant selector was not used
    }
  }
}
```

**Modifiers**:

```scss
div.alert {
  //...
  &.\~raised {
    box-shadow: 10px 10px 5px 0px rgba(0,0,0,0.75);
  }
  > h4._title {
    //...
    &.\~important {
      text-text-transform: uppercase;
      font-weight: bold;
    }
  }
}
```

Modifiers will never by typecast because they will always be applied with another class whether it is a component or a child.

**States**:

```scss
div.alert {
  //...
  &.\+danger {
    background-color: red;
    &.\~raised {
      box-shadow: 15px 15px 10px 0px rgba(0,0,0,0.5);
    }
    > h4._title {
      color:#FFF
      &.\~important {
        text-text-transform: capitalize;
      }
    }
  }
}
```

A state works similar to a modifier but it is top level and instead of just modifying the element it is applied to it allows you to modify its children and their modifiers.

### Conclusion

Hiccup is still in it's very early days and there is still plenty of room to improve so any suggetions / edits please fire through a PR.
