# WP_WooHooksFunction
WordPress WooCommerce all hooks and function 

<br /> ACTIONS : Returning the data specific while wp is doing something
<br /> add_action() : Hooks our function set everything on it
<br /> do_action() : Executes "hooked" function that was set 
```PHP
// add action | This is where we can set our function 
add_action( 'custom_name_of_hook', 'call_back_func_name', 10, 2 );
function call_back_func_name( $a, $b ){
   
  // to pass a data to this function we have to use a do_action();
  // Here we can do something with the parameters
  if( $a == $b ) { return true; }
}

// do_action | This is where we can pass the argument that we set from our custom hooks
/* Set a filter using apply_filter then use add_action to filter the data and use do_action   
   Once the call_back_function has an argument before we pass it to the action 
   means do_action comes into play once call back function from action('apply_filter_name','call_back_function') 
   having parameter or argument */

do_action( 'custom_name_of_hook', 3, 4 );
// Result: false
```
<br /> HOOKS
<br /> add_filter() : Hooks our function modify data on it
<br /> apply_filter() : Executes the "hook" function that was set from it! 

<br /> ``` apply_filters( string $hook_name, mixed $value, mixed $args ): mixed ```
<br /> Reference: https://developer.wordpress.org/reference/functions/apply_filters/

```PHP
/* Yesterday */
/* I set the current value to my filter so soon I can modify or replace */
// https://nielsoffice2017.wordpress.com/2021/11/21/wordpress-add_filter/
// https://github.com/nielsoffice/WP-CPT-apply-custom-filter/blob/main/functions.php
appy_filters('my_filter_name','Current Value is Event');

/* Today w/ Priority 10 and having 3 argument */
add_action('my_filter_name', 'todays_i_modifying', 10, 4 );
function todays_i_modifying( $args , $date, $month, $yr) {
 
  // Yesterday  $args = 'Current Value is Event'
  // First is argument is Date
  // Second is Month
  // Third is YR
  $arg = '<i>' . '</i>';

  return $arg. ' '. $date . ' ' . $month . ' ' . $yr;
  // Expected Result: Current Value is Event 16, July 2023
  
}

/* Today's date */
do_action('my_filter_name', wp_date( 'F' ), wp_date( 'j' ), wp_date( 'Y' ));

/* Present & Future */
//  Create a Custom filter | The filter callback function.
add_filter('custom_filter_i_like','func_for_this_custom_filter', 10, 3 );
function func_for_this_custom_filter( $string, $arg1, $arg2  ) {

  // filtered result condition
  if( isset($string) ) { return $string; } 

  // default value set
  return 'The filter';

}

// if NOT filter the value return as (string) " The filter "
$value = apply_filters( 'custom_filter_i_like',  $string, $arg1, $arg2 );

// if this is FILTERED the value return as (string) " filtered this! "
$value = apply_filters( 'custom_filter_i_like', 'filtered this!', $arg1, $arg2 );

var_dump($value);

/* Conclusion: For me add_action and add_filter() are similar however let's say the add_filter is used to modify the data before and out of the database
while the add_action will replace or add something to it, ui layout related etc... 
*/

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

<br /> NOTE: ```  int $priority = 10 ``` by default set to 10 means first, for instance, you have multiple actions with that hook
like ex. the first hook getting user_id, the second hook finds the post by user_id which is priority 20 because we have to GET the user_id first before 
we can use the id to filter the post etc...

<br /> NOTE: ``` int $accepted_args = 1 ``` by default set to 1 means the argument that you can pass to the function hooks is one
so you have to set how many arguments or parameters you will supply for the particular hook.
<br /> ``` ex. add_action( 'custom_name_of_hook', 'call_back_func_name', 10, 2 ); ```

<br /> Approach Static class method:
<br /> ```  add_filter( 'media_upload_newtab', array( 'My_Class', 'media_upload_callback' ) ); ```
<br /> Approach Instance method:
<br /> ```  add_filter( 'media_upload_newtab', array( $this, 'media_upload_callback' ) ); ```
<br /> Approach You can also pass an anonymous function as a callback:
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
<br /> General Snippet
<br /> https://woocommerce.com/documentation/plugins/woocommerce/woocommerce-codex/snippets/

<br /> Create/Assigned a custom hooks
<br /> https://nielsoffice2017.wordpress.com/2022/08/04/wordpress-using-and-create-custom-hooks/
<br /> Add Filter WP Core
<br /> https://nielsoffice2017.wordpress.com/2021/11/21/wordpress-add_filter/
<br /> https://nielsoffice2017.wordpress.com/2021/11/20/wordpress-apply-filter-hook-custom-filter/
<br /> https://github.com/nielsoffice/WP-CPT-apply-custom-filter/blob/main/functions.php

<br /> Related:
<br /> https://adambrown.info/p/wp_hooks

<br /> And also 
<br /> https://github.com/nielsoffice/wooCommerce-DisplayProducts-php-loop

