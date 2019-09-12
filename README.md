# PSD-blog

This is a GitHub Pages repo containing the PSD TechBlog Website.

Within this site you can maintain:
* Blog posts
* Structured, permanent documentation
* TechRadar

## Blog posts
Follow these steps for creating a post:

- Create a file with `_posts` directory. Naming convention is `YYY-MM-DD-<Name>.md`. E.g. `2019-09-12-test.md`
- With the header you should add 
```---
layout: post
title: <Title of your article>
author: <Your name>
summary: <Summary>  
image: <Filename of image>
---
```
The image will be shown along your article. If you don't specify it, a default will be shown. The image file needs to be uploaded to `images/posts/` directory.
- Write your article as markdown.

## Structured documentation
- Structure them in an appropriate directory structure.
- Put an `index.md` file within each directory
- Create additional markdown files within this directory structure as appropriate

## Tech Radar
The TechRadar is auto-generated from a YUML file: `_data/tech_radar.yml`
In order to maintain the entries you just need to edit this file accordingly.
