---
layout: post
title: Thinking Process - URL Shortener
excerpt: Thinking process of making a URL Shortner
categories: articles
tags: [thinking, systems-design, development, url-shortener, systems-analysis]
comments: true
share: true
modified: 2017-08-26 19:24:54.454961
---

Sooo...

This is the first set of a new Series I'd like to call "The Thinking Process".
Two things to note.

1. Opinions expressed here are purely mine.
2. I'm not asserting this is the best way to it, but this is how I would look at it

# Problem Statement: URL Shortening Service 
We are currently faced with the task of creating a service that will give the shortened verison of any url that is sent to it. Simple yh?

We need this service to
- Save a long URL in its database and return the shortened version
- Uniquely identify shortened urls and return the longer version.

Assumptions
- This Is going to be a public 


# Sample Request
## Sending a URL to the service to shorten
POST http://imageshorten.com/api/shorten/
```json
{
"data_url":"http://thisisadamlongurlnoonehasthetimetotyprinthebrowser.com/getthisright"
}
```

| Parameter     | Default       | Description                               |
| :------------ |:--------------| :-----------------------------------------|
| data_url      | null          | The URL for the service to shorten        |

### Via [Curl](https://curl.haxx.se/docs/manpage.html)
```bash
curl -X POST \
  http://imageshorten.com/api/shorten/ \
  -H 'content-type: application/json' \
  -d '{
"data_url":"http://thisisadamlongurlnoonehasthetimetotyprinthebrowser.com/getthisright"
}'
```

### Expected response
```json
 {
    "message":"URL Shortened Successfully",
    "data":{
        "id": 1,
        "created":true,
        "short_url":"http://sho.rt/w3uye1/"
    }
  }
```

