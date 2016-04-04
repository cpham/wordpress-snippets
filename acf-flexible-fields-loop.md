##Automatically include corresponding template for a ACF Flexible Field layout using get_template_part and get_row_layout:

```php
// check if the flexible content field has rows of data
if( have_rows('flexible_content_field_name') ):
	
	// loop through the rows of data
	while ( have_rows('flexible_content_field_name') ) : the_row();
	
		//include layout-LAYOUT_NAME.php
		get_template_part('layout', get_row_layout() );
	
	endwhile;

else :

// no layouts found

endif;
```
