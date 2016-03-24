##Automatically wrap contents of `var_dump` with a `<pre>` element for better readability.

Put this in functions.php to use anywhere in your theme:

```
function pre_dump($var) {
	echo '<pre>';
	var_dump($var);
	echo '</pre>';	
}
```	
