# Nano-Navigation

[Pico](http://pico.dev7studios.com/index.html) Plugin for more robust navigation and redirects. Inspired by [Pico Navigation](https://github.com/ahmet2106/pico-navigation).

### Installation

Start by moving the <code>plugins/nano_navigation.php</code> file into your existing Pico install. 

### Usage

The plugin adds 3 key pieces of functionality to any Pico site:

* Ability to draw an order-able, loop-able navigation tree
* Ability to redirect a sub-directory to it's first child
* Ability to define custom 301 page redirects

#### Navigation Tree

Start by defining the page order (relative to sibling pages) in the page content files:

	/*
		Title: Sub page
		Order: 1
	*/
	...

If no implicit order is defined the page title will be used instead. Ordered pages will be exist before any unordered sibling pages.

Then loop through the navigation tree in your template using a [Twig include](http://twig.sensiolabs.org/doc/templates.html#including-other-templates):

	...
	<nav>
		{% include "layout/navigation-loop.html" %}
	</nav>
	...

navigation-loop.html:

	{% for page in navigation %}
		<a href="{{ page.url }}" class="{% if page.current %}active{% endif %}">{{ page.title }}</a>
		{% if page.children %}
		<div class="children" style="margin-left: 20px">
			{% include "layout/navigation-loop.html" with {'navigation': page.children} %}
		</div>
		{% endif %}
	{% endfor %}

#### Redirect Lower 

To redirect a sub-directory's landing page to it's first child set the <code>template</code> value to <code>redirect</code> in the landing page's content file. This is especially useful when landing pages do not contain any content. 
	
	/*
		Template: redirect
		Title: About
	*/
	...

You should still define a page title as it will be used when drawing the navigation tree. 

#### Page Redirect

If you ever have to move content, or you want to deep link from a short URL, you can define a page redirect by adding a key/value pair to the <code>redirects</code> configuration array (where the key is the old page path and the value is the new page path):

	$config["nano_navigation"]["redirects"] = array(
		"old/path" => "new/path",
		...
	);