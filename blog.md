---
nav-title: Blog
---

## Recent blog articles

{% for post in site.posts %}
		<a href="{{ post.url }}"><h3>{{ post.title }}</h3></a>
		<sub>{{ post.date | date: '%B %d, %Y' }}</sub>
    {{ post.excerpt }}
{% endfor %}
      
