```PHP
https://stackoverflow.com/questions/54699603/creatig-a-hooks-filters-fuctionality-with-php
```

```PHP

<?php

global $hooks;

$hooks = array();

function addHook($hook, $function)
{
    global $hooks;
    $hooks[$hook][] = $function;
}


function getHook($hook)
{
    global $hooks;
    foreach ($hooks[$hook] as $func) {
        $func();
    }
}

function myFunc()
{
    echo "hello!";
}

addHook("myhook", "myFunc");

getHook('myhook');


?>

```
