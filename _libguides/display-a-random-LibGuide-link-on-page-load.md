---
layout: implementations
title: 'Display a Random LibGuide Link on Page Load'
author: "Brad Coffield"
author_email: "bcoffield@gmail.com"
#tags:
#- javascript
#- jquery
#- client-side
---

## Description

This code will pull a random guide from your system's published guides and display it as a simple link. Also, there's a button that will regenerate the randomized guide.

### Screenshot

![{{page.title}} screenshot]({{site.baseurl}}/assets/{{page.title}}-screenshot.jpg){:.screenshot}

### More details

The API call, which is written in jQuery, will send your authorization key along with some pieces of information that will tell LG to only return JSON data about guides from a single subject or subjects.

Once that data is returned we pull out the title of the guide and also its URL and place those pieces into a link element. The jQuery will then enumerate through each JSON entry doing the same thing again and again until complete. You might have a list of 3 items or 300, it doesn't matter.

You link up the jQuery and your actual HTML using a specific ID name in your HTML, assigned to an `<a>`.

Pressing the button will re-run everything and display a new link.

## The Code

#### HTML

{:.code-notes}

* Line 3 HTML: This shows the link.
* Line 4 HTML: This generates the button which, when clicked, will re-roll the guide.

{% highlight html linenos %}

<div id="random-guide-container">
    <h4>Have you considered?</h4>
    <div id="foo"></div>
    <input type="button" id="btn" value="Try Another" />
</div>

{% endhighlight %}

#### CSS

{% highlight css linenos %}
#random-guide-container {font-family: 'Martel', serif; font-size: 1.2em;text-align:center;}
#random-guide-container #btn {font-size: .7em;}
#btn { margin-top: .7em; display: inline-block; line-height: 1.25; text-align: center; vertical-align: middle; background: #FFF; border: 1px solid #777; padding: .5rem 1rem; border-radius: .25rem; }

{% endhighlight %}

#### JavaScript/jQuery

##### Notes for implementation:

{:.code-notes}

* Line 3: `foo1` and `foo2` are placeholders. Fill these in with the information provided by Springshare.
* Line 3: `status=1` pulls JSON data for only your published guides.
* Line 4: This is the randomizing part.
* Line 5: Builds the actual link pulling in the url and name from the JSON data returned from the API call.
* Line 6: Attaches to the `#foo` and places the link we've built.
* Line 8: Runs the above code so that a link appears on page load.
* Line 9: Creates the button and makes the code run again on click.

##### The code itself:

{% highlight javascript linenos %}
var apis4librarians_randomLibguide = function(){$(document).ready(function () {
var getRandomGuide = function () {
$.getJSON('https://lgapi-us.libapps.com/1.1/guides?site_id=foo1&key=foo2&status=1', function (result) {
var entry = result[Math.floor(Math.random() * result.length)];
var randomGuide = '<a href="' + entry.url + '">' + entry.name + '</a>';
$('#foo').html(randomGuide);
}); };
getRandomGuide();
$('#btn').click(function () {getRandomGuide()}); });}();
{% endhighlight %}
