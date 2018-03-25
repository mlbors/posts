# An overview of Sass Maps #

In this article, we are going to have an overview of _Sass Maps_.

## Introduction ##

_Sass_ brought a lot of interesting features that help us to create _style sheets_ more easily and more elegantly. One of those features are the _Maps_. Let's see what they are.

## Definition ##

Basically, _Maps_ are like _associative arrays_. A _Map_ has a name and one or more unique _key_ associated with one value. A _Sass Map_ looks like this:

    $map: (
        key: value,
        another-key: another-value
    );

## Examples ##

So, let's declare a _Map_ to make a few examples:

    $colors: (
        red: #cc3120,
        green: #3fcc41,
        blue: #4286f4
    );

### Accessing a value ###

We can access a value like so:

    .element {
        background-color: map-get($colors, red);
    }

Here, we use the function "_map-get()_" that takes, as arguments, the name of the _Map_ and the _key_ we want to target.

### Checking a value ###

We can check if a value exists like so:

    .element {
        @if map-has-key($colors, orange) {
            background-color: map-get($colors, orange);
        } @else {
            background-color: $default;
        }
    }

So, knowing that, it is easy to create such a function:

    @function color($key) {
        @if map-has-key($colors, $key) {
            @return map-get($colors, $key);
        }

        @warn "Unknown `#{$key}` in $colors.";
        @return null;
    }

    .element {
        background-color: color(red);
    }

### Nested Maps ###

Let's go a little further and imagine something like this:

    $colors: (
        red: (
            color: #f4cfcb,
            background: #cc3120
        ),
        green: (
            color: #b8d6b8,
            background: #3fcc41
        ),
        blue: (
            color: #bacff2,
            background: #4286f4
        )
    );

Here, we have a nested _Map_. We can use it like so:

    @function color($color, $attribute) {
        @return map-get(map-get($colors, $color), $attribute);
    }

    .element {
        color: color(green, color);
    }

### Creating loops ###

With _Maps_, we can easily create loops like so:

    // Map
    $sections:(
        'red-section': (
            'background':         #cc3120,
            'color':              #ffffff
        ),
        'green-section':(
            'background':         #3fcc41,
            'color':              #ffffff
        )
    );

    // Function
    @function color($map, $section, $attribute) {
        @if map-has-key($map, $section) {
            @return map-get(map-get($map, $section), $attribute);
        }
        @warn "The key ´#{$section} is not available in the map.";
        @return null;
    }

    // Loop
    @each $key, $val in $sections {
        @if map-has-key($sections, $key) {
            .#{$key} {
                background-color: color($sections, $key, background);
                color: color($sections, $key, color);
            }
        }
    }

## Conclusion ##

Through this brief article, we saw what _Sass Maps_ are. We had a few examples of how to use them and where they could be helpful.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!
