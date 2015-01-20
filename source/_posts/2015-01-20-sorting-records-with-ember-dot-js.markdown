---
layout: post
title: "Sorting records with Ember.js"
date: 2015-01-20 22:48:42 +0100
comments: true
categories: [ "emberjs", "javascript" ]
published: true
---
Ember.js provides a few really nice features out of the box to let you sort your records
for display purposes. In the [docs](http://emberjs.com/guides/controllers/representing-multiple-models-with-arraycontroller/)
there is a short paragraph about it (SORTING) that does not tell the whole story so be
sure to click the link about the [Ember.SortableMixin](http://emberjs.com/api/classes/Ember.SortableMixin.html)
for some more information.
<!--more-->

The important detail is in the last two paragraphs:
> SortableMixin works by sorting the arrangedContent array, which is the array that
> ArrayProxy displays. Due to the fact that the underlying 'content' array is not changed.

So if you changed your controller to an **ArrayController** and added the properties
**sortProperties** and **sortAscending** so your controller looks like this:

```javascript
App.SongsController = Ember.ArrayController.extend({
  sortProperties: ['name', 'artist'],
  sortAscending: true // false for descending
});
```
and you are wondering why your content is not sorted when writing the following in your template

{% highlight html %}
{% raw %}
{{#each record in content}}
 :
{{/each}}
{% endraw %}
{% endhighlight %}

It is because it should be 
{% highlight html %}
{% raw %}
{{#each record in arrangedContent}}
 :
{{/each}}
{% endraw %}
{% endhighlight %}

However the very last paragraph on the above mentioned API docs page states
> Although the sorted content can also be accessed through the arrangedContent property,
> it is preferable to use the proxied class and not the arrangedContent array directly.

That makes sense, but I am not sure how to do that in a template. So far it seems to work
as needed.
