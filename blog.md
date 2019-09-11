---
nav-title: Blog
---

# Recent blog articles

{% for post in site.posts %}
### [{{ post.title }}]({{ post.url | prepend: site.baseurl }})
_{{ post.date | date: '%d.%m.%Y' }}_

{{ post.content | strip_html | truncatewords: 50 }}

{% endfor %}
      
