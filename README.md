codeigniter_crawler
===================

A website crawler for the CodeIgniter framework

Usage
-----

1. Copy Crawler.php into the library directory of your CodeIgniter application
2. codeigniter_crawler has one requirement, Simple_Html_Dom. Download from http://simplehtmldom.sourceforge.net/ and copy Simple_html_dom.php into the libraries directory of your CodeIgniter application.
3. Load Crawler library from a controller. `$this->load->library('crawler');`
4. Set a URL. `$this->crawler->set_url('http://github.com');`
5. Use any of the public methods to obtain data from the URL you just set. `$this->crawler->get_text();`

Public Methods
--------------

- `get_text()` - Get plain text from the loaded URL.
- `get_title()` - Get the meta title from the loaded URL.
- `get_description()` - Get the meta description from the loaded URL.
- `get_keywords()` - Get the meta keywords from the loaded URL.
- `get_links($excluded_terms, $included_terms)` - Get all links from the loaded URL.

Example
-------

Crawl github:


    function get_site_data($url, $max_depth = 1, $current_depth = 0){
      $current_depth++;
    
      $this->load->library('crawler');
    
    	$site_data = array();
    
    	if($this->crawler->set_url($site_url) !== false){
    		$site_data['title'] = $this->crawler->get_title();
    		$site_data['description'] = $this->crawler->get_description();
    		$site_data['keywords'] = $this->crawler->get_keywords();
    		$site_data['text'] = $this->crawler->get_text();
    		$site_data['links'] = $this->crawler->get_links();
    
    		if($current_depth <= $max_depth){
    			foreach($site_data['links'] as $link_key => &$link){
    				$link['data'] = get_site_data($link, $max_depth, $current_depth);
    				}
    			}
    		}
    
    		return $site_data;
    	}
    	else{
    		return false;
    	}
    }
    
    $site_data = get_site_data("http://github.com", 1, 0);

