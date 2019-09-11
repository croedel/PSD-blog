---
nav-title: Blog
author: Christian Rödel
---

<h1> Recent blog articles </h1>

{% for post in site.posts %}
  <section class="post"> 
    {% if post.image %}
      <img class="image" src="{{ site.baseurl }}/assets/images/{{ post.image }}"/>
    {% endif %}  
      <span class="title"> <a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a> </p>
      <span class="date"> {{ post.date | date: '%d.%m.%Y' }} </p>
      <span class="author"> {{ post.author }} </p>
      <p class="excerpt"> {{ post.content | strip_html | truncatewords: 50 }} </p>
  </section>
{% endfor %}
