<h1>Elementor Customize by Sergey Samokvalov (Redirex)</h1>
<p>Recently i'm faced with a challenge to resolve some w3 validator and optimiziation (like PageSpeedInsights) issues. So i'm decided that it's will be well if i create this repo for share some solutions about not trivial methods of resolve elementor issues.</p>
<p>My list of solutions here willn't cover all stuff like simply edditing html-elements from visual interface or accessable methods of edditing your templates for regular user of layout designer.</p>
<p>Here you'll not found solution like: dont contain &lsaquo;a&rsaquo; into &lsaquo;a&rsaquo; tag ot what difference betwen HTML block and inline elements.</p>
<p>Here we'll review how resolve some issues throw elementor systems code and append this code for additional improvement.</p>
<br/>
<p>So, let's get down to business</p>
<h2>1. Error: Element style not allowed as child of element div in this context. (Suppressing further errors from this subtree.)</h2>
<b>Description:</b><br/>
<p><i>From line 79, column 1; to line 79, column 7</i><br/>
ntainer">↩<style>@media<br/>
Contexts in which element <a href="https://html.spec.whatwg.org/multipage/#the-style-element" target="_blank">style</a> may be used:<br/>
Where <a href="https://html.spec.whatwg.org/multipage/#metadata-content-2" target="_blank">metadata</a> content is expected.<br/>
In a <a href="https://html.spec.whatwg.org/multipage/#the-noscript-element" target="_blank">noscript</a> element that is a child of a <a href="https://html.spec.whatwg.org/multipage/#the-head-element" target="_blank">head</a> element.<br/>
Content model for element <a href="https://html.spec.whatwg.org/multipage/#the-div-element" target="_blank">div</a>:<br/>
If the element is a child of a <a href="https://html.spec.whatwg.org/multipage/#the-dl-element" target="_blank">dl</a> element: one or more <a href="https://html.spec.whatwg.org/multipage/#the-dt-element" target="_blank">dt elements followed by one or more <a href="https://html.spec.whatwg.org/multipage/#the-dd-element" target="_blank">dd</a> elements, optionally intermixed with <a href="https://html.spec.whatwg.org/multipage/#script-supporting-elements-2" target="_blank">script-supporting elements</a>.<br/>
If the element is not a child of a <a href="https://html.spec.whatwg.org/multipage/#the-dl-element" target="_blank">dl</a> element: <a href="https://html.spec.whatwg.org/multipage/#flow-content-2" target="_blank">flow content</a>.</p>
<h3><strong>Solution:</strong></h3>
That error means that w3-validator dont allow us contain &lsaquo;script&rsaquo; tag into sort of elements like &lsaquo;div&rsaquo;.
Unfortunatly Elementor use "elementor widget container" that include all styles from visual editor into <div> elements following initializate your blocks on page and don't get access/feature to change this for developer from interface.
<strong>So what you need for chenging it?</strong>
<ol>
<li>You should follow by this path: <code>/wp-content/plugins/elementor/includes/base</code></li>
<li>Find <code>widget-base.php</code></li>
<li>Open this file and find the line <code>:576</code> that starts by <code>&lsaquo;div class="elementor-widget-container"&rsaquo;</code> into <code>public function render_content() {</code></li>
<li>Replace this code <code>$this->print_widget_css();</code> to this:
<code>
  add_action('wp_footer', function(){
    $this->print_widget_css(); // add same styles to footer
	});
</code>
as you can see - we replace all widget css code to footer that resolve our issue.
<li>Done :-).</li>
</ol>
