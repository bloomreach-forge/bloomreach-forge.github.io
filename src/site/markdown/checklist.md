
## Quality Checklist

This page contains a quality checklist for community maintained projects hosted at the Hippo Forge. If your project 
meets all items in this list, it rocks!

### 1. Clear project name
Give your project a name that makes it unique and that explains what it does. If possible, make the name stand out of 
other projects that do the same thing but in a different way.

### 2. Documentation available 
Without documentation the plugin doesn't exists! And screenshots rule!

We use [GitHub Pages](https://pages.github.com/) to publish documents from the `master/docs` folder to a documentation 
site at `https://onehippo-forge.github.io/[project-name]/`

### 3. Artifact group and package 
To distinct from other plugins, use an artifact group id and Java package that both start with `org.onehippo.forge.[project-name]`. 
In the project name only use lowercase letters [a-z].

### 4. Use project pom as parent
Use the project pom `org.onehippo.cms7:hippo-cms7-project` as a parent to the plugin project. It provides a number of 
standard pom sections and has some basic dependencies defined that are common to all Hippo projects. 

The release pom `org.onehippo.cms7:hippo-cms7-release` is extended from that and defines too much (like standard plugin 
versions, Cargo configuration) to be used as parent pom. It is meant for implementation (end) projects. 

### 5. Demo project available 
Provide an example demo project within the code as Maven submodule, to demonstrate the functionality.

### 6. Delivery tier best practices
To the plugin's delivery tier (HST) code, apply our [best practices](https://www.onehippo.org/library/setup/best-practices.html).

Some key point are: use limits on queries, have translatable labels, make smart usage of logging. 

### 7. Hide any namespace
If the plugin defines a namespace and that namespace is not intended for editing in the CMS, make sure to hide it. 
See [Hide a Namespace in the CMS editor](https://www.onehippo.org/library/concepts/plugins/hide-a-namespace-in-the-cms-editor.html) 
for how this can be achieved.

### 8. Clustering
Make sure the plugin runs within a clustered environment, preferably without additional configuration.
