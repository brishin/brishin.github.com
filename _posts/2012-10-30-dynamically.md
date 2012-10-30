---
layout: post
title: "Dynamically Downloading Templates Using AngularJS"
description: ""
category:
tags: ["AngularJS"]
---

{% include JB/setup %}

One of the pain points that I discovered while working at my tech internship this year working with [AngularJS](http://angularjs.org/) was that, when deploying AngularJS applications to third party sites where cross-site requests are disallowed to our servers hosting the MVC code.

The solution that I came up with was to dynamically download the templates when the controller code runs using jQuery and JSONP.

{% highlight coffeescript linenos %}
IndexCtrl = ($scope, $compile) ->
  STATIC_DOMAIN = 'http://site.com/static/'
  jQuery.ajax
    url: STATIC_DOMAIN + 'index.html.json'
    dataType: 'jsonp'
    success: (data) ->
      template = data
      newElement = $compile(template)($scope)
      jQuery('.row-fluid').append newElement
      $scope.$apply()
    jsonpCallback: 'callback_index_html'
{% endhighlight %}

This requires pregenerated JSON that is appropriate for the JSONP format and is served as a static file by nginx.

$compile will process the text based template and attach the appropriate listeners.

$scope.$apply() is necessary to trigger an apply cycle to update the DOM with the javascript.