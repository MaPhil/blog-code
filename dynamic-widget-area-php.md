this code belongs into the functions.php

```php
<?php
function widget_theme_actionSidebarAdminSetup() {
    global $post;
    global $wp_registered_sidebars;

    // find all pages that use template with dynamic widget zone
    $query = new WP_Query(array(
        'post_type' => 'page',
        'nopaging' => true,
        'orderby' => 'title',
        'order' => 'ASC',
    ));
    while ($query->have_posts()) {
        $query->the_post();
        if (!isset($wp_registered_sidebars['widget-area' . $post->ID])) {
            register_sidebar( array(
              'name' => __("Widget Area of the Page {$post->post_title}"),
              'id' => "widget-area{$post->ID}",
              'description' => "Dynamic Widget Area",
              'before_widget' => '<div class="widget-container">',
              'after_widget' => '</div>',
             ));
        }
    }
    wp_reset_postdata();
}
add_action('widgets_init', 'widget_theme_actionSidebarAdminSetup');

?>
```


this code belongs into the template file you want to display your widget area

```php
<?php
dynamic_sidebar("widget-area{$post->ID}");
?>
```
