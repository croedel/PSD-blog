---
nav-title: Blog
author: Christian Rödel
image: radar.jpg
---

<h1> Recent blog articles </h1>

{% for post in site.posts %}
  <section class="post"> 
    {% if post.image %}
      <img class="image" src="{{ site.baseurl }}/assets/images/{{ post.image }}"/>
    {% endif %}  
      <span class="title"> <a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a> </span>
      <span class="date"> {{ post.date | date: '%d.%m.%Y' }} </span>
      <span class="author"> {{ post.author }} </span>
      <p class="excerpt"> {{ post.content | strip_html | truncatewords: 50 }} </p>
  </section>
{% endfor %}
