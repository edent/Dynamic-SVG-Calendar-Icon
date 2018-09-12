# This SVG always shows today's date

[Originally posted on my blog](https://shkspr.mobi/blog/2018/02/this-svg-always-shows-todays-date/).

For [my contact page](https://edent.tel/), I wanted a generic calendar icon to let people view my diary.  Calendar icons are almost always a skeuomorph of a paper calendar, but I wondered if I could make it slightly more useful by creating a *dynamic* icon.

Here it is, <a href="https://shkspr.mobi/svg/calendar.svg">an SVG calendar which always display's today's date</a>.

(Note - GitHub doesn't allow data URIs or iFrames - so you'll need to [click the link to see it](https://shkspr.mobi/svg/calendar.svg))

The background image is derived from the [Twitter TweMoji Calendar icon](https://github.com/twitter/twemoji/blob/gh-pages/2/svg/1f4c5.svg) - CC-BY.

[![Buy me a coffee](https://www.ko-fi.com/img/donate_sm.png)](https://ko-fi.com/edent)

## Use
SVG supports JavaScript. Browsers will only run the JavaScript if the SVG is included...

* in an iframe `<iframe src="calendar.svg"></iframe>`
* as inline `<body><svg>.....</svg></body>`

It will not run JavaScript in an `<img>` element.

## HOWTO

Text support in SVG is a little awkward, so let me explain how I did this.

SVG supports JavaScript. This will run as soon as the image is loaded.

```
<svg onload="init(evt)" xmlns="http://www.w3.org/2000/svg"
aria-label="Calendar" role="img" viewBox="0 0 512 512">
```

Next step is to get the various date strings. I'm using the `en-GB` locale as that's where I'm based.

```
<script type="text/ecmascript"><![CDATA[
function init(evt) {
  var time = new Date();
  var locale = "en-gb";
```

I want to display something like "Sunday 25 FEB" - the locale options allow for short and long names. So you could have "SUN 25 February".

```
  var DD   = time.getDate();
  var DDDD = time.toLocaleString(locale, {weekday: "long"});
  var MMM = time.toLocaleString(locale,  {month:   "short"});
```

Finally, we need to add the text on to the image.
```  
  var svgDocument = evt.target.ownerDocument;

  var dayNode = svgDocument.createTextNode(DD);
  svgDocument.getElementById("day").appendChild(dayNode);

  var weekdayNode = svgDocument.createTextNode(DDDD);
  svgDocument.getElementById("weekday").appendChild(weekdayNode);

  var monthNode = svgDocument.createTextNode(MMM.toUpperCase());
  svgDocument.getElementById("month").appendChild(monthNode);
  
}
]]></script>
```

Text positioning is relatively simplistic.  An X & Y position which is anchored to the *bottom* of the text - remember that letters with descenders like `g` will extend beyond the bottom of the Y co-ordinate.  This is also where we set the colour of the text, its size, and a font. 

A monospace font makes it easier to predict the layout.
```
<text id="month"
  x="32" 
  y="164" 
  fill="#fff" 
  font-family="monospace"
  font-size="140px"
  style="text-anchor: left"></text>
```

A word on anchoring.  To centre the anchor, use `style="text-anchor: middle"`

A quick test shows that this works on all desktop browsers and Android browsers. I've not tested on iPhones or anything more exotic.

Enjoy!
