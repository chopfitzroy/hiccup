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
* Children - `_` & `-`
* Modifiers - `&`
* States - `*`
* Globals - `!`

Note: we explicity avoid the use of the `~` and `+` symbols as a prefix becuase they can be used as CSS selectors so we thought it best to keep them out of class names.

We differentiate these class types using prefixes as follows:

**Globals**:

```scss
.\!lead{
  font-size: 30px;
}
```

Globals are the only selector not typecast and that do not require the direct descendant selector as they can be used globally.

**Components**:

```scss
div.menu {
  background-color: lime;
  display: inline-block;
  min-width: 350px;
  height: 100vh;
  float: left;
}
```

All classes **excluding globals** should be typecast to a certain element like `div.menu` why is this?
When we write CSS we assume certain properties about the element we are going to apply the class to (for example that a `div` is `display: block;`). This means despite how CSS is usually written there is an intended HTML structure for said CSS.
To strictly enforce the intended structure we require all classes be element typecast to force the correct semantics.

As an added bonus this also speeds up the browsers CSS selector engine.

**Children**:

```scss
div.menu {
  //...
  a._menu-link {
    display: block;
    padding: 10px;
    color: white;
    background-color: deepskyblue;
    border-bottom: 2px dashed white;
    text-decoration: none; 
  }
}
```

Like BEM we identify children using the `_` however we do this slightly differently using the `_` at the start of the class name followed by the parent component name with the `-` appended and then the class name of the child. Hiccup also recommends against using the `-` in naming components we do this to try and enforce a one word naming convention for our components for example we recommend `.menu` as opposed to `.side-menu`.

Unline BEM Hiccup does allow for the targeting of up to 3 tags without classes, this is bassed on some generic markup associated with traditional HTML elements, for example:

```html
<table>
  <tr>
    <td></td>
  </tr>
</table>

<ul>
  <li>
    <a></a>
  </li>
</ul>
```

If you find yourself needing more than 3 generic selectors you should probably revisit the way you are structuring your markup or whether or not you should be extracing a new component.

You may also nest your children in generic tags if you need to, like so:

```scss
div.menu {
  //...
  ul {
    li {
      a._menu-link {
        //...  
      } 
    }
  }
}
```

We usually recommend against this but do allow it, it is intended for plugins where you may find the structure of your markup is pre defined and you need to be more specific.

**Modifiers**:

```scss
div.menu {
  //...
  a._menu-ink {
    &.\&active {
      background-color: purple;
    }
  }
}
```

Modifiers will never by typecast because they will always be applied with another class whether it is a component or a child.

**States**:

```scss
div.menu {
  //...
  &.\*shrunk {
    a._menu-ink {
      //...
    }
  }
}
```

A state works similar to a modifier but it is top level and instead of just modifying the element it is applied to it allows you to modify its children and their modifiers. In most cases states will be applied/toggled with javascript (as seen in [demo](http://codepen.io/crashy/pen/vKVWZP))

### Conclusion

Hiccup is still in it's very early days and there is still plenty of room to improve so any suggetions / edits please fire through a PR.
