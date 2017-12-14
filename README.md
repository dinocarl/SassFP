# SassFP

A set of utilities inspired by [Ramda][ramda], [lodash FP][lodashfp], and other Functional libraries for Sass.

Like Ramda, all the functions in SassFP are iteratee-first, data-last. Some native Sass methods have been re-supplied here with that argument order, but, so far, only those that I've personally needed in projects.

## Available Methods

### String Methods
#### `prefixStr($prefix, $str)`

Returns **`$str`** prefixed with **`$prefix`**.

```scss
prefixStr('selector', 'one'); // => 'selectorone'
```

#### `suffixStr($suffix, $str)`

Returns **`$str`** with **`$suffix`** appended to it

```scss
suffixStr('selector', 'one'); // => 'oneselector'
```

#### `explode($separator, $str)`

Converts a string to a list by splitting on $separator

```scss
explode('-', 'selector-one'); // => ('selector', 'one')
```

### List Methods
#### `implode($glue: '', $list: ())`

Returns a string where all the members of **`$list`** have been concatenated together with **`$glue`** between them.

```scss
implode('-', ('selector', 'one')); // => 'selector-one'
```

#### `repeat-into-list($times, $item)`

Returns a list where **`$item`** is represented **`$times`** times

```scss
repeat-into-list(3, 10); // => (10, 10, 10)
```

#### `slice($start, $end, $list)`

Returns **`$list`**'s members beginning at position **`$start`** and ending at **`$end`**.

```scss
slice(3, 5, ('alex', 'billy', 'charlie', 'dani', 'elliot')); // => ('charlie', 'dani', 'elliot')
```

#### `head($list)`

Returns the first member of **`$list`**.

```scss
head(('alex' 'billy' 'charlie' 'dani' 'elliot')); // => 'alex'
```

#### `tail($list)`

Returns all but the first member of **`$list`**.

```scss
tail(('alex' 'billy' 'charlie' 'dani' 'elliot')); // => 'billy' 'charlie' 'dani' 'elliot'
```

#### `init($list)`

Returns all but the last member of **`$list`**.

```scss
init(('alex' 'billy' 'charlie' 'dani' 'elliot')); // => 'alex' 'billy' 'charlie' 'dani'
```

#### `last($list)`

Returns the last member of **`$list`**.

```scss
last(('alex' 'billy' 'charlie' 'dani' 'elliot')); // => 'elliot'
```

#### `flatten($list...)`

Returns a flattened version of **`$list`**.

```scss
flatten(#fff, red, (#222, #333)); // => (#fff, red, #222, #333)
```

### Functional Methods
#### `map($fn, $list)`

Returns a new list where each member of **`$list`** has had function **`$fn`** run against it.

```scss
@function darkenbyten($color) {
  @return darken($color, 10%);
}
map(darkenbyten, (#fff, red, #222, #333)); // => #e6e6e6 #cc0000 #090909 #1a1a1a
```

#### `filter($predicate, $list)`

Returns a new list where **`$predicate`** returns **`true`** for members of **`$list`**.

```scss
@function gt5($val) {
  @return $val > 5;
}
filter(gt5, (4,5,6,7)); // => (6, 7)
```

#### `reduce($fn, $initial, $list)`

Accumulates the result of running each member of **`$list`** through **`$fn`** starting with the **`$initial`** value and **`$list`**'s first member.

```scss
reduce(prefixStr, '.', ('alex', 'billy', 'charlie')); // => ".alexbillycharlie"
reduce(suffixStr, '', ('alex', 'billy', 'charlie')); // => "charliebillyalex"
reduce(add, 0, (4,5,6)); // => 15
```

#### `pipe($params...)`

Accepts a list of arguments where the last item is the initial data and the others are a sequence of functions to run. Returns the result of each of the functions being run on the successive results from first to last. The very last item must be the data being operated on. Functions being passed in with additional parameters take the form of sub-lists.

```scss
pipe(
  flatten,
  (map, darkenbyten),
  (#fff, red, (#222, #333))
); // => #e6e6e6 #cc0000 #090909 #1a1a1a
```

#### `compose($params...)`

Same as **`pipe`**, but functions run in reverse order. Initial data remains last argument.

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

