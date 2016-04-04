# Passing Variables to Template Parts
You can pass a global variable to `get_template_part()` with the `set_query_var()` function.

##Example:

You wish to make `$my_var` available to the template part at content-part.php
```php
set_query_var( 'my_var', $my_var );
get_template_part( 'content', 'part' )
```

In content-part.php, use $my_var as you would any global variable.

```php
echo $my_var;
```
