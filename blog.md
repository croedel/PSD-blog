---
nav-title: Blog
breadcrumb: Blog
---

<h1> Our blog articles </h1>

<section class="post-list">
  {% for post in site.posts %}
    <section class="post-teaser"> 
        {% if post.image %}
          <a href="{{ post.url | prepend: site.baseurl }}"> <img class="image" src="{{ site.baseurl }}/images/posts/{{ post.image }}"/> </a>
        {% else %}
          <a href="{{ post.url | prepend: site.baseurl }}"> <img class="image" src="{{ site.baseurl }}/images/blog.jpg"/> </a>
        {% endif %}    
        <p class="title"> <a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a> </p>
        <p class="date"> Date: {{ post.date | date: '%d.%m.%Y' }} </p>
        <p class="author"> Author: {{ post.author }} </p>
      {% if post.summary %}
        <p class="excerpt"> {{ post.summary }} </p>
      {% else %}
        <p class="excerpt"> {{ post.content | strip_html | truncatewords: 50 }} </p>
      {% endif %}
    </section>
  {% endfor %}
</section>