### Object Methods
#### `prop($path, $map)`

Allows for getting at nested attributes in a Sass map. Uses dot syntax to get at nested attributes. Ensures a `null` return for any unrecognized paths.

```scss
$colors: (header:(one: #333, two: #444), footer: #666);
prop('header.two', $colors); // => #444
prop('footer', $colors); // => #666
prop('body', $colors); // => null
```

#### `assign($map1, $map2)`

Allows for merging deeply nested maps


### Mathematical Methods
#### `add($x, $y)`

Adds **`$y`** to **`$x`**.

```scss
add(10, 2); // => 12
```

#### `multiply($x, $y)`

Multiplies **`$x`** by **`$y`**.

```scss
multiply(10, 2); // => 20
```

#### `subtract($x, $y)`

Subtracts **`$y`** from **`$x`**.

```scss
subtract(10, 2); // => 8
```

#### `divide($x, $y)`

Divides **`$x`** by **`$y`**.

```scss
divide(10, 2); // => 5
```

#### `percent($x, $y)`

Returns **`$y`**'s percent of **`$x`**.

```scss
percent(10, 2); // => 50%
```

#### `double($x)`

Doubles **`$x`**.

```scss
double(10); // => 20
```

#### `square($x)`

Squares **`$x`**.

```scss
square(10); // => 100
```

#### `inc($x)`

Increments **`$x`**.

```scss
inc(10); // => 11
```

#### `dec($x)`

Decrements **`$x`**.

```scss
dec(10); // => 9
```

#### `sum($num-list...)`

Accepts a list of numbers and returns the sum of them.

```scss
sum(10, 5, 2); // => 17
```

#### `power($num: 1, $exponent: 1)`

Returns the total after multiplying **`$num`** **`$exponent`** times.

```scss
power(10, 2); // => 100
```

#### `to-decimal-places($num, $digits: 2)`

Returns **`$num`** to **`$digits`** number of significant digits dropping anything beyond it. **`$digits`** is 2 by default since Sass has a default of returning 3 significant digits.

```scss
to-decimal-places(10.129, 2); // => 10.12
```
### Misc Methods

#### `applyUnit($unit, $val)`

Appends **`$unit`** to **`$val`

```scss
applyUnit(px, 50); // => 50px
applyUnit(em, 50); // => 50em
```

#### `px($val)`

Shortcut function to apply `px` unit

```scss
px(50); // => 50px
```

#### `em($val)`

Shortcut function to apply `em` unit

```scss
em(50); // => 50em
```

#### `vw($val)`

Shortcut function to apply `vw` unit

```scss
vw(50); // => 50vw
```

#### `vh($val)`

Shortcut function to apply `vh` unit

```scss
vh(50); // => 50vh
```

#### `rem($val)`

Shortcut function to apply `rem` unit

```scss
rem(50); // => 50rem
```

### Argument-converted Sass Functions
#### `fpAppend($item, $list)`
#### `fpJoin($list2, $list1)`
#### `fpNth($item, $list)`

### Convenience Type Boolean Methods
#### `is_list($val)`

Returns whether **`$val`** is a list.

```scss
is_list((#fff, red, #222, #333)); // => true
is_list(#fff red #222 #333); // => true
is_list(#fff); // => false
```

#### `is_color($val)`

Returns whether **`$val`** is a color.

```scss
is_color(red); // => true
is_color('red'); // => false
```

#### `is_string($val)`

Returns whether **`$val`** is a string.

```scss
is_string('val'); // => true
is_string(false); // => false
```

#### `is_boolean($val)`

Returns whether **`$val`** is a boolean.

```scss
is_boolean(false); // => true
is_boolean('val'); // => false
```

#### `is_number($val)`

Returns whether **`$val`** is a number.

```scss
is_number(10); // => true
is_number('10'); // => false
```

#### `is_null($val)`

Returns whether **`$val`** is a null.

```scss
is_null(null); // => true
is_null(true); // => false
```

#### `is_map($val)`

Returns whether **`$val`** is a map.

```scss
is_map((header: red)); // => true
is_map((header red)); // => false
```


  [ramda]: http://ramdajs.com
  [lodashfp]: https://github.com/lodash/lodash/wiki/FP-Guide