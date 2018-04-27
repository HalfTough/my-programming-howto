# my-programming-howto
Little howto I made to myself, to remind me how to accomplish certain tasks in given technologies

# PHP
## Wordpress
### Create Custom Type
Just put ```register_post_type( $post_type, $args )``` in any function running on `init`  
 ```add_action( 'init', 'my_func' );``` to run function at init.

[reference](https://codex.wordpress.org/Function_Reference/register_post_type)
