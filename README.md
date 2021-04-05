# SassFP
#### v1.6.1

A set of utilities inspired by [Ramda][], [lodash FP][], and other Functional
libraries for Sass.

Like Ramda, all the functions in SassFP are iteratee-first, data-last. Some
native Sass methods have been re-supplied here with that argument order, but, so
far, only those that I've personally needed in projects.

### Installation

SassFP is a node-module and can be installed via

```bash
npm install sassfp
```

### Requirements

SassFP requires [Sass 3.3][] or greater due to its reliance on and ability to
manipulate [Sass maps][]. As of **`v1.3.1`**, it is compatible with [Sass 3.5][].

- [String Methods](#string-methods)
    - [prefixStr](#prefixstr)
    - [suffixStr](#suffixstr)
    - [explode](#explode)
    - [pathToMap](#pathtomap)
- [List Methods](#list-methods)
    - [implode](#implode)
    - [repeat](#repeat)
    - [slice](#slice)
    - [head](#head)
    - [tail](#tail)
    - [init](#init)
    - [last](#last)
    - [flatten](#flatten)
    - [partition](#partition)
    - [contains](#contains)
    - [intersection](#intersection)
    - [difference](#difference)
- [Functional Methods](#functional-methods)
    - [always](#always)
    - [identity](#identity)
    - [T](#T)
    - [F](#F)
    - [map](#map)
    - [map-with-index](#map-with-index)
    - [filter](#filter)
    - [reject](#reject)
    - [reduce](#reduce)
    - [pipe](#pipe)
    - [compose](#compose)
    - [cond](#cond)
- [Object Methods](#object-methods)
    - [path](#path)
    - [prop](#prop)
    - [pathOr](#pathor)
    - [propOr](#propor)
    - [assign](#assign)
    - [pick](#pick)
    - [omit](#omit)
    - [listPaths](#listPaths)
- [Mathematical Methods](#mathematical-methods)
    - [add](#add)
    - [multiply](#multiply)
    - [subtract](#subtract)
    - [divide](#divide)
    - [percent](#percent)
    - [double](#double)
    - [square](#square)
    - [inc](#inc)
    - [dec](#dec)
    - [sum](#sum)
    - [power](#power)
    - [to-decimal-places](#to-decimal-places)
- [Misc Methods](#misc-methods)
    - [applyUnit](#applyunit)
    - [px](#px)
    - [em](#em)
    - [vw](#vw)
    - [vh](#vh)
    - [rem](#rem)
- [Argument-converted Sass Functions](#argument-converted-sass-functions)
    - [fpAppend](#fpappend)
    - [fpJoin](#fpjoin)
    - [fpNth](#fpnth)
- [Convenience Type Boolean Methods](#convenience-type-boolean-methods)
    - [is_list](#is_list)
    - [is_color](#is_color)
    - [is_string](#is_string)
    - [is_boolean](#is_boolean)
    - [is_number](#is_number)
    - [is_null](#is_null)
    - [is_map](#is_map)
    - [isnt_list](#isnt_list)
    - [isnt_color](#isnt_color)
    - [isnt_string](#isnt_string)
    - [isnt_boolean](#isnt_boolean)
    - [isnt_number](#isnt_number)
    - [isnt_null](#isnt_null)
    - [isnt_map](#isnt_map)
- [Relational Methods](#relational-methods)
    - [equals](#equals)
    - [eq](#eq)
    - [lt](#lt)
    - [lte](#lte)
    - [gt](#gt)
    - [gte](#gte)

## String Methods
### prefixStr
`($prefix, $str)`

Returns **`$str`** prefixed with **`$prefix`**.

```scss
prefixStr('selector', 'one'); // => 'selectorone'
```

### suffixStr
`($suffix, $str)`

Returns **`$str`** with **`$suffix`** appended to it.

```scss
suffixStr('selector', 'one'); // => 'oneselector'
```

### explode
`($separator, $str)`

Converts a string to a list by splitting on $separator.

```scss
explode('-', 'selector-one'); // => ('selector', 'one')
```

### pathToMap
`($path, $val)`

Converts a dot-delimited string and any value into a Sass map with that same
object heirarchy.

```scss
pathToMap('x', 10px); => (x: 10px)
pathToMap('x.y', 10px); => (x: (y: 10px))
pathToMap('x.y.z', 10px); => (x: (y: (z: 10px)))
```


## List Methods
### implode
`($glue: '', $list: ())`

Returns a string where all the members of **`$list`** have been concatenated
together with **`$glue`** between them.

```scss
implode('-', ('selector', 'one')); // => 'selector-one'
```

### repeat
`($times, $item)`

Returns a list where **`$item`** is represented **`$times`** times

```scss
repeat(3, 10); // => (10, 10, 10)
```

### slice
`($start, $end, $list)`

Returns **`$list`**'s members beginning at position **`$start`** and ending at
**`$end`**.

```scss
slice(3, 5, ('alex', 'billy', 'charlie', 'dani', 'elliot')); // => ('charlie', 'dani', 'elliot')
```

### head
`($list)`

Returns the first member of **`$list`**.

```scss
head(('alex' 'billy' 'charlie' 'dani' 'elliot')); // => 'alex'
```

### tail
`($list)`

Returns all but the first member of **`$list`**.

```scss
tail(('alex' 'billy' 'charlie' 'dani' 'elliot')); // => 'billy' 'charlie' 'dani' 'elliot'
```

### init
`($list)`

Returns all but the last member of **`$list`**.

```scss
init(('alex' 'billy' 'charlie' 'dani' 'elliot')); // => 'alex' 'billy' 'charlie' 'dani'
```

### last
`($list)`

Returns the last member of **`$list`**.

```scss
last(('alex' 'billy' 'charlie' 'dani' 'elliot')); // => 'elliot'
```

### flatten
`($list...)`

Returns a flattened version of **`$list`**.

```scss
flatten(#fff, red, (#222, #333)); // => (#fff, red, #222, #333)
```

### partition
`($predicate, $list)`

Returns 2-dimensional list where the first member contains all the members of
**`$list`** for which **`$predicate`** is **`true`**, and the second, all those
for which it is **`false`**.

```scss
partition(gt5, (4,5,6,7)); // => ((6 7), (4 5))
partition(gt5, (0,1,2,3)); // => ((), (0 1 2 3))
partition(gt5, (6,7,8,9)); // => ((6 7 8 9), ())
```

### contains
`($item, $list)`

Returns a Boolean whether **`$item`** is in **`$list`**.

```scss
$strlist: ('alex' 'billy' 'charlie' 'dani' 'elliot');
contains('alex', $strlist); => true
contains('allen', $strlist); => false
contains('billy', $strlist); => true
```

### intersection
`($list1, $list2)`

Returns a new list of what both **`$list1`** and **`$list2`** have in common.
Inverse of **`difference`**.

```scss
$strlist: ('alex' 'billy' 'charlie' 'dani' 'elliot');
intersection(('alex', 'billy', 'allen'), $strlist); // // => ('alex' 'billy')
intersection(('alex.a', 'billy', 'allen'), $strlist); // // => ('billy')
intersection(('alex.a', 'allen'), $strlist); // // => ()
```

### difference
`($list1, $list2)`

Returns new list of what **`$list1`** and **`$list2`** do not have in common.
Inverse of **`intersection`**.

```scss
$strlist: ('alex' 'billy' 'charlie' 'dani' 'elliot');
difference(('alex' 'billy'), $strlist); // => ()
difference(('alex' 'billy' 'allen'), $strlist); // => ('allen')
difference(('alex.a' 'billy' 'allen'), $strlist); // => ('alex.a' 'allen')
```

## Functional Methods
### always
`($val, `_`$b`_`)`

Returns the first value passed to it. As of **`v1.5.0`**, this accepts a second
optional parameter, _`$b`_, to not only better align with Ramda's signature, but
to also allow it to participate in compositional flows.  **`identity`** ought to
be substituted for any previous usage of **`always`**, though there are no plans
to deprecate the single-argument functionality.

```scss
always(1)  // => 1
always('asdf')  // => "asdf"
always(('x', 'y', 'z'))  // => ("x", "y", "z")
always('x', 'y')  // => "x"
always(true, false)  // => true
always(false, true)  // => false
```

### identity
`($val)`

Returns the value passed to it.

```scss
identity(1)  // => 1
identity('asdf')  // => "asdf"
identity(('x', 'y', 'z'))  // => ("x", "y", "z")
identity(true)  // => true
identity(false)  // => false
```
### T
`($val...)`

Returns `true`, regardless of what arguments are passed or how many there are.

```scss
T(1)  // => true
T('asdf')  // => true
T(('x', 'y', 'z'))  // => true
T('x', 'y', 'z')  // => true
T(true)  // => true
T(false)  // => true
T()  // => true
```
### F
`($val...)`

Returns `false`, regardless of what arguments are passed or how many there are.

```scss
F(1)  // => false
F('asdf')  // => false
F(('x', 'y', 'z'))  // => false
F('x', 'y', 'z')  // => false
F(true)  // => false
F(false)  // => false
F()  // => false
```

### map
`($fn, $list)`

Returns a new list where each member of **`$list`** has had function **`$fn`**
run against it.

```scss
@function darkenbyten($color) {
  @return darken($color, 10%);
}
map(darkenbyten, (#fff, red, #222, #333)); // => #e6e6e6 #cc0000 #090909 #1a1a1a
```

As of **`v1.2.0`**, **`$fn`** may itself be a list where the first member is the
function to be run against each member of **`$list`**, and the others are extra
arguments that the function would require, in effect allowing functions to be
decorated.

```scss
@function darkenby($pct, $color) {
  @return darken($color, $pct);
}
map((darkenby, 10%), (#fff, red, #222, #333)); // => #e6e6e6 #cc0000 #090909 #1a1a1a
```

Because Sass supports space-separated lists, the commas are optional, and
omitting them may increase legibility.

```scss
map((darkenby 10%), (#fff, red, #222, #333)); // => #e6e6e6 #cc0000 #090909 #1a1a1a
```

Most important, however, is that the value(s) provided by iterating over
**`$list` must always be in the last argument position `$fn` expects**.

### map-with-index
`($fn, $list)`

Returns a new list where each member of **`$list`** has had function **`$fn`**
run against it and its index as two arguments. Works identically to [map](#map),
but with the item's index passed as an additional argument to **`$fn`** in the
last argument position and the value provided by **`$list`** in the
second-to-last.
```scss
map-with-index(prefixStr, ("alex", "billy", "charlie")); // => ("alex1", "billy2", "charlie3")
map-with-index(add, (4, 5, 6)); // => (5, 7, 9)
```

### filter
`($predicate, $list)`

Returns a new list where **`$predicate`** returns **`true`** for members of
**`$list`**. Inverse of **`reject`**.

```scss
@function gt5($val) {
  @return $val > 5;
}
filter(gt5, (4,5,6,7)); // => (6, 7)
```

### reject
`($predicate, $list)`

Returns a new list where **`$predicate`** returns **`false`** for members of
**`$list`**. Inverse of **`filter`**.

```scss
reject(gt5, (4,5,6,7)); // => (4, 5)
```

### reduce
`($fn, $initial, $list)`

Accumulates the result of running each member of **`$list`** through **`$fn`**
starting with the **`$initial`** value and **`$list`**'s first member.

```scss
reduce(prefixStr, '.', ('alex', 'billy', 'charlie')); // => ".alexbillycharlie"
reduce(suffixStr, '', ('alex', 'billy', 'charlie')); // => "charliebillyalex"
reduce(add, 0, (4,5,6)); // => 15
```

As of **`v1.4.0`**, **`$fn`** may itself be a list where the first member is the
function to be run against each member of **`$list`**, and the others are extra
arguments that the function would require, in effect allowing functions to be
decorated.

```scss
reduce((sum, 1, 2), 0 (4, 5, 6)); // => 24
```

Most important, however, is that the value(s) provided by iterating over
**`$list` must always be in the last argument position `$fn` expects and the
accumulator in the second-to-last**. So, in the example above, the execution
goes like this:

```
  function  add'l values  accumulator   member    outcome
     ⬇︎       ⬇︎   ⬇︎           ⬇︎         ⬇︎         ⬇︎
    sum(      1,   2,          0,         4 )   =>   7
    sum(      1,   2,          7,         5 )   =>   15
    sum(      1,   2,          15,        6 )   =>   24
```

In the particular case of **`sum`**, argument order isn't important, but for others it could very well be.

### pipe
`($params...)`

Accepts a list of arguments where the last item is the initial data and the
others are a sequence of functions to run. Returns the result of each of the
functions being run on the successive results from first to last. **The very
last item must be the data being operated on.** Functions being passed in with
additional parameters take the form of sub-lists.

```scss
pipe(
  flatten,
  (map, darkenbyten),
  (#fff, red, (#222, #333))
); // => #e6e6e6 #cc0000 #090909 #1a1a1a
```

### compose
`($params...)`

Same as **`pipe`**, but functions run in reverse order. Initial data remains
last argument.

```scss
compose(
  unquote,
  (prefixStr, '.'),
  (implode, '-'),
  (join, ('d', 'e')),
  ('a', 'b', 'c')
); // => .d-e-a-b-c

compose(
  double,
  (reduce, add, 0),
  (map, square),
  (4,5,6)
); // => 154
```

### cond
`(($predicates-and-actions-list, $data))`

Accepts a two dimensional list of predicate and transformation functions that,
taken together, are read as a series of nested `if / else` statements, where the
first `true` value encountered has its transformation executed against the
supplied data. Like **`pipe`** and **`compose`**, the data comes in the last
position.

The assumption of both the predicate and transformation functions is that they
are unary functions where the provided data serves as the parameter to both
functions. Alternatively, n-ary functions may also be used in either assuming
that the passed value is coming in the functions' last argument positions, and
all other arguments are provided by as a list.

To ensure a return, use the **`T`** function in the last position of
predicate-transformation pairs (so, second-to-last in overall list), coupled
with an **`always`**. As noted with **`map`**, omitting commas in the inner
lists may help with legibility.

```scss
@function darkenby($pct, $color) { @return darken($color, $pct); }
@function lightenby($pct, $color) { @return lighten($color, $pct); }

cond((
  ((equals yellow), (lightenby 20))
  ((equals red), (darkenby 10))
  (T, (always #000))
  red
)) // => #cc0000
```

## Object Methods

### path
`($key-list, $map)`

Allows for getting at nested attributes in a Sass map using list syntax. Ensures
a null return for any unrecognized paths.

```scss
$colors: (header:(one: #333, two: #444), footer: #666);
path(('header', 'two'), $colors); // => #444
path(('footer'), $colors); // => #666
path(('header', 'two', 'three', 'four'), $colors); // => null
```

### prop
`($path, $map)`

Allows for getting at nested attributes in a Sass map using dot syntax. Ensures
a null return for any unrecognized paths.

```scss
$colors: (header:(one: #333, two: #444), footer: #666);
prop('header.two', $colors); // => #444
prop('footer', $colors); // => #666
prop('body', $colors); // => null
```

### pathOr
`($fallback, $key-list, $map)`

Returns the value of a `path` lookup when successful, and returns a provided
fallback when not.

```scss
$colors: (header:(one: #333, two: #444), footer: #666);
pathOr(':(', ('header', 'two'), $colors); // => #444
pathOr(':(', ('body'), $colors); // => ':('
pathOr(':(', ('header', 'two', 'three', 'four'), $colors); // => ':('
```

### propOr
`($fallback, $path, $map)`

Returns the value of a `prop` lookup when successful, and returns a provided
fallback when not.

```scss
$colors: (header:(one: #333, two: #444), footer: #666);
propOr(':(', 'header.two', $colors); // => #444
propOr(':(', 'footer', $colors); // => #666
propOr(':(', 'body', $colors); // => ':('
```

### assign
`($map1, $map2)`

Merges 2 deeply-nested map objects.

```scss
$colors: (header:(one: #333, two: #444), footer: #666);
assign($colors, (header: (one: red))); // => (header:(one: red, two: #444), footer: #666)
assign($colors, (header: (three: red))); // => (header:(one: #333, two: #444, three: red), footer: #666)
```

### pick
`($key-list, $map)`

Creates new map object from **`$map`** selecting keys provided by
**`$key-list`**.

```scss
$colors: (header:(one: #333, two: #444), footer: #666);
pick('footer', $colors); // => (footer: #666)
pick('header.one', $colors); // => (header:(one: #333))
pick(('header', 'footer'), $colors); // same as original
```

### omit
`($key-list, $map)`

Creates new map object from the provided one, then removing a specified list of
keys.

```scss
omit(
  ('header.one', 'header.three.four'),
  (header: (
    one: #333,
    two: #444,
    three: (
      four: red,
      five: blue
    )
  ),
  footer: #666
));
// =>
//   (header: (
//     two: #444,
//     three: (
//       five: blue
//     )
//   ),
//   footer: #666
// )
```

### listPaths
`($map)`

Creates a flat, dot-delimited list of all the paths of **`$map`**.

```scss
$colors: (header:(one: #333, two: #444), footer: #666);
listPaths($colors); // => ("header" "header.one" "header.two" "footer")
```



## Mathematical Methods
### add
`($x, $y)`

Adds **`$y`** to **`$x`**.

```scss
add(10, 2); // => 12
```

### multiply
`($x, $y)`

Multiplies **`$x`** by **`$y`**.

```scss
multiply(10, 2); // => 20
```

### subtract
`($x, $y)`

Subtracts **`$y`** from **`$x`**.

```scss
subtract(10, 2); // => 8
```

### divide
`($x, $y)`

Divides **`$x`** by **`$y`**.

```scss
divide(10, 2); // => 5
```

### percent
`($x, $y)`

Returns **`$x`**'s percent of **`$y`**.

```scss
percent(2, 10); // => 50%
```

### double
`($x)`

Doubles **`$x`**.

```scss
double(10); // => 20
```

### square
`($x)`

Squares **`$x`**.

```scss
square(10); // => 100
```

### inc
`($x)`

Increments **`$x`**.

```scss
inc(10); // => 11
```

### dec
`($x)`

Decrements **`$x`**.

```scss
dec(10); // => 9
```

### sum
`($num-list...)`

Accepts a list of numbers and returns the sum of them.

```scss
sum(10, 5, 2); // => 17
```

### power
`($exponent: 1, $num: 1)`

Returns the total after multiplying **`$num`** **`$exponent`** times.

```scss
power(2, 10); // => 100 (10^2)
power(10, 2); // => 1024 (2^10)
```

### to-decimal-places
`($digits: 2, $num: 1)`

Returns **`$num`** to **`$digits`** number of significant digits dropping
anything beyond it. **`$digits`** is 2 by default since Sass has a default of
returning 3 significant digits.

```scss
to-decimal-places(2, 10.129); // => 10.12
```


## Misc Methods
### applyUnit
`($unit, $val)`

Appends **`$unit`** to **`$val`**.

```scss
applyUnit(px, 50); // => 50px
applyUnit(em, 50); // => 50em
```

### px
`($val)`

Shortcut function to apply **`px`** unit.

```scss
px(50); // => 50px
```

### em
`($val)`

Shortcut function to apply **`em`** unit.

```scss
em(50); // => 50em
```

### vw
`($val)`

Shortcut function to apply **`vw`** unit.

```scss
vw(50); // => 50vw
```

### vh
`($val)`

Shortcut function to apply **`vh`** unit.

```scss
vh(50); // => 50vh
```

### rem
`($val)`

Shortcut function to apply **`rem`** unit.

```scss
rem(50); // => 50rem
```

## Argument-converted Sass Functions

Existing Sass functions with data-last argument orders.

### fpAppend
`($item, $list)`

Adds an item to the end of a provided list. See Sass [append][] documentation.

```scss
fpAppend('charlie', ('alex', 'billy')); // => 'alex' 'billy' 'charlie'
```

### fpJoin
`($list2, $list1)`

Joins **`$list2`** to the end of **`$list1`**. See Sass [join][] documentation.

```scss
fpJoin(('charlie' 'dani'), ('alex', 'billy')); // => 'alex' 'billy' 'charlie' 'dani'
```

### fpNth
`($position, $list)`

Returns the item at **`$position`** from **`$list`**. See Sass [nth][] documentation.

```scss
fpNth(2, ('alex', 'billy')); // => 'billy'
```


## Convenience Type Boolean Methods
### is_list
`($val)`

Returns whether **`$val`** is a list.

```scss
is_list((#fff, red, #222, #333)); // => true
is_list(#fff red #222 #333); // => true
is_list(#fff); // => false
```

### is_color
`($val)`

Returns whether **`$val`** is a color.

```scss
is_color(red); // => true
is_color('red'); // => false
```

### is_string
`($val)`

Returns whether **`$val`** is a string.

```scss
is_string('val'); // => true
is_string(false); // => false
```

### is_boolean
`($val)`

Returns whether **`$val`** is a boolean.

```scss
is_boolean(false); // => true
is_boolean('val'); // => false
```

### is_number
`($val)`

Returns whether **`$val`** is a number.

```scss
is_number(10); // => true
is_number('10'); // => false
```

### is_null
`($val)`

Returns whether **`$val`** is a null.

```scss
is_null(null); // => true
is_null(true); // => false
```

### is_map
`($val)`

Returns whether **`$val`** is a map.

```scss
is_map((header: red)); // => true
is_map((header red)); // => false
```

### isnt_list
`($val)`

Returns whether **`$val`** is not a list.

```scss
isnt_list((#fff, red, #222, #333)); // => false
isnt_list(#fff red #222 #333); // => false
isnt_list(#fff); // => true
```

### isnt_color
`($val)`

Returns whether **`$val`** is not a color.

```scss
isnt_color(red); // => false
isnt_color('red'); // => true
```

### isnt_string
`($val)`

Returns whether **`$val`** is not a string.

```scss
isnt_string('val'); // => false
isnt_string(false); // => true
```

### isnt_boolean
`($val)`

Returns whether **`$val`** is not a boolean.

```scss
isnt_boolean(false); // => false
isnt_boolean('val'); // => true
```

### isnt_number
`($val)`

Returns whether **`$val`** is not a number.

```scss
isnt_number(10); // => false
isnt_number('10'); // => true
```

### isnt_null
`($val)`

Returns whether **`$val`** is not a null.

```scss
isnt_null(null); // => false
isnt_null(true); // => true
```

### isnt_map
`($val)`

Returns whether **`$val`** is not a map.

```scss
isnt_map((header: red)); // => false
isnt_map((header red)); // => true
```

## Relational
### equals
`($a, $b)`

Returns whether **`$a`** and **`$b`** are equivalent (function version of Sass
`==` operator).

```scss
equals(1, 1); // => true
equals(1, '1'); // => false
equals((header: red), (header: red)); // => true
equals((header red), (header red)); // => true
```

### eq
`($a, $b)`

Returns whether **`$a`** and **`$b`** are equivalent (alias of `equals`).


### lt
`($a, $b)`

Returns whether the first argument is less than the second (function version of
Sass `<` operator).

```scss
lt(1, 10) // => true
lt(10, 1) // => false
lt(1, 1) // => false
```

### lte
`($a, $b)`

Returns whether the first argument is less than or equal to the second (function
version of Sass `<=` operator).

```scss
lte(1, 10) // => true
lte(10, 1) // => false
lte(1, 1) // => true
```

### gt
`($a, $b)`

Returns whether the first argument is greater than the second (function version
of Sass `>` operator).

```scss
gt(1, 10) // => false
gt(10, 1) // => true
gt(1, 1) // => false
```

### gte
`($a, $b)`

Returns whether the first argument is greater than or equal to the second
(function version of Sass `>=` operator).

```scss
gte(1, 10) // => false
gte(10, 1) // => true
gte(1, 1) // => true
```


  [Ramda]: http://ramdajs.com
  [lodash FP]: https://github.com/lodash/lodash/wiki/FP-Guide
  [Sass 3.3]: http://thesassway.com/news/sass-3-3-released#maps
  [Sass maps]: http://sass-lang.com/documentation/file.SASS_REFERENCE.html#maps
  [Sass 3.5]: https://oddbird.net/2017/03/30/safe-get/
  [append]: http://sass-lang.com/documentation/Sass/Script/Functions.html#append-instance_method
  [join]: http://sass-lang.com/documentation/Sass/Script/Functions.html#join-instance_method
  [nth]: http://sass-lang.com/documentation/Sass/Script/Functions.html#nth-instance_method
