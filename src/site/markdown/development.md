
## Development

On the Hippo Forge, plugins are developed and maintained by the Hippo Community. However, since Hippo will not give push 
rights to external developers, we like code contributions! 

Also, plugins can be maintained during the course of an implementation project. For instance, it is pretty common for a 
plugin to be upgraded during a project upgrade. This also goes for new features or bug fixing.

### Code Contributions
On GitHub, code contributions exist in the form of pull requests. External organisations can always drive those pull 
requests, or feature requests, or release requests by contacting the 
[Hippo services department](https://www.onehippo.com/en/professional-services).

See also the [GitHub documentation on contributing](https://guides.github.com/activities/contributing-to-open-source/#contributing), 
basically forking and developing in a separate branch.

### Forking
Forking into other (non-Hippo) GitHub locations is of course a possibility for further development on behalf of your 
project as well. In fact, a fork is the starting point of a pull request!  

See also the [GitHub documentation on forking](https://guides.github.com/activities/forking/).

### Branching Strategy
We like to adhere to the the well-established [Git Flow by Vincent Driessen](http://nvie.com/posts/a-successful-git-branching-model/).

In short, the 'master' branch is always the same as the latest release, whilst the 'develop' branch is used as base for 
features and bugfixes. We intend to write some more detailed documentation on how to handle branching and releasing.

### Artifact Repository
Artifacts can be retrieved from the Hippo Maven Forge Repository at [maven.onehippo.com/maven2-forge/](http://maven.onehippo.com/maven2-forge/).

For publishing, the Maven `distributionManagement.repository.url` value is to be set at `https://maven.onehippo.com/content/repositories/forge-releases/`. 

Maybe later we will apply for a spot in Maven Central.

### Issue Tracking
Hippo's JIRA at [issues.onehippo.com/browse/HIPFORGE](https://issues.onehippo.com/browse/HIPFORGE).

### Continuous Integration
Travis at [travis-ci.org/onehippo-forge/](https://travis-ci.org/onehippo-forge/).
