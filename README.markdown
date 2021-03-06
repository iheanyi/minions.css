# minions

Evil, friendly, and obedient immutable CSS class selectors.

```html
<div class="p-1"> This element has 1em of padding</div>
<div class="b-1p"> This element has a 1px border</div>
<div class="m-1r"> This element has 1rem margin</div>
<div class="m-1r m-2r@sm"> This element has 1rem margin on mobile and a 2rem margin for anything above </div>
<div class="p_05">
  <div> Every direct child of this element has .5em of padding</div>
  <div class="p-0!"> ...but not this one. It's been reset to 0 </div>
</div>
```

## goals

* "immutable", single-property classes
* whatever-first. mobile-first, wearable-first, mega-widescreen-first, toaster-first, who cares?
* predictable class names
* the best of inline, plus media queries

## why?

That's a great question.

I've come to believe that the biggest problem with CSS is that you have to name things to do anything. People suck at naming things and only the best people go back to reconsider.

This approach allows you to defer naming thing... maybe indefinitely.

## syntax
    {property-shortand}-{value-shorthand}

#### property-shorthand

In the majority of cases, properties get shortened to a single charactor per word.

    display  = d
    ovelflow = o
    margin   = m
    padding  = p

Properties with two words (separated by a dash) will have two characters.

    border-color = bc
    border-width = bw

Likewise specific properties get a carachter for each dash-delimited word.

    border-top-width    = btw
    border-right-width  = brw
    border-bottom-width = bbw
    border-left-width   = blw

`x` and `y` have been added as aliases for `left & right` and `top & bottom`, respectively, for box-model properties.

    border-left-width && border-right-width  = bxw
    border-left-top   && border-bottom-width = bxw

Where there is a conflit, the rules should still apply. `background-color` is an example where another shorthand is needed, to avoid conflicts with `border-color`. Here I've used `gc` for `background-color` because `bc` is taken by `border-color`.

    bc = border-color
    gc = background-color

Sadly, these exceptions just have to be memorized and internalized. Fortunately, there aren't many of them.

#### value-shorthand

In the majority of cases, values get shortened to a single character per word.

    1px = 1p
    hidden = h
    inline-block = ib

`em` is used for measurement shorthand. `em`s are a sensible default because they are alays a multiplier of font-size. At some point I'll likely variations of this library with othe defaults. Until then, the other measurement values are suffixed.

    1px = 1p
    1rem = 1r
    1em = 1

#### measurements

There is no additional abstraction between measurement values and class selectors:

    1em = 1
    .5em = 05
    .25em = 025

    1rem = 1r
    .5rem = 05r
    .25rem = 025r

I could see this feeling cumbersome to some. I'll likely add some abstracted values (e.g., `xs`, `sm`, `md`, `lg`) as an option.

#### opacity/lightness

For values of opacity and lightness, the unsuffixed value is the default value and those suffixed with a number represent a point value.

    .c-l   { color: hsl(0, 0%, 100%)
    .c-l.5 { color: hsl(0, 0%,  50%)
    .c-l.0 { color: hsl(0, 0%,   0%)

    .o   { opacity: 1 }
    .o.5 { opacity: .5 }
    .o.0 { opacity: 0 }

Out of the box, only classes for 10ths are provided. But This can easily be extended to fit your needs.

    .c-l.42 { color: hsl(0, 0%, 52%) }
    .c-l.13 { color: hsl(0, 0%, 13%) }

    .o.42 { opacity: .42 }
    .o.13 { opacity: .13 }

#### negative

There is only one number prefix. `n` may be used to prefix a numer as `negative`. I'm avoiding the use of `-` to prevent confusion with OOCSS-style classes that use the `--` (double-dash) delimiter as a modifier.

    .o-1  { order: 1 }
    .o-n1 { order: -1 }

## property lexicon
    ai  = align-items
    as  = align-self
    bw  = border-width
    bc  = border-color
    c   = color
    d   = display
    f   = float, flex
    fd  = flex-direction
    gc  = background-color
    jc  = justify-content
    m   = margin
    o   = opacity, overflow, order
    p   = padding, position
    ta  = text-align
    v   = visibility

`background-color` is an outliers. `gc` is better than `bgc` for background-color. These are obviously my opnion but this is my library. So I'm still sleeping well at night.

### attributes

Attributes are hi-level modifiers to certain low-level classes.

    .fd-r.-row-gutter-1    = a 1em horizontal gutter between flex rows
    .fd-c.-column-gutter-1 = a 1em vertical gutter between flex columns

### Measurement

```css
/* 0 */
p-0 { padding: 0 }

/* px */
p-1p { padding: 1px }
p-2p { padding: 2px }
p-3p { padding: 3px }
p-4p { padding: 4px }

/* em */
p-025 { padding: .25em }
p-05  { padding: .5em }
p-1   { padding: 1em }
p-2   { padding: 2em }

/* rem */
p-025r { padding: .25em }
p-05r  { padding: .5em }
p-1r   { padding: 1em }
p-2r   { padding: 2em }
```

## Padding

Padding has a few extra rules. It does so to minimize the need for negative margin. I really hate negative margin.

Where `_` is used as a delimiter, all rules are applied to direct children.

Those children can override these rules with a `!` suffix. This makes it clear when a child is acting outside of it's parents requests. Enough bangs and it might be time to change the parent's mind.

```html

<div class="p_0">
  <div> all direct children have 0 padding </div>
</div>

<div class="p_1">
  <div> all direct children have 1em padding </div>
</div>

<div class="p_1p">
  <div> all direct children have 1px padding </div>
</div>

<div class="p_1r">
  <div> all direct children have 1rem padding </div>
</div>
```

The inner elements can force their padding rules to override their parents:

```html
<div class="p_1">
  <div> all direct children have 1em padding </div>
  <div> this one two </div>
  <div class="p-0!"> this one has reset to 0 </div>
</div>
```

## Media Queries

The primary advantage that these classes provide over inline-styles is there ability to leverage media-queries.

Breakpoint-specific classes are suffixed with an `@`-symbol, followed by the two-character shorthand for the breakpoint (i.e., `mn`, `sm`, `md`, `lg`, `xl`).

For example, this element becomes darker at every available breakpoint.

```html
<p class="c-l c-l.8@sm c-l.5@md c-l.2@lg c-l.0@xl">
  The wider I get, the darker I become
</p>
```

### Breakpoints

I've used the breakpoints from [Bootstrap](http://getbootstrap.com) because it's the most popular CSS framework on the planet. If you dont like them, feel free to modify that single line in your local copy.

    xl >=1200
    lg >=992
    md >=768
    sm >=544

I've added two additional breakpoints:

    xs >=320
    mn >=0   (min)
