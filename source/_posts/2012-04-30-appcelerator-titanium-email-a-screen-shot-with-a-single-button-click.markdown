---
layout: post
title: "Appcelerator Titanium - Email a Screen Shot with a Single Button Click"
date: 2012-04-30 16:30
comments: true
categories: [appcelerator, mobile dev, dev, titanium, javascript, ios, testing]
---
During testing it is often helpful if a tester can send you, the developer, a screen shot of a particular window in the app.
Of course this is generally very simple to do on an iOS device right out of the box. However, you can make it even easer for your testers.
<!-- more -->
With a single click of a button in your app you can

* grab the screen content
* save it as an image
* attach it to an email
* prepopulate the message with default values
* allow the user to write a comment and send

Providing all that cuts down on a whole lot of steps and might make it more likely that you get that much needed feedback.

{% codeblock lib/UiElements.js lang:javascript %}
/*
 * For beta testing purposes create a camera button that can be set
 * as the right nav button. Upon clicking create a screenshot and
 * display the email dialog with pre-populated fields.
 * This is a convenience for testers to send a screenshot straight
 * out of the applicaiton.
 */
function createScreenShotButton() {
    var buttonObjects = [
        {image:'images/camera.png', width:45}
    ];

    var buttonBar = Titanium.UI.createButtonBar({
        labels:buttonObjects,
        backgroundColor:'#000'
    });

    var indicator = createIndicatorWindow();

    buttonBar.addEventListener('click', function(e) {
        if (e.index === 0) {
            Ti.Media.takeScreenshot(function(event) {
                indicator.openIndicator();
                var emailDialog = Ti.UI.createEmailDialog()
                emailDialog.subject = "Screenshot from myApp";
                emailDialog.toRecipients = ['my@email-address.com'];
                emailDialog.messageBody = 'Describe the problem please';
                emailDialog.addAttachment(event.media);
                emailDialog.open();
                indicator.closeIndicator();
            });
        }
    });

    return buttonBar;
}

exports.createScreenShotButton = createScreenShotButton;
{% endcodeblock %}

The activity indicator used in this example is described [here](/blog/2012/03/19/appcelerator-titanium-a-nice-looking-activity-indicator-window).

A great place to get free icons for this button is at [glyphish.com](http://glyphish.com/).

To provide this screen shot button on specific windows of your application simply do the following

{% codeblock lang:javascript %}
win = Ti.UI.createWindow({});
uie = require('lib/UiElements');

var bb = uie.createScreenShotButton();
win.setRightNavButton(bb);

// When returning to this window from a child view the nav button gets
// removed by default. This will show it again.
win.addEventListener('focus', function(e) {
    win.setRightNavButton(bb);
});
{% endcodeblock %}


