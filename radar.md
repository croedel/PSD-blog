---
layout: radar
nav-title: Tech Radar
---

# PSD Tech Radar 

<svg class="radar" id="radar"></svg>

<script>
radar_visualization({
  svg_id: "radar",
  width: 1200,
  height: 800,
  colors: {
    background: "#fff",
    grid: "#9a9a9a",
    inactive: "#ddd"
  },
  quadrants: [
    { name: "Languages & Frameworks" },
    { name: "Tools" },
    { name: "Platforms" },
    { name: "Techniques" }
  ],
  rings: [
    { name: "ADOPT", color: "#78bd59", background: "hsl(221, 100%, 93%)" },
    { name: "TRIAL", color: "#47c9a7", background: "hsl(221, 100%, 95%)" },
    { name: "ASSESS", color: "#f7d26d", background: "hsl(221, 100%, 97%)" },
    { name: "HOLD", color: "#eb8178", background: "hsl(221, 100%, 99%)" }
  ],
  print_layout: true,
  // zoomed_quadrant: 0,
  //ENTRIES
// entries: [
//     {
//        quadrant: 3,
//        ring: 0,
//        label: "Spark",
//        moved: 0
//      },
//  ]
  entries: [
  {% for quadrant in site.data.tech_radar %}
    {% for ring in quadrant.rings %}
        {% for item in ring.items %}
            {
                quadrant: {{ quadrant.quadrant }},
                ring: {{ ring.ring }},
                label: "{{ item.label }}",
                moved: {{ item.moved }},
            },    
          {% endfor %}
      {% endfor %}
    {% endfor %}
    ]
  //ENTRIES
});
</script>

---

### What is the Tech Radar?

The Tech Radar shows most of the technologies, techniques and tools we’re using at PSD.  

We clustered them in rings and quadrants, just as the folks at [ThoughtWorks](https://www.thoughtworks.com/radar) do. The visualization is based on an Open Source project from [Zalando](https://github.com/zalando/tech-radar).

### Rings

We use four rings with the following semantics:

* **ADOPT**: Technologies we have high confidence in to serve our purpose, also in large scale. Technologies with a usage culture in our Zalando production environment, low risk and recommended to be widely used.
* **TRIAL**: Technologies that we have seen work with success in project work to solve a real problem; first serious usage experience that confirm benefits and can uncover limitations. TRIAL technologies are slightly more risky; some engineers in our organization walked this path and will share knowledge and experiences.
* **ASSESS**: Technologies that are promising and have clear potential value-add for us; technologies worth to invest some research and prototyping efforts in to see if it has impact. ASSESS technologies have higher risks; they are often brand new and highly unproven in our organisation. You will find some engineers that have knowledge in the technology and promote it, you may even find teams that have started a prototyping effort.
* **HOLD**: Technologies not recommended to be used for new projects. Technologies that we think are not (yet) worth to (further) invest in. HOLD technologies should not be used for new projects, but usually can be continued for existing projects.

### Quadrants

The four quadrants categorize the technology or technique:

* **Programming Languages and Frameworks**: Programming languages and frameworks as you would expect.
* **Tools**: These can be components, such as databases, software development tools, such as versions control systems; or more generic categories of tools, such as the notion of polyglot persistence.
* **Platforms**: Things that we build software on top of such as mobile technologies like Android, virtual platforms like the JVM, or generic kinds of platforms like hybrid clouds.
* **Techniques**: These include elements of a software development process, such as experience design; and ways of structuring software, such as microservices.
