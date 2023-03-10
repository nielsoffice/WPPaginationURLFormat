# WPPaginationURLFormat
WP Pagination URL Format from < d.com/blog/page/2 > To < d.com/blog/?page=2 >

Format URL custom pagination page counter display. 

```PHP
<?php 
  
  # Default Pagination : < domain.com >/blog/page/2
  # Modified URL : < domain.com >/blog/?page=2
  
  $current   = max( 1, (int) filter_input( INPUT_GET, 'p-page' ) );

  $args = [
      'posts_per_page'   => 6,
      'post_type'        => 'post',
      'paged'            => $current,
      'orderby'          => 'date',
      'order'            => 'DESC'
  ];

  $customPostQuery = new WP_Query($args);
   
   echo ('<div id="tl_container" class="sb-row">');

   if($customPostQuery->have_posts() ): 
			
      while($customPostQuery->have_posts()) :
                   
	     $customPostQuery->the_post();
					   
	     print(wp_strip_all_tags(get_the_title()) . "<br />"); 

     endwhile; 
	 endif; 
	 
	 wp_reset_query();

   // Update URL Format Post Pagination
   echo paginate_links([

        'base'         => '%_%',
        'total'        => $customPostQuery->max_num_pages,
        'current'      => $current,
        'format'       => '?p-page=%#%',
        'show_all'     => false,
        'type'         => 'plain',
        'end_size'     => 2,
        'mid_size'     => 1,
        'prev_next'    => true,
        'prev_text'    => '<i></i> <i class="icon-chevron-left"></i>',
        'next_text'    => '<i class="icon-chevron-right"></i> <i></i>',
        'add_args'     => false,
        'add_fragment' => '',
    
   ]);

?>
```
