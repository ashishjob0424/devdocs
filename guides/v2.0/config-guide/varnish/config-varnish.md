---
layout: default
group: config-guide
subgroup: CM_Varnish
title: Configure and use Varnish
menu_title: Configure and use Varnish
menu_order: 1
menu_node: parent
github_link: config-guide/varnish/config-varnish.md
---


#### Contents
*	<a href="#config-varnish-over">Overview of the Varnish solution</a>
*	<a href="#config-varnish-process">Process overview</a>
*	<a href="#config-varnish-issues">Known issues</a>
*	Install Varnish and configure Magento to use it:
	*	<a href="#config-varnish-process">Process overview</a>
	*	<a href="{{ site.gdeurl }}config-guide/varnish/config-varnish-install.html">Install Varnish</a>
	*	<a href="{{ site.gdeurl }}config-guide/varnish/config-varnish-configure.html">Configure Varnish and your web server</a>
	*	<a href="{{ site.gdeurl }}config-guide/varnish/config-varnish-magento.html">Configure Magento to use Varnish</a>
	*	<a href="{{ site.gdeurl }}config-guide/varnish/config-varnish-final.html">Final verification</a>
*	Use Varnish:
	*	<a href="{{ site.gdeurl }}config-guide/varnish/use-varnish-cache.html">How Magento cache clearing works with Varnish</a>
	*	<a href="{{ site.gdeurl }}config-guide/varnish/use-varnish-cache-how.html">How Varnish caching works</a>

<h2 id="config-varnish-over">Overview of the Varnish solution</h2>
<a href="https://www.varnish-cache.org/" target="_blank">Varnish Cache</a> is an open source web application accelerator (also referred to as an *HTTP accelerator* or *caching HTTP reverse proxy*). Varnish stores (or caches) files or fragments of files in memory; this enables Varnish to reduce the response time and network bandwidth consumption on future, equivalent requests. Unlike other servers like Apache and nginx, Varnish was designed for use exclusively with the HTTP protocol.

Magento 2 supports Varnish versions 3.0.5 or later or any Varnish 4.x version.

<div class="bs-callout bs-callout-warning">
    <p>For performance reasons, we <em>strongly recommend</em> you use Varnish in production instead of the default full-page caching. Full-page caching (to either the file system or database) is much slower than Varnish. Varnish is designed to accelerate the HTTP protocol.</p>
    <p>Full-page caching works well in a development environment.</p>
</div>


For more information about Varnish, see:

*	<a href="https://en.wikipedia.org/wiki/Varnish_%28software%29" target="_blank">wikipedia</a>
*	<a href="https://www.varnish-cache.org/about" target="_blank">About Varnish</a>
*	<a href="https://www.varnish-software.com/book/4.0/chapters/Introduction.html#what-is-varnish" target="_blank">Introduction to Varnish</a>
*	<a href="https://www.varnish-cache.org/docs/trunk/reference/varnishd.html#ref-varnishd-options" target="_blank">Varnish startup options</a>
*	<a href="https://www.varnish-software.com/book/3/Tuning.html#threading-parameters" target="_blank">Varnish tuning parameters</a>

<div class="bs-callout bs-callout-info" id="info">
	<ul><li>Except where noted, you must enter all commands discussed in this topic as a user with <code>root</code> privileges.</li>
		<li>This topic is written for Varnish on CentOS and Apache 2.2. If you're setting up Varnish in a different environment, some commands are likely different. Consult Varnish documentation for more information.</li></ul>
</div>

<h2 id="config-varnish-process">Process overview</h2>
This topic discusses how to initially install Varnish with a minimal set of parameters and test that it works. Then you'll export a Varnish configuration from the Magento Admin and test it again.

The process can be summarized as follows:

1.	Install Varnish and test it by accessing any Magento page to see if you're getting HTTP response headers that indicate Varnish is working.
2.	Install the Magento software and use the Magento Admin to create a Varnish configuration file. 
3.	Replace your existing Varnish configuration file with the one generated by the Admin.
3.	Test everything again.

	If there is nothing in your `<your Magento install dir>/var/page_cache` directory, you've successfully configured Varnish with Magento!


<h2 id="config-varnish-issues">Known issues</h2>
We know of the following issues with Varnish:

*	<a href="https://www.varnish-cache.org/docs/3.0/phk/ssl.html" target="_blank">Varnish does not support SSL</a>

	As an alternative, use SSL termination or an <a href="https://en.wikipedia.org/wiki/TLS_termination_proxy" target="_blank">SSL termination proxy</a>.

*	If you manually delete the contents of the `<your Magento install dir>/var/cache` directory, you must restart Varnish.
*	Possible error installing Magento:

		Error 503 Service Unavailable
		Service Unavailable
		XID: 303394517
		Varnish cache server

	If you experience this error, edit `default.vcl` and add a timeout to the `backend` stanza as follows:

		backend default {
	      .host = "127.0.0.1";
	      .port = "8080";
	      .first_byte_timeout = 600s;
		}


#### Next step
<a href="{{ site.gdeurl }}config-guide/varnish/config-varnish-install.html">Install Varnish</a>