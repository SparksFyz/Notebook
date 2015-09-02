## Sass Snippets

>     use function and mixin for automation (color & font & media)

### color

```sass
$color-stack:
    (group: orange, id: normal, color: #e67835),
    (group: orange, id: pale, color: #f8a878),
    (group: orange, id: dark, color: #ad490c),
    (group: blue, id: normal, color: #426682);

// Color  Function
@function color($group, $shade:normal, $transparency:1){
  @each $color in $color-stack{
    $c-group: map-get($color, group);
    $c-shade: map-get($color, id);
    @if($group == map-get($color, group) and $shade == map-get($color, id)){
      @return rgba(map-get($color, color), $transparency);
    }
  }
}
```
- invoking :

```css
  body{
    color: color(blue, normal,.8);
  }
  p{
    color: color(orange);
  }
  blockquote{
    color: color(blue);
    background: color(orange, pale,.4);
    border-color: color(orange, dark);
  }
```
- effect :

```css
body {
  color: rgba(66, 102, 130, 0.8);
}
p {
  color: #e67835;
}
blockquote {
  color: #426682;
  background: rgba(248, 168, 120, 0.4);
  border-color: #ad490c;
}
```

-------------------------------------------------------------------------------------------------------------

### Font

```sass
$font-stack:
  (group: brandon, id: light, font: ('Brandon Grot W01 Light', san-serif ), weight: 200, style: normal),
  (group: brandon, id: light-italic, font: ('Brandon Grot W01 Light', san-serif ), weight: 200, style: italic),
  (group: brandon, id: regular, font: ('Brandon Grot W01-Regular', san-serif), weight: 400, style: normal),
  (group: brandon, id: regular-italic, font: ('Brandon Grot W01-Regular', san-serif), weight: 400, style: italic),
  (group: brandon, id: bold, font: ('Brandon Grot W01 Black', san-serif), weight: 700, style: normal),
  (group: brandon, id: bold-italic, font: ('Brandon Grot W01-Regular', san-serif), weight: 400, style: italic),
  (group: clarendon, id: regular, font: ('Clarendon LT W01', serif), weight: 200, style: normal),
  (group: code, id: regular, font: (monospace), weight: 400, style: normal);

// Breakpoint Mixin
@mixin font($group, $id:regular){
  @each $font in $font-stack{
    @if($group == map-get($font, group) and $id == map-get($font, id)){
      font-family: map-get($font, font);
      font-weight: map-get($font, weight);
      font-style: map-get($font, style);
    }
  }
}
```
- invoking :

```css
h1{
  @include font(brandon, light-italic);
}
p{
  @include font(brandon);
}
p i{
  @include font(brandon, regular-italic);
}
p b{
  @include font(brandon, bold);
}
blockquote{
  @include font(clarendon);
}
code{
  @include font(code);
}
```

- effect :

```css
h1 {
  font-family: "Brandon Grot W01 Light", san-serif;
  font-weight: 200;
  font-style: italic;
}

p {
  font-family: "Brandon Grot W01-Regular", san-serif;
  font-weight: 400;
  font-style: normal;
}

p i {
  font-family: "Brandon Grot W01-Regular", san-serif;
  font-weight: 400;
  font-style: italic;
}

p b {
  font-family: "Brandon Grot W01 Black", san-serif;
  font-weight: 700;
  font-style: normal;
}

blockquote {
  font-family: "Clarendon LT W01", serif;
  font-weight: 200;
  font-style: normal;
}

code {
  font-family: monospace;
  font-weight: 400;
  font-style: normal;
}
```

-------------------------------------------------------------------------------------------------------------

### Media

- sass :

```sass
$media-stack:
  (group: tablet, id: general, rule: "only screen and (min-device-width: 700px)"),
  (group: small, id: general, rule: "only screen and(min-device-width: 1100px)"),
  (group: small, id: inbetween, rule: "only screen and(min-device-width: 1100px) and (max-device-width: 1400px)"),
  (group: large, id: general, rule: "only screen and(min-device-width: 1400px)"),
  (group: print, id: general, rule: "only print"),
  (group: custom, id: screen, rule: "only screen and");

@mixin media($group, $id: general, $customRule: ""){
  @each $media in $media-stack{
    @if($group == map-get($media, group) and $id == map-get($media, id)){
      $rule: map-get($media, rule);
      @media #{$rule} #{$customRule} {@content}
    }
  }
}
h1{
  color: #333;
  @include media(tablet){
    font-size: 1rem;
  };
  @include media(small, inbetween){
    font-size: 1.2rem;
  };
  @include media(small){
    color: #000;
  };
  @include media(custom, screen, " (max-device-width: 360px)"){
    color: blue;
  };
}
```

- css effect :

```css
h1 {
  color: #333; }
  @media only screen and (min-device-width: 700px) {
    h1 {
      font-size: 1rem; } }
  @media only screen and (min-device-width: 1100px) and (max-device-width: 1400px) {
    h1 {
      font-size: 1.2rem; } }
  @media only screen and (min-device-width: 1100px) {
    h1 {
      color: #000; } }
  @media only screen and (max-device-width: 360px) {
    h1 {
      color: blue; } }

```