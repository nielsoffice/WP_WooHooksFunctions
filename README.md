# WP_WooHooks-Function
WordPress WooCommerce all hooks and function 

``` add_action( string $hook_name, callable $callback, int $priority = 10, int $accepted_args = 1 ): true ```
<br /> Reference: https://developer.wordpress.org/reference/functions/add_action/

``` add_filter( string $hook_name, callable $callback, int $priority = 10, int $accepted_args = 1 ): true ```
<br /> Reference: https://developer.wordpress.org/reference/functions/add_filter/

<br /> Approach Static class method:
<br /> ```  add_filter( 'media_upload_newtab', array( 'My_Class', 'media_upload_callback' ) ); ```
<br /> Approach Instance method:
<br /> ```  add_filter( 'media_upload_newtab', array( $this, 'media_upload_callback' ) ); ```
<br /> Approach You can also pass an an anonymous function as a callback:
<br /> ```  add_filter( 'the_title', function( $title ) { return '<strong>' . $title . '</strong>'; } ); ```
<br />


```PHP
// Filters and actions are both assigned to hooks. Functions assigned to hooks are stored in global
// $wp_filter variable. So all you have to do is to print_r it.

echo '<pre>';
print_r($GLOBALS['wp_filter']);
echo '</pre>';

```

```
 PS. add_action function makes a add_filter call. And the latter does $wp_filter[$tag][$priority][$idx].
```

```PHP
// NOTE: you can directly add this code in functions.php, and you will see a debug on your site:
add_action('wp', function(){ echo '<pre>';print_r($GLOBALS['wp_filter']); echo '</pre>';exit; } );
```


<br /> All WP core Hook lists 
<br /> https://codex.wordpress.org/Plugin_API/Action_Reference
<br /> All WP core Filter lists
<br /> https://codex.wordpress.org/Plugin_API/Filter_Reference

<br /> All Woo hooks
<br /> https://wp-kama.com/plugin/woocommerce/hook
<br /> All Woo Functions
<br /> https://wp-kama.com/plugin/woocommerce/function

<br /> Create/Assigned a custom hooks
<br /> https://nielsoffice2017.wordpress.com/2022/08/04/wordpress-using-and-create-custom-hooks/
<br /> Add Filter WP Core
<br /> https://nielsoffice2017.wordpress.com/2021/11/21/wordpress-add_filter/

<br /> And also 
<br /> https://github.com/nielsoffice/wooCommerce-DisplayProducts-php-loop

