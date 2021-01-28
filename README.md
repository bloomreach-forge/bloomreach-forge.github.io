
# Generate

Generate the Bloomreach Forge Documentation site from Markdown files at /src:  

> mvn site:site 

The Maven site plugin's `<outputDirectory>` is `${project.basedir}` so the all files are placed in the root.

# Publish

Publish the documentation by
- commit all changed files: *.html in the root as well as all files below css, images, img, js and src directories
- push to Github

