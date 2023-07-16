# WP_WooHooksFunction
WordPress WooCommerce all hooks and function 

<br /> ACTIONS : Returning the data specific while wp is doing something
<br /> do_action() : Executes "hook" function that was set 
<br /> ``` do_action( $tag, $arg ) ``` ``` do_action( $tag, $arg1, $arg2 ) ``` ``` do_action_ref_array( $tag, $array_arg ) ```
<br /> add_action() : Hooks our function set everything on it

```PHP
// ex1 Execute the action hook* Custom Hook or Function without argument!
do_action('insert_after_content');
// Add action to hook
add_action('insert_after_content', call_back_func_insert_after_content);
function call_back_func_insert_after_content() { /* return content */ }

// ex2 Execute the action hook* Custom Hook or Function with a single argument!
// do_action argument is the set or default value from the given place, for instance, user class $user_id, $user_name
$argu1 = $user_id;
do_action('insert_after_content', $argu1 );

// add action to hook
add_action('insert_after_content', call_back_func_insert_after_content);
function call_back_func_insert_after_content(  $argu1 ) {

  // Default value is $argu1 = $user_id;
  /* work on it $argu1 then return content... */

}

// ex3 Execute the action hook* Custom Hook or Function two arguments !
// do_action argument is the set or default value from the given place, for instance, user class $user_id, $user_name
// It depends on how this hook will work for instance this hook return data user_id and user_role
$argu1 = user_id;
$argu2 = user_role;
do_action('get_user_capability', $argu1, $argu2 );
// add action to hook
add_action('get_user_capability', call_back_func_get_user_capability);
function call_back_func_insert_after_content(  $argu1, $argu2 ) {
   // Default value are user_id and user_role
   /* work on it return content... */
   /* Note: this content will run or execute to the place where do_action() are being placed 
     and return the output to the front end for instance or admin if it is assigned or placed to wp-admin
   */

}

// ex3 Execute the action hook* Custom Hook or Function without argument!
// This is the same as the above example however this is an array approach
do_action('insert_after_content', [ ] );
//Add action to hook
add_action('insert_after_content', call_back_func_insert_after_content);
function call_back_func_insert_after_content(  []  ) { /* return content... */ }

// Default WP Hook assigned in WP core 
add_action('the_title', 'modifying_return_data_title', 10 , 1 );
// As you can see this is not a custom hook by default wp only assigned this hook for 1 argument by default
function modifying_return_data_title( $title ) {
 return 'Hey, ' + title; 
}
// Result Post: Hey, Blog post for July 2023!  | /* post everywhere while WordPress does something! or fetch the title and display */

add_action('the_title', 'modifying_return_data_title', 10, 2 );
// As you can see this is not a custom hook by default wp only assigned this hook for 1 argument by default
function modifying_return_data_title( $title ) {
 // get $user_name =  get_wp_username_function() // this is example only! 
 return 'Hey, ' + $user_name + ' : ' + $title; 
}
// Result Post: Hey, John : Blog post for July 2023! | /* post everywhere while WordPress does something! or fetch the title and display */

```
<br /> HOOKS
<br /> apply_filter() : Executes the "hook" function that was set from it! 
<br /> add_filter() : Hooks our function modify data on it

<br /> ``` apply_filters( string $hook_name, mixed $value, mixed $args ): mixed ```
<br /> Reference: https://developer.wordpress.org/reference/functions/apply_filters/

```PHP
/* Yesterday */
/* Present & Future */
// if NOT filter the value return as (string) " Default Value "
$value = apply_filters( 'custom_or_wp_hook',  $string);

// if this is FILTERED the value return as (string) " 'Value is being modified! "
$value = apply_filters( 'custom_or_wp_hook_with_argument', $string, $user_name, $user->id ); // just in case you need to pass an argument

var_dump($value);

/* Result : */ 
// Value is being modified!;

/* Result in case of multiple arguments: */ 
// John ID: 2 Value is being modified!;

apply_filter('custom_or_wp_hook', ' Default Value ');
add_filter('custom_or_wp_hook', 'modifying');
function modifying(  $string ) {
  return 'Value is being modified!';
}

add_filter('custom_or_wp_hook_with_argument', 'modifying', 10, 3);
function modifying(  $string,  $user_name, $user_id ) {

  $set = $user_name .' ID: '. $user_id  + 'Value is being modified!';
  return ( $set );  
 
}


// NOTE: Set the result based on request return value, then place using hook 
// Assign Custom add-hook: https://nielsoffice2017.wordpress.com/2022/08/04/wordpress-using-and-create-custom-hooks/

/* Conclusion: For me add_action and add_filter() are similar however let's say the add_filter is used to modify the data before and out of the database
while the add_action will replace or add something to it, ui layout related etc... 
*/

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

