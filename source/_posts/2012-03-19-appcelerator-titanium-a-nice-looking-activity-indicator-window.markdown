---
layout: post
title: "Appcelerator Titanium - A Nice Looking Activity Indicator Window"
date: 2012-03-19 13:13
comments: true
categories: [appcelerator, titanium, mobile dev, dev, javascript]
---
>#### UPDATE (April 11th 2013):
>I updated the demo project on [github](http://github.com/hglattergotz/indicatordemo) with a patch from Khawar. The demo now also works on android.

The *Titanium.UI.ActivityIndicator* is a commonly used component in my iOS applications, but on its own it does not look very appealing.
To make the activity indicator look a little nicer I dressed it up in its own window that has is slightly opaque and stuck it in a commonjs module.
<!--more-->
{% codeblock lib/UiElements.js lang:javascript %}
/**
 * Indicator window with a spinner and a label
 *
 * @param {Object} args
 */
function createIndicatorWindow(args) {
    var width = 180,
        height = 50;

    var args = args || {};
    var top = args.top || 140;
    var text = args.text || 'Loading ...';

    var win = Titanium.UI.createWindow({
        height:           height,
        width:            width,
        top:              top,
        borderRadius:     10,
        touchEnabled:     false,
        backgroundColor:  '#000',
        opacity:          0.6
    });

    var view = Ti.UI.createView({
        width:   Ti.UI.SIZE,
        height:  Ti.UI.FILL,
        center:  { x: (width/2), y: (height/2) },
        layout:  'horizontal'
    });

    function osIndicatorStyle() {
        style = Ti.UI.iPhone.ActivityIndicatorStyle.PLAIN;

        if ('iPhone OS' !== Ti.Platform.name) {
            style = Ti.UI.ActivityIndicatorStyle.DARK;
        }

        return style;
    }

    var activityIndicator = Ti.UI.createActivityIndicator({
        style:   osIndicatorStyle(),
        left:    0,
        height:  Ti.UI.FILL,
        width:   30
    });

    var label = Titanium.UI.createLabel({
        left:    10,
        width:   Ti.UI.FILL,
        height:  Ti.UI.FILL,
        text:    text,
        color:   '#fff',
        font:    { fontFamily: 'Helvetica Neue', fontSize: 16, fontWeight: 'bold' }
    });

    view.add(activityIndicator);
    view.add(label);
    win.add(view);

    function openIndicator() {
        win.open();
        activityIndicator.show();
    }

    win.openIndicator = openIndicator;

    function closeIndicator() {
        activityIndicator.hide();
        win.close();
    }

    win.closeIndicator = closeIndicator;

    return win;
}

// Public interface
exports.createIndicatorWindow = createIndicatorWindow
{% endcodeblock %}

This would then be called from inside an event listener like this:
{% codeblock lang:javascript %}
var uie = require('lib/UiElements');
var indicator = uie.createIndicatorWindow();

someViewObject.addEventListener('click', function(e) {
    indicator.openIndicator();

    setTimeout(function() {
        // Do the work that takes a while
        // and requires an activity indicator
        indicator.closeIndicator();
    },0);
});
{% endcodeblock %}
