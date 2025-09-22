---
title : 'Figuring out hugo'
description : "First post gonna be my process of figuring out hugo for me to post my writeups"
category : ['writeups']
tags : ['Hugo']
date : 2025-09-17
draft : false
showAuthorBottom: true
---



## First Hugo post
Hello, this will be my first hugo posts basically documenting firstly my experience on getting it to work

I mostly use my arch linux laptop that I use for school but I was setting it up on my desktop using WSL(Windows Sub-System for Linux) a thing to let me run linux things on my windows computer

I was using the Hugo theme 'blowfish' for this setup so some things may not be applicable if you are using another theme.

Ps. some of my links may be outdated if you are viewing this in the far future.



## Installing Hugo
Now this was one of the more tedious parts as I did not know about the different package managers not having the latest versions but the steps are basically:

1. Ensure you have a package manager that has the latest version of Hugo, I used brew(Click for that [here](https://discourse.gohugo.io/t/hugo-0-149-0-released/55805)) because apt didn't have the lastest
2. enter this code to install `brew install hugo`
3. Check youve installed it using `hugo version`

## Creating a new site with a theme
This part could be different based on what theme you chose but I used blowfish as my theme

1. Type `hugo new site <your website name>` to create the directory structure for your site like a skeleton
2. Type `cd <your website name>` to enter that directory
3. Type `git init` to initialise that reposistory into a git repository
4. Was told idk blah blah (link to website for hugo themes [here](https://themes.gohugo.io/))
5. In your current directory you should see a hugo.toml file, delete it and replace it with a folder `config/_default` and then take the .toml files from `themes/<theme name>/config/_default`
and put them inside

## Customising
You can now get to customising and adding things to your website, testing by typing hugo server to set it up locally

1. Check your hugo.toml to set your base url as well as your theme, to prevent confusion this is the theme you just installed like blowfish
2. Check your languages.toml file and edit all the values to your liking such as your name
3. Check your params.toml, here you will be able to set things like light/dark mode, default settings and your theme for website(For all options click [here](https://blowfish.page/docs/getting-started/#blowfish-default))

## Making first post
Most commonly in Hugo your posts are gonna be made in Markdown files which look like this(Cheat sheet in references below):
![Image of markdown](MarkdownExample.png)

1. Make a new folder inside your content folder for neatness so you can keep related posts together e.g Writeups
2. Place a new Markdown file inside and follow the cheat sheet to create your own post
3. You can also create a front header that helps you change the settings of the post it looks like:
```
---
title : 'Figuring out hugo'
description : "First post gonna be my process of figuring out hugo for me to post my writeups"
category : ['writeups']
tags : ['Hugo']
date : 2025-09-17
draft : false
showAuthorBottom: true
---
``` 
All front matter settings [here](https://blowfish.page/docs/front-matter/)

## References
- [Brew package manager](https://brew.sh/) 
- [Hugo quickstart guide](https://gohugo.io/getting-started/quick-start/) 
- [Hugo themes](https://themes.gohugo.io/) 
- [Blowfish theme guide](https://www.youtube.com/watch?v=MX4yy1dTVYg&t=707s) 
- [Markdown cheat sheet](https://www.markdownguide.org/cheat-sheet/) 