# H2O-Site

Site for [H2O](https://www.github.com/OutlierVentures/H2O).

Requires Ruby 2.3+, Jekyll 3.6+, gcc and make.

## Develop

```
bundle install
bundle exec jekyll serve
```

## Theme: Jasper

This is a port of Ghost's default theme [Casper](https://github.com/tryghost/casper) for Jekyll inspired by [Kasper](https://github.com/rosario/kasper).


### Deployment

**Important:**  For security reasons, Github does not allow plugins (under _plugins/) when deploying with Github Pages. This means:

**1)** that we need to generate your site locally (more details below) and push the resulting HTML to a Github repository;

**2)** built the site with [travis-ci](https://travis-ci.org/) (with goodies from [jekyll-travis](https://github.com/mfenner/jekyll-travis)) automatically pushing the generated *_site/* files to your *gh-pages* branch.
 This later approach is the one I am currently using to generate the live demo.

For option **1)** simply clone this repository (*master branch*), and then run `bundle exec jekyll serve` inside the directory. Upload the resulting *_site/* contents to your repository (*master branch* if uploading as your personal page (username.github.io) or *gh-pages branch* if uploading as a project page (as for the [demo](https://github.com/jekyller/jasper/tree/gh-pages)).

For option **2)** you will need to set up travis-ci for your personal fork. Briefly all you need then is to change your details in *[\_config.yml](_config.yml)* so that you can push to your github repo. You will also need to generate a secure key to add to your *[.travis.yml](.travis.yml)* (you can find more info on how to do it in that file). Also make sure you read the documentation from [jekyll-travis](https://github.com/mfenner/jekyll-travis). This approach has clear advantages in that you simply push changes to your files and all the html files are generated for you. Also you get to know if everything is still fine with your site builds. Don't hesitate to contact me if you still have any issues (see below about issue tracking).
