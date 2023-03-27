# WPPaginationURLFormat
WP Pagination URL Format from < d.com/blog/page/2 > To < d.com/blog?page-id=2 >

Format URL custom pagination page counter display. 

```PHP
<?php 
  
  # Default Pagination : < domain.com >/blog/page/2
  # Modified URL : < domain.com >/blog?page-id=2
  
  $current = max( 1, (int) filter_input( INPUT_GET, 'page-id' ) );

  $wp_query_this = new WP_Query([
  
       'posts_per_page'   => 3,
       'post_type'        => 'post',
       'paged'            => $current,
       'orderby'          => 'date',
       'order'            => 'DESC'
  
  ]);

   if($wp_query_this->have_posts() ): 
			
      while($wp_query_this->have_posts()) : $wp_query_this->the_post();
					   
	print(wp_strip_all_tags(get_the_title()) . "<br />"); 

      endwhile;
      
    endif; 
	 
    wp_reset_query();

  // Update URL Format Post Pagination 
  echo('<div id="id_tl_pagination">');
  echo paginate_links([
  
        'base'         => '%_%',
        'total'        => $wp_query_this->max_num_pages, // $wp_query_this base on parent query !
        'current'      => $current,
        'format'       => '?post-paged=%#%',
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
  echo('<div>');

?>
```

Initialized Current URL

```JS

jQuery(() => {
			
  let isURL = window.location.href;
  let isD   = 'https://< domain >/blog/';
  let isDp  = 'https://< domain >/blog/?post-paged=2';
  let isDp1 = 'https://< domain >/blog/?post-paged=1';	
	
  if( isURL === isD ) {
     jQuery('#id_tl_pagination a:nth-Child(2)').attr('href','/blog/?post-paged=2');  
  } else if( isURL ===  isDp ) {
     jQuery('#id_tl_pagination a:nth-Child(1)').attr('href','/blog/?post-paged=1'); 
     jQuery('#id_tl_pagination a:nth-Child(2)').attr('href','/blog/'); 
  } else if( isURL === isDp1 ) {
     jQuery('#id_tl_pagination a:nth-Child(2)').attr('href','/blog/?post-paged=2');   
  } else if( isURL !== isD || isURL !== 'https://< domain >/blog/' ) {
     jQuery('#id_tl_pagination a:nth-Child(2)').attr('href','/blog/'); 
	   
  } 
});
```

<br /> Reference: 
<br /> https://developer.wordpress.org/reference/functions/paginate_links/
<br /> https://wordpress.stackexchange.com/questions/275527/paginate-links-ignore-my-format
<br /> https://nielsoffice197227997.wordpress.com/2021/11/24/wordpress-the-loop-fetch-data-from-database-wp_query-php/
