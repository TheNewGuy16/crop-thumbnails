# Crop-Thumbnails

"Crop-Thumbnails" is an Wordpress-Plugin for editing cropped image-sizes. Every wordpress-media-image that is defined via "add_image_size" could be defined as cropped image, but in the default Wordpress-Image-Editor you are not able to choose what part of the image should be shown. Thats where the plugin kicks in. "Crop-Thumbnails" provide an additional editor, which is available from all relevant positions in the backend.

## Installation

The plugin can be installed from the wordpress plugin repository. 

https://wordpress.org/plugins-wp/crop-thumbnails/

## How to add custom cropped image-sizes
This is the default wordpress way to add new image-sizes. 
```php
add_action('after_setup_theme', function() {
	add_image_size('my-custom-imagesize', 500, 500, true);
	//you have to set "true" to get a cropped size
	//the plugin will only handle cropped sizes
});
```

## Filters and action-hooks
The plugin provides some filters/actions if you want to adjust or extend the behaviour.

### ACTION `crop_thumbnails_after_save_new_thumb`
This action is called after saving a thumbnail by the plugin.

Provided data:
* `$fullFilePath`
* `$imageSizeName`
* `$imageMetadata['sizes'][$imageSizeName]`



### FILTER `crop_thumbnails_editor_printratio`

Filters the ratio that is printed in the modal-dialog. Based on this ratio a selection is possible.

Parameters:
* `$printRatio`
* `$imageSizeName`

The filter can be used if you have two image-sizes that have nearly the same ratio but are are slightly different. You can add the following code in the functions.php of your theme to adjust the ratio of one or more specified image-sizes.

CAUTION: use only when the ratios are really close.

```php
add_filter( 'crop_thumbnails_editor_printratio', 'my_crop_thumbnails_editor_printratio', 10, 2);
function my_crop_thumbnails_editor_printratio( $printRatio, $imageSizeName) {
	if($imageSizeName === 'strange-image-ratio') {
		$printRatio = '4:3';//do override ratio
	}
	return $printRatio;
}
```



### FILTER `crop_thumbnails_editor_jsonDataValues`

The filter can be used to adjust the json-data that are used by the editor.

Parameters:
* `$jsonDataValues`



### FILTER `crop_thumbnails_before_update_metadata`

The filter is called right before the attachement metadata are saved.

Parameters:
* `$imageMetadata`
* `$input->sourceImageId` - the id of the attachement



### FILTER `crop_thumbnails_image_sizes`

A filter to remove/adjust image-sizes used by the plugin. Use carefully, in most cases the settings-screen provide enough possibilities to adjust displaying of image-sizes.

Parameters:
* `$sizes`



### FILTER `image_size_names_choose` (wordpress core)

The filter is provided by wordpress and used by the plugin. You can use the filter to provide the human-readable name of a image-size.

Example
```php
add_filter( 'image_size_names_choose', 'my_custom_sizes' );
function my_custom_sizes( $sizes ) {
	$sizes = array_merge( $sizes, array(
		'strange-size' => 'my new name',
		'abc' => 'alphabet',
	));
	return $sizes;
}
```