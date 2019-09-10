---
layout: radar
---

<svg id="radar" viewBox="0 0 60 55"></svg>

<script>
radar_visualization({
  svg_id: "radar",
  width: 1450,
  height: 1000,
  colors: {
    background: "#fff",
    grid: "#bbb",
    inactive: "#ddd"
  },
  title: "PSD Tech Radar",
  quadrants: [
    { name: "Languages" },
    { name: "Infrastructure" },
    { name: "Frameworks" },
    { name: "Data Management" }
  ],
  rings: [
    { name: "ADOPT", color: "#93c47d" },
    { name: "TRIAL", color: "#93d2c2" },
    { name: "ASSESS", color: "#fbdb84" },
    { name: "HOLD", color: "#efafa9" }
  ],
  print_layout: true,
  // zoomed_quadrant: 0,
  //ENTRIES
  entries: [
      {
        quadrant: 3,
        ring: 0,
        label: "Spark",
        active: false,
        link: "../data_processing/spark.html",
        moved: 0
      },
  ]
  //ENTRIES
});
</script>

### What is the Tech Radar?

The PSD Tech Radar is a list of technologies, complemented by an assessment result, called **ring assignment**. We use four rings with the following semantics:

* **ADOPT**: Technologies we have high confidence in to serve our purpose, also in large scale. Technologies with a usage culture in our Zalando production environment, low risk and recommended to be widely used.
* **TRIAL**: Technologies that we have seen work with success in project work to solve a real problem; first serious usage experience that confirm benefits and can uncover limitations. TRIAL technologies are slightly more risky; some engineers in our organization walked this path and will share knowledge and experiences.
* **ASSESS**: Technologies that are promising and have clear potential value-add for us; technologies worth to invest some research and prototyping efforts in to see if it has impact. ASSESS technologies have higher risks; they are often brand new and highly unproven in our organisation. You will find some engineers that have knowledge in the technology and promote it, you may even find teams that have started a prototyping effort.
* **HOLD**: Technologies not recommended to be used for new projects. Technologies that we think are not (yet) worth to (further) invest in. HOLD technologies should not be used for new projects, but usually can be continued for existing projects.

### What is the purpose?

The Tech Radar is a tool to inspire and support engineering teams at PSD to pick the best technologies for new projects; it provides a platform to share knowledge and experience in technologies, to reflect on technology decisions and continuously evolve our technology landscape. Based on the pioneering work of [ThoughtWorks](https://www.thoughtworks.com/radar), our Tech Radar sets out the changes in technologies that are interesting in software development - changes that we think our engineering teams should pay attention to and consider using in their projects.

### Visualization

The vizualization is based on an Open Source project from Zalando: [https://github.com/zalando/tech-radar]()
