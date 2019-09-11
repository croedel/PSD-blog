---
nav-title: Blog
author: Christian Rödel
---

<h1> Recent blog articles </h1>

{% for post in site.posts %}
<h3 class="post-title"> <a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a> </h3>
  <section class="post-date"> {{ post.date | date: '%d.%m.%Y' }} </section>
  <section class="post-author"> {{ post.author }} </section>
  <img class="post-image" src="{{ site.baseurl }}/assets/images/{{ post.image }}"/>
  <section class="post-excerpt"> {{ post.content | strip_html | truncatewords: 50 }} </section>
  
{% endfor %}
      
