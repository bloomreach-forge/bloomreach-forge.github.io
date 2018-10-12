
## Using git flow

We recommend to use git-flow as branching model in Hippo Forge projects because it is easier for most people to 
understand and follow and also widely accepted. This page explains how to set up ```git flow``` and how to release and 
deploy artifacts.

The instructions are based on these available references:

- http://danielkummer.github.io/git-flow-cheatsheet/
- https://gitversion.readthedocs.io/en/latest/git-branching-strategies/gitflow-examples/
- http://nvie.com/posts/a-successful-git-branching-model/

### Git flow initialization

In order to use ```git flow``` in your local workspace folder, do the following first, once:

- Have two branches, ```master``` and ```develop```, in prior.
- Initialize your working space once for the first time by running ```git flow init``` and accepting the default settings.
- It is also recommended to set the default branch to ```develop```. Change the settings of the project to change the default branch
to ```develop```.

### Maintenance support branches

If you want to have maintenance branches supporting older versions, you can use ```git flow support``` subcommand. You 
create it from a tag or a commit hash.

    git flow support start x.y tag-x.y.z
    
The above creates a branch ```support/x.y``` from a tag called 'tag-x.y.z'. So for instance, if develop is on 
2.0.0-SNAPSHOT, you can create a support/1.x branch from latest tag 1.5.0.
  
Git flow is only about branching so you must set the correct SNAPSHOT versions manually and commit, push it.  
    
### Releasing with git flow (summary)

From develop (or from support/x.y) branch, create release x.y.z: 

1. Start: ```git flow release start x.y.z develop``` (or from ```support/x.y```) 
2. Set release version: ```mvn versions:set -DnewVersion="x.y.z"```, also for demo. 
3. Commit: ```git commit -a -m "<ISSUE_ID> releasing x.y.z: set version"```
4. Finish: ```git flow release finish x.y.x```
5. Set next version: ```mvn versions:set -DnewVersion="x.y.z+1-SNAPSHOT"```, also for demo.
6. Commit: ```git commit -a -m "<ISSUE_ID> releasing x.y.z: set next development version"```
7. Push: ```git push origin develop``` (or ```support/x.y```) 

When started from ```develop```, ```master``` is now same as the release. Then from master:

1. Regenerate docs: usually ```mvn clean site -Pgithub.pages``` 
2. Commit: ```git commit -a -m "<ISSUE_ID> releasing x.y.z: regenerate docs"```
3. Push: ```git push origin master```

Push the tag & deploy artifacts:

1. Push: ```git push origin x.y.z```
2. Check out: ```git checkout tags/x.y.z```
3. Deploy artifacts: ```mvn deploy```


### Releasing with git flow (extended)

##### Start a new release

With ```git flow release start``` a release branch is created. You can do that from ```develop``` branch (as in the this 
example) but also from ```support/x.y``` branch.

        git flow release start x.y.z develop

Now, you're automatically moved to a new branch already, ```release/x.y.z```.

##### Set release versions

It's time to set a proper release version in the pom.xml files. For example,

        mvn versions:set -DgenerateBackupPoms=false -DnewVersion="x.y.z"

Also, if you have ```demo``` folder underneath, set the release version in the demo folder as well:

        cd demo
        mvn versions:set -DgenerateBackupPoms=false -DnewVersion="x.y.z"
        cd ..

**NOTE**: ```demo``` is a child folder managed in the same git repository, but not a maven submodule.
            That's why you set the maven versions separately.

Let's commit the version changes. For example,

        git commit -a -m "<ISSUE_ID> releasing x.y.z: set version"

##### (Optionally) publish the release branch

You can skip this step if you are releasing it alone, but if you want your team to do final hardening in the shared 
release branch, you should do this step.

        git flow release publish x.y.z

Now, your release branch was published. You can inform the team of this new release branch for final hardening work.

##### Finish the release

Finally, let's finish the release. This step includes making a tag, merging changes to master and develop branches
and removing the release branch.

        git flow release finish x.y.z

This finishing step removes the ```release/x.y.z``` branch *locally*, and moves to ```develop``` branch.

##### Bump version in develop branch and push it.

In the finishing step above, every change was already merged to ```master``` and ```develop``` branch from the
release branch. So, don't forget to push local commits to the remote.

Before pushing the commits in ```develop``` branch, let's bump the version for next development cycle with ```-SNAPSHOT```
in ```develop``` branch.

        mvn versions:set -DgenerateBackupPoms=false -DnewVersion="x.y.z+1-SNAPSHOT"

Again, don't forget bumping up versions in the demo folder if it exists.

        cd demo
        mvn versions:set -DgenerateBackupPoms=false -DnewVersion="x.y.z+1-SNAPSHOT"

        cd ..
        git commit -a -m "<ISSUE_ID> releasing x.y.z: set next development version"

        git push origin develop

##### Push master branch and bump up version in develop branch and push it.

If the release was done from develop branch, not from support branch, ```master``` should be the same as the release
branch. Next is to generate docs (if applicable) and commit, push.

        git checkout master
        mvn clean site -Pgithub.pages
        git commit -a -m "<ISSUE_ID> releasing x.y.z: regenerate docs"```
        git push origin master

##### Push the newly created tag.

In the finishing step, a tag was made for the release version. e.g, ```x.y.z```

You can check all the tags by the following command:

        git tag -l

**Tip**: You can also run the following command to fetch all the tags pushed by someone else in the remote:

        git fetch --all --tags --prune

Now, let's push the newly created tag generated in the finishing step:

        git push origin x.y.z

If you've published the release branch in the previous optional step, you may now remove the remote (temporary) release branch:

        git push origin :release/x.y.z

Now, your normal release process is done. Your new release was made as a tag and published.

##### Deploy the release tag to Hippo Forge Maven Repository

Check out the release tag like the following example. You may name the local branch to anything else.

        git checkout tags/x.y.z -b tag-x.y.z

Now, deploy it to Hippo Forge Maven repository from the tag branch:

        mvn deploy

You may remove the local tag branch.

        git checkout develop
        git branch -D tag-x.y.z

Congratulations! Great work!
