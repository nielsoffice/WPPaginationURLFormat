# WPPaginationURLFormat
WP Pagination URL Format from < d.com/blog/page/2 > To < d.com/blog/?page-id=2 >

Format URL custom pagination page counter display. 

```PHP
<?php 
  
  # Default Pagination : < domain.com >/blog/page/2
  # Modified URL : < domain.com >/blog/?page-id=2
  
  $current = max( 1, (int) filter_input( INPUT_GET, 'page-id' ) );

  $args = [
	'posts_per_page'   => 3,
	'post_type'        => 'post',
	'paged'            => $current,
	'orderby'          => 'date',
	'order'            => 'DESC'
  ];

  $wp_query_this = new WP_Query($args);
   
   echo ('<div id="tl_container" class="sb-row">');

   if($wp_query_this->have_posts() ): 
			
      while($wp_query_this->have_posts()) : $wp_query_this->the_post();
					   
	     print(wp_strip_all_tags(get_the_title()) . "<br />"); 

     endwhile; 
	 endif; 
	 
	 wp_reset_query();

   // Update URL Format Post Pagination
  echo paginate_links([
  
        'base'         => '%_%',
        'total'        => $wp_query_this->max_num_pages, // $wp_query_this base on parent query !
        'current'      => $current,
        'format'       => '?page-id=%#%',
        'show_all'     => false,
        'type'         => 'plain',
        'end_size'     => 2,
        'mid_size'     => 1,
        'prev_next'    => true,
        'prev_text'    => '« Previous',
        'next_text'    => 'Next »',
        'add_args'     => false,
        'add_fragment' => '',
    ]);

?>
```

<br /> Reference: 
<br /> https://developer.wordpress.org/reference/functions/paginate_links/
<br /> https://wordpress.stackexchange.com/questions/275527/paginate-links-ignore-my-format
