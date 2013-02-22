# Style Guide for PHP Code

### The Daily Californian

---

##  Paradigms and Principles

 - **Don't repeat yourself (DRY)**. In theme code, avoid copying and pasting code by generating statements procedurally or by adding functions to `functions.php`. Also, use the [Wordpress built-in functions](http://codex.wordpress.org/Function_Reference/) where possible.
 - **Humans first, then machines**. Clever one-liners like `((empty($foo))?'':$bar)` may be more concise, but they are also more difficult to understand for future maintainers than an expanded conditional.
 - **The global namespace is sacred**. Utilize PHP Classes liberally where they make sense, especially in plugins and scripts that are loaded onto the global Wordpress namespace.
 - **Respect abstraction barriers**. Modifications to Wordpress behavior should be made with [built-in Wordpress hooks](http://codex.wordpress.org/Plugin_API#Hooks.2C_Actions_and_Filters): actions and filters.

##  Code Style

### Tags, Short Tags, and End Tags

 - No short tags. (`<?` instead of `<?php`)
 - Avoid end tags at the end of the file where possible.

### Comments

The PHP in `main-theme` almost always uses `//` over `#`. However, **both `//` and `#` are satisfactory and valid for one-liners**.

Multi-line comments start with `/**` and end with `*/`.

In `functions.php`, annotations like the example below are sometimes used to keep track of a function's history, but this functionality is now also provided by GitHub versioning.

```php
/**
 * Register and enqueue various Javascript files
 *
 * @since Daily Cal 1.5
 */
```

Read more about multi-line comment style on the [inline documentation page on the Zend framework docs](http://framework.zend.com/manual/1.12/en/coding-standard.coding-style.html#coding-standards.inline-documentation).

### Indentation: Tabs vs Spaces

```bash
$ ack -c --type=php "\t" | awk -F : '{t+=$2}END{print t}'
5225
$ ack -c --type=php "    " | awk -F : '{t+=$2}END{print t}'
981
```

**Both tabs and spaces** are used to indent in `main-theme`. Whichever one you choose to use, make sure your text editor displays **4 spaces per tab-stop**. 

*Side note:* Occasionally tabs can be useful for quick commenting and un-commenting of code:

```php
if ($foo) {
    barbar();
#   foobar();
}
```

If spaces were used to indent in this case, uncommenting the second `bar()` call would require deleting the hash and inserting a space to restore the indentation (or switching into replace mode) - **2 keystrokes**.

However, if a tab-stop is used, uncommenting the second `bar()` simply means deleting the pound sign, which will preserve indentation automatically - **1 keystroke**.

### Code Spacing

For function and conditional blocks:

```php
function foo($bar = 1, $baz) {
    if ($baz && $bar >= 0) {
        foo($bar - 1, !$baz);
    } else {
        echo $bar;
    }
}
```

For argument lists to function calls:

```php
foo(8 + 9, true);
```

For extra-long argument lists to function calls:

```php
$t = array(
    get_var('foo'),
    get_var('bar'), // Valid, preferred.
);
count($t); // Produces 2, not 3.
```

For string concatenation:

```php
echo 'Result: ' . get_text() . '.';
```

To sum up:

 - Spaces after commas, always!
 - Spaces between parentheses and their associated curly-braces.
 - Pad operators with spaces on both sides (except the logical negation operator `!` where a space is optional).
 - No space between function name and open parenthesis.

### PHP and Logical Blocks

Add semicolons to php one-liners even if they aren't necessary:

```php
<?php echo foo(); ?>
```

Avoid removing curly-braces from one-line blocks even if they aren't necessary:

```php
if (foo()) // Bad
    bar();

if (foo()) { // Good
    bar();
}
```

### Strings: Single Quotes vs Double Quotes

For simple strings that do not require variable interpolation, **single quotes are preferred**.

```php
throw new Exception('Something happened!');
```

Exceptions include cases where quotation mark escape sequences would complicate the code:

```php
echo 'That\'s Sean\'s.';
echo "That's Sean's.";
```

Also, be aware of variable interpolation behavior differences between single and double quotes:

```php
$foo = 'bar';
echo "Foo: $foo"; // produces Foo: bar
echo 'Foo: $foo'; // produces Foo: $foo
```

### Equality Checking

Use the strict equality operator `===` conservatively. Unless boolean values are involved, `==` usually suffices and is preferred.

```php
$a = "";
$a == false; // True
$a === false; // False
```

Additionally, the reversed equality expression is acceptable and commended:

```php
if ($t == 3) foo();
if (3 == $t) foo(); // Equivalent to above
```

### Operators

The symbol logical operators `&&`, `||`, and `!` are preferred over `and` and `or`. A sixth operator `xor` does not appear in `main-theme` but is equivalent to `!$a == (boolean) $b`.

Bitwise operators `&`, `|`, `^`, `~`, `>>`, and `<<` should be avoided for readability.

### Type Checking

When checking for primitive types, `is_float`, `is_int`, `is_bool`, and `is_array` are preferred. Otherwise, use `instanceof`:

```php
class A {}
$foo = new A();
if ($foo instanceof A) bar();
```

### Else vs Else If

```bash
$ ack -c --type=php "elseif" | awk -F : '{t+=$2}END{print t}'
36
$ ack -c --type=php "else if" | awk -F : '{t+=$2}END{print t}'
8
```

Both are used somewhat in the `main-theme`'s PHP code. This one's up to your personal preference.
