---
title: API Hacking
date: 2017-03-16T00:00:00.000-05:00
tags:
- projects
- api
- lifehacking
- web
description: Learn how to collect data directly from source with this walkthrough
  of a website dissection.
meta_img: "/img/favicon.png"
hacker_news_id: ''

---
In preparation for a [Spartan Sprint](https://www.spartan.com/) in 2016, I subscribed to their "Workout of the Day." I wanted to pull it up on my phone without having to go to their website or search through my inbox, so I took a closer look at the [Workout of the Day web page](https://www.spartan.com/en/training/wods/spartan-wod) and found an API link I might be able to use.

{{< figure src="https://s3.amazonaws.com/dpfrakes/wod-request.png" alt="screenshot of workout of the day request" caption="You shouldn't have to wait 13 seconds and download 10 MB of ads, images, and popups just to view 630 bytes of text." >}}

First, I searched through the Network tab in Developer Tools, looking for "api" or "wod" or some other single-link origin of the daily workout data. After filtering by XHR and peeking at the JSON responses to several requests, I found the following URL with everything I needed:

```bash
https://api.spartan.com/v6/api/get_page/?route=spartan.training.wod&secondary=spartan-wod&primary=1
```

If you click on this request and view the HTTP response body, you'll see JSON that conveniently contains properly-formatted HTML for each daily workout for the current week, ready to be written straight to a webpage.

Copy that whole link and add it to your webpage's JS file as an AJAX request (code below uses [jQuery](https://api.jquery.com)):

```javascript
$.get('https://api.spartan.com/v6/api/get_page/?route=spartan.training.wod&secondary=spartan-wod&primary=1', function(data) {
  console.log('Retrieved this data from API:');
  console.log(data);
});
```

After successfully retrieving this data, I needed to whittle down the results to find just the workout for a particular day:

```javascript
$.get('https://api.spartan.com/v6/api/get_page/?route=spartan.training.wod&secondary=spartan-wod&primary=1', function(data) {
  var workoutHtml = data.wods[0].post_content;
  document.write(workoutHtml);
});
```

This should replace whatever is currently on your webpage with the Workout of the Day for Sunday ("Day 0") of this week. To find the current day of the week, I just needed to use the native `Date().getDay()` functionality:

```javascript
$.get('https://api.spartan.com/v6/api/get_page/?route=spartan.training.wod&secondary=spartan-wod&primary=1', function(data) {
  var dayOfWeek = (new Date()).getDay();
  var workoutHtml = data.wods[dayOfWeek].post_content;
  document.write(workoutHtml);
});
```

After modifying the callback function to find the workout for the current day of the week, I was pretty satisfied just loading this every day for a while, but realized I could easily optimize it a bit. The first things to go were the unnecessary images.

To conserve screen real estate, and more importantly, decrease data usage, I sabotaged the image requests by changing their filenames before the browser could load them:

```javascript
$.get('https://api.spartan.com/v6/api/get_page/?route=spartan.training.wod&secondary=spartan-wod&primary=1', function(data) {
  var dayOfWeek = (new Date()).getDay();
  var workoutHtml = data.wods[dayOfWeek].post_content;
  workoutHtml = workoutHtml.split('jpg').join('x');
  workoutHtml = workoutHtml.split('png').join('x');
  document.write(workoutHtml);
});
```

If you run the above script in a fresh browser page, you'll see the failed HTTP requests (403 Forbidden) for each image, as the browser tried to get `image.x` instead of `image.jpg`. Additionally, you'll notice a significant decrease in data transferred, as the images are several KB each, whereas 403 responses cost just a few bytes each.

A final cosmetic touch to this lightweight Workout of the Day was to hide the resulting broken images on the page. A simple DOM removal fixes this:

```javascript
$.get('https://api.spartan.com/v6/api/get_page/?route=spartan.training.wod&secondary=spartan-wod&primary=1', function(data) {
  var dayOfWeek = (new Date()).getDay();
  var workoutHtml = data.wods[dayOfWeek].post_content;
  workoutHtml = workoutHtml.split('jpg').join('x');
  workoutHtml = workoutHtml.split('png').join('x');
  document.write(workoutHtml);
  $('img').hide();
});
```

Finally, to isolate this as its own webpage, I created a simple HTML file with no content, added the above code in a `<script>` tag, then [refactored to not even require jQuery](http://youmightnotneedjquery.com/):

```html
<html>
<head></head>
<body>
  <script>
  var request = new XMLHttpRequest();
  request.open('GET', 'https://api.spartan.com/v6/api/get_page/?route=spartan.training.wod&secondary=spartan-wod&primary=1', true);

  request.onload = function() {
    if (request.status >= 200 && request.status < 400) {
      var response = JSON.parse(request.responseText);
      var dayOfWeek = (new Date()).getDay();
      var workoutHtml = response.wods[dayOfWeek].post_content;
      workoutHtml = workoutHtml.split('jpg').join('x');
      workoutHtml = workoutHtml.split('png').join('x');
      document.write(workoutHtml);
      document.querySelectorAll('img').forEach((i) => { i.remove(); });
    }
  };

  request.send();
  </script>
</body>
</html>
```

And that's it. Add it to a webpage or mobile app and you've got free, performance-optimized API content.