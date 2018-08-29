# Fading out Text

A Sass mixin to fade out text to white.  
All credit goes to [@stephanpavlovic](https://github.com/stephanpavlovic)

```sass
@mixin text-fade-out($height: 50px, $color: $white, $bottom: 0){
  position: relative;
  &:after{
    content: '';
    height: $height;
    width: 100%;
    background: linear-gradient( 0deg, rgba($color, 1), rgba($color, 0));
    display: block;
    position: absolute;
    bottom: $bottom;
  }
}
```

## How to use

Since the mixin itself is positioned `absolute`, it must be placed in a non-static parent. For example:

```sass
.myTextWrapper
  position: relative
  // Probably some text styling
  
  &-displayMore
    +text-fade-out
    // ...
```
