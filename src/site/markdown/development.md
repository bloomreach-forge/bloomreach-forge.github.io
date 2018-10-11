
## Development

On the Hippo Forge, plugins are developed and maintained by the Hippo Community, consisting of both developers from 
BloomReach, the owner of Hippo CMS, and external developers, from partners and clients.  

Forge projects can be maintained during the course of an implementation project. For instance, it is pretty common 
for a plugin to be upgraded during a project upgrade. This also goes for new features or bug fixing.

### Code Contributions
Code contributions are very much welcome! On GitHub, they exist mostly in the form of pull requests. External 
organizations can drive those pull requests, or feature requests, or release requests by contacting the 
[BloomReach Services department](https://www.bloomreach.com/en/services/s).

See the [GitHub documentation on contributing](https://guides.github.com/activities/contributing-to-open-source/#contributing), 
basically forking and developing in a separate branch.

### Forking
Forking into other (non-Forge) GitHub locations is of course a possibility for further development on behalf of your 
project. In fact, a fork is the starting point of a pull request!  

See also the [GitHub documentation on forking](https://guides.github.com/activities/forking/).

### Outside Collaborators
External developers can also apply, or be asked for, the GitHub role of 'Outside Collaborator' of a project. If that
occurs, an external developer has push rights on the repository.      

### Branching Strategy
We like to adhere to the the well-established [Git Flow by Vincent Driessen](http://nvie.com/posts/a-successful-git-branching-model/).

In short, the 'master' branch is always the same as the latest release, whilst the 'develop' branch is used as base for 
features and bug fixes. 

### Artifact Repository
Artifacts can be retrieved from the Hippo Maven Forge Repository at [maven.onehippo.com/maven2-forge/](http://maven.onehippo.com/maven2-forge/).

For publishing, the Maven `distributionManagement.repository.url` value is to be set at `https://maven.onehippo.com/content/repositories/forge-releases/`.
Only BloomReach developers have the access rights to publish it here.  

### Issue Tracking
Hippo's JIRA at [issues.onehippo.com/browse/HIPFORGE](https://issues.onehippo.com/browse/HIPFORGE).

### Continuous Integration
Travis at [travis-ci.org/onehippo-forge/](https://travis-ci.org/onehippo-forge/).
