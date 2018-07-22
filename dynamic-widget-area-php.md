Dynamic widget Areas in a Wordpress Theme
=====
Currently I'm working on a Wordpress theme wich allows you to configure the whole page over widgets. This approach makes the system easier to use and way more flexible in administration.

One problem I encountered just from the beginning was that I had to create widget areas for every page rather then widget areas for the global theme. Since I'm not mainly a php developer it took me a little while to get this problem sorted. In this article I want to share my experience, maybe it can be useful for someone else.

To tackle this problem you need to do basically two things:

1. You need to append some code at the end of the functions.php

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

2. You need to add some code into you template at the position you want the widget area to be

```php
<?php
dynamic_sidebar("widget-area{$post->ID}");
?>
```

I hope this little example helps you to understand how to work with dynamic widget areas. Here are some links I used as a reference for my solution:

https://stackoverflow.com/questions/15410313/wordpress-dynamic-sidebar-displays-nothinghttps://wordpress.stackexchange.com/questions/76601/individual-widgets-per-page
