# my-programming-howto
Little howto I made to myself, to remind me how to accomplish certain tasks in given technologies.

For that reason those are aimed primarily at people who already have some knowledge about those tasks. This page shouldn't be treated as a tutorial.

# PHP
## Wordpress
### Create Custom Type
Just put ```register_post_type( $post_type, $args )``` in any function running on `init`  
 ```add_action( 'init', 'my_func' );``` to run function at init.

[reference](https://codex.wordpress.org/Function_Reference/register_post_type)

### Adding custom meta boxes
Most important bit is ```add_meta_box( $id, $title, $callback, $screen, $context, $priority, $callback_args)``` ([reference](https://codex.wordpress.org/Plugin_API/Action_Reference/add_meta_boxes)).  
Remember to call in under `add_meta_boxes` hook (`add_action( 'add_meta_boxes', 'adding_custom_meta_boxes')`).

```
function add_my_metabox($post){
	add_meta_box('my-metabox', 'Tytuł', 'my_metabox_fill', 'mytype', 'normal', 'high');
}
add_action('add_meta_boxes', add_my_metabox');
```

Callback must be a function echoing content of your box.  
It's good to start with `wp_nonce_field(basename( __FILE__ ), 'my_nonce_name')` to ensure form requests are all right.  
`get_post_meta($id, $meta-id, $single)` should be useful


```
function my_metabox_fill($post){
	wp_nonce_field(basename( __FILE__ ), 'my_nonce_name');
	$value = get_post_meta( $post->ID, 'my_field', true);
	?>
	<input type='text' name='my_field' value="<?= $value ?>" style="width: 100%" />
	<?php
}
```

Also, we should save our data

```
function save_my_metabox($post_id){
	if ( !isset( $_POST['my_nonce_name'] ) || !wp_verify_nonce( $_POST['my_nonce_name'], basename( __FILE__ ) ) )
		return;
	if ( ! current_user_can( 'edit_post', $post_id ) )
		return;
	if( isset($_POST['my_field'])){
		update_post_meta($post_id, 'my_field', $_POST['my_field']);
	}
}
add_action('save_post_mytype', 'save_my_metabox');
```
