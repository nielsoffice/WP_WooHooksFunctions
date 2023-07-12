# WP_WooHooks-Function
WordPress WooCommerce all hooks and function 

``` apply_filters( string $hook_name, mixed $value, mixed $args ): mixed ```
<br /> Reference: https://developer.wordpress.org/reference/functions/apply_filters/

```PHP
// Create a Custom filter | The filter callback function.
function add_custom_filter( $string, $arg1, $arg2 ) {
  // filtered result condition
 	if( isset($string) ) {	return $string; } 
  // default value set
  return 'The filter';
}
add_filter( 'custom_filter_name', 'add_custom_filter', 10, 3 );

// if NOT filter the value return as (string) " The filter "
$value = apply_filters( 'custom_filter_name',  $string, $arg1, $arg2 );

// if this is FILTERED the value return as (string) " filtered this! "
$value = apply_filters( 'custom_filter_name', 'filtered this!', $arg1, $arg2 );

var_dump($value);

// NOTE: Set the result based on request return value, then place using hook 
// Assign Custom add-hook: https://nielsoffice2017.wordpress.com/2022/08/04/wordpress-using-and-create-custom-hooks/

```

``` remove_action( string $hook_name, callable|string|array $callback, int $priority = 10 ): bool ```
<br /> Reference: https://developer.wordpress.org/reference/functions/remove_action/

``` add_action( string $hook_name, callable $callback, int $priority = 10, int $accepted_args = 1 ): true ```
<br /> Reference: https://developer.wordpress.org/reference/functions/add_action/

``` add_filter( string $hook_name, callable $callback, int $priority = 10, int $accepted_args = 1 ): true ```
<br /> Reference: https://developer.wordpress.org/reference/functions/add_filter/

``` do_action( string $hook_name, mixed $arg ) ```
<br /> Reference: https://developer.wordpress.org/reference/functions/do_action/

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

```PHP
// WP do_action(); | Calls the callback functions that have been added to an action hook.
#Example_1
// Arguments that will be passed on to the functions hooked to the action.
$a = $defined; // Some dinamic variable
$b = 'Some string';

// Do the action (activate the hook)
do_action( 'my_hook', $a, $b );
And functions.php of the theme contains such code:

function do_my_hook( $a, $b ){

	// if variable $a is equal true,
	// then, for example, delete the post with ID 10
	if( $a === true ) {
		wp_delete_post( 10 );
	}

	// A here just display the value of the second variable
	echo '<br />'.$b;
}

// add function to the hook
add_action( 'my_hook', 'do_my_hook', 10, 2 );

#Example_2
function who_is_hook( $a, $b ) {

	print_r( $a ); // `print_r` the array data inside the 1st argument

	echo $b; // echo linebreak and value of 2nd argument
}

// then add it to the action hook, matching the defined number (2) of arguments in do_action
// see [https://codex.wordpress.org/Function_Reference/add_action] in the Codex

// add_action( $tag, $function_to_add, $priority, $accepted_args );
add_action( 'i_am_hook', 'who_is_hook', 10, 2 );

// Define the arguments for the action hook
$a = array(
	'eye patch'  => 'yes',
	'parrot'     => true,
	'wooden leg' => 1
);

$b = 'And Hook said: "I ate ice cream with Peter Pan."';

// Executes the action hook named 'i_am_hook'
do_action( 'i_am_hook', $a, $b );

// Result:
Array (
	['eye patch'] => 'yes'
	['parrot'] => true
	['wooden leg'] => 1
)
And hook said: "I ate ice cream with Peter Pan."

// Source: https://wp-kama.com/function/do_action
```

<br /> All WP core Hook lists 
<br /> https://codex.wordpress.org/Plugin_API/Action_Reference
<br /> All WP core Filter lists
<br /> https://codex.wordpress.org/Plugin_API/Filter_Reference

<br /> All Woo Hooks
<br /> https://wp-kama.com/plugin/woocommerce/hook
<br /> All Woo Functions
<br /> https://wp-kama.com/plugin/woocommerce/function

<br /> Create/Assigned a custom hooks
<br /> https://nielsoffice2017.wordpress.com/2022/08/04/wordpress-using-and-create-custom-hooks/
<br /> Add Filter WP Core
<br /> https://nielsoffice2017.wordpress.com/2021/11/21/wordpress-add_filter/

<br /> And also 
<br /> https://github.com/nielsoffice/wooCommerce-DisplayProducts-php-loop

