# jekyll-local-diagram

# Quickstart

## Local Jekyll 

For a local Jekyll host you should add the following to your `Gemfile`

    group :jekyll_plugins do
      ...
      gem "jekyll-local-diagram"
    end

It should also appear in your `_config.yml` file thus:

    plugins:
    - jekyll-local-diagram

Then either run:
    
    gem install jekyll-local-diagram
    
 Or run 
 
    bundle update

### Prerequisites 

In order to draw the diagrams locally the plugin assumes that you have the following applications installed and in the current path that is running `jekyll-local-diagram`.

* `java` - i.e. any old Java JRE capable of running PlantUML
* `mmdc` - the NPM command for the mermaid CLI [mermaid.cli](https://www.npmjs.com/package/mermaid.cli)
* `dot`- the Grahviz executable that PlantUML uses
* `tex2vg` - the MathJax CLI [tex2svg](https://www.npmjs.com/package/tex2svg)


### PlantUML Library Upgrade

The `jekyll-local-diagram` plugin will install the latest version of the PlantUML jar when the gem is built.  When installing it from (RubyGems)[https://rubygems.org].

In order to use a specific version of the PlantUML jar you can package your own Gem and install it.

## Github Pages / Docker

`jeyll-local-diagram` was written to be used with GitHub Pages.  However GitHub Pages only allows certain types of plugin to be used that are considered safe.  

In order to use this gem with GitHub pages requires the use of a GitHub build action capable of checking out the repository with the documentation.

 [jekyll-local-diagram-build-action](https://github.com/hackinghat/jekyll-local-diagram-build-action) was written for this purpose and provides the necessary pre-requisites to a docker container that `jekyll-local-diagram` can run inside.


# What

This is a [Jekyll](https://jekyllrb.com) Plugin that will generate SVGs as static assets that can be embedded into a Jekyll site.

# Why?

If you have a private repository on GitHub and are using [GitHub Pages](https://pages.github.com) to host your documentation but also want to make use of text based diagramming tools to inline diagrams (within your markdown) then you will likely be drawn to using something like [Jekyll Spaceship](https://github.com/jeffreytse/jekyll-spaceship) or[Jekyll PlantUML](https://github.com/yegor256/jekyll-plantuml).

## Diagrammatic support

However not all diagramming tools are created equal, some have broad support for different diagrams (i.e. [Jekyll Spaceship](https://github.com/jeffreytse/jekyll-spaceship)) and some support just one (i.e. [Jekyll PlantUML](https://github.com/yegor256/jekyll-plantuml)).  For my use case I needed:

* [PlantUML](https://plantuml.com)
* [GraphViz](https://graphviz.org/documentation/)
* [Mermaid](https://mermaid-js.github.io/mermaid/#/)
* [LaTeX maths expressions](https://en.wikibooks.org/wiki/LaTeX/Mathematics).
* [BPMN diagrams](https://bpmn.io)

## Local assets

Not withstanding this not all diagramming tools produce image assets that are local to the repo.  [Jekyll Spaceship](https://github.com/jeffreytse/jekyll-spaceship) leverages this method pretty much exclusively for [PlantUML](https://plantuml.com) and [Mermaid](https://mermaid-js.github.io/mermaid/#/) diagrams.

To illustrate this consider the following diagram that uses the same technique as [Jekyll Spaceship](https://github.com/jeffreytse/jekyll-spaceship) to display:

![](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuNB9JovMqBLJ2CX9p2i9zVLHi58eACeiIon9LKZ9J4mlIinLI4aiIUI2oOFKWlLOmUIBkHnIyrA0PW40)

The above image is rendered just in time by hitting a service URL at www.plantuml.com which takes part of the URL and extracts a specially encoded string that contains the diagram content.

    https://www.plantuml.com/plantuml/png/SoWkIImgAStDuNB9JovMqBLJ2CX9p2i9zVLHi58eACeiIon9LKZ9J4mlIinLI4aiIUI2oOFKWlLOmUIBkHnIyrA0PW40

The encoding includes some compression and something like [base64 encoding](https://plantuml.com/text-encoding) in order to arrive at as minimal URL as possible.  The approach [Jekyll Spaceship](https://github.com/jeffreytse/jekyll-spaceship) uses for Mermaid diagrams is different but technically similar.

The downsides of managing diagrams in this way are two-fold: 

1. The complexity of the diagram is limited by the length of the URL (circa 2,000 characters) 
2. The content of the diagram is leaked to a third-party by encoding the diagram text in the URL - this is over https but leaks the diagram to the server.

The first issue is the biggest deal-breaker, and indeed diagrams with lots of labels and notes are hard to create and render.  

The second issue is potentially less of a concern but if we're making our repo private we may not want to leak sensitive design details to multiple third parties.  Also knowing that this service could leak technical specs could well make it a target for hackers. 

## GitHub Pages Support

GitHub pages does not allow the inclusion of arbitrary Jekyll plugins which makes neither [Jekyll PlantUML](https://github.com/yegor256/jekyll-plantuml) nor [Jekyll Spaceship](https://github.com/jeffreytse/jekyll-spaceship) usable on GitHub pages without additional components.  

In order to use this plugin with GitHub Pages requires creating custom build actions that can use



# How

The plugin uses the [Liquid](https://shopify.github.io/liquid/) templating tooling that is already installed as part of Jekyll. 

