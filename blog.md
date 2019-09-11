---
nav-title: Blog
---

# Recent blog articles

{% for post in site.posts %}
### [{{ post.title }}] ({{ post.url }})
_{{ post.date | date: '%B %d, %Y' }}_

{{ post.excerpt }}
{% endfor %}
      
