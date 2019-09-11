# Welcome to the TechBlog of ProSiebenSat.1 Digital GmbH

This site is from software engineers for software enginners. It contains a growing number of ressources on how we develop software within ProSiebenSat.1 Digital - or short PSD. It covers guidelines, processes, best practices, technology and other stuff software engineers may be interested in. 

### Recent Blog Articles

<section class="post-list">
  {% for post in site.posts limit:3 %}
    <section class="post"> 
      {% if post.image %}
        <img class="image" src="{{ site.baseurl }}/images/posts/{{ post.image }}"/>
      {% endif %}  
        <p class="title"> <a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a> </p>
        <p class="date"> Date: {{ post.date | date: '%d.%m.%Y' }} </p>
        <p class="author"> Author: {{ post.author }} </p>
        <p class="excerpt"> {{ post.content | strip_html | truncatewords: 50 }} </p>
    </section>
  {% endfor %}
</section>

[-> All blog articles](blog)


### More Information

* [Tech Radar](radar): The Tech Radar is a tool to inspire and support engineering teams at PSD to pick the best technologies for new projects as well as to show the PSD tech stack to outside world.
* [Engineering Guidelines](engineering_guidelines/): These Engineering Guidelines give an overview of how we want to work together, develop and operate our software successfully within the PSD.
* [Open Source](opensource): Here you can find the links to a growing number of our open source projects
 
