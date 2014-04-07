---
layout: post
title: "PHP: The Right Way"
date: 2012-07-10 11:12
comments: true
categories: [php, best practices]
---
Today I discovered an interesting new project called ["PHP: The Right Way"](http://www.phptherightway.com/). It is an open source project maintained by [Josh Lockhart](https://twitter.com/codeguy) attempting to provide a well rounded set of best practices for developing in PHP.

While I have not yet attempted to contribute any content I have added the banner to my personal blog to show my support. If you are interested it adding a "PHP: The Right Way" banner to the side bar of your [Octopress](http://octopress.org) blog, here is how. Allthough, if you are using octopress you probably don't need help with this ;-).
<!--more-->
Following is the html for the aside. The 240x400 image size works well for the sidebar width that I have. This is easy to change.

{% codeblock /source/includes/custom/asides/php_the_right_way.html lang:html %}
{% raw %}
{% if site.php_the_right_way_show %}
<section>
  <a href="http://www.phptherightway.com">
    <img class="center" src="http://www.phptherightway.com/images/banners/vert-rect-240x400.png" alt="PHP: The Right Way"/>
  </a>
</section>
{% endif %}
{% endraw %}
{% endcodeblock %}

In ```/_config.yml``` add the aside to the list of ```default_asides``` in the location you want it to show up in. Mine is at the end.

{% codeblock /_config.yml lang:yaml %}
default_asides: [custom/asides/about.html, asides/recent_posts.html, asides/twitter.html, asides/github.html, custom/asides/php_the_right_way.html]
{% endcodeblock %}

In the configuration file add the following to the very end.
{% codeblock /_config.yml lang:yaml %}
# PHP The Right Way Banner image
php_the_right_way_show: true
{% endcodeblock %}

That's it!
