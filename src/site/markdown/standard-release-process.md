
## Standard Release Process Using git-flow

This article explains how you can cut a release using ```git-flow``` and deploy the release artifacts.

The instructions are based on ```git-flow```. Please follow available references of ```git-flow``` like the following:

- http://danielkummer.github.io/git-flow-cheatsheet/
- https://gitversion.readthedocs.io/en/latest/git-branching-strategies/gitflow-examples/
- http://nvie.com/posts/a-successful-git-branching-model/

### Preparations to use git-flow

#### Initialize git-flow in the project once

In order to use ```git-flow``` in your local workspace folder, you should do the following first once:

- You should have at least two branches, ```master``` and ```develop```, in prior.
- You must initialize your working space once for the first time by running ```git flow init``` and accepting the default settings.
- It is also recommended to set the default branch to ```develop```. Change the settings of the project to change the default branch
to ```develop```.

### Start a new release

In this instruction, let's suppose we want to cut a release of version 1.2.3 from the ```develop``` branch.
You need to start the release process with the following command:

        git flow release start RELEASE [BASE]

So, for example,

        git flow release start 1.2.3 develop

Now, you're automatically moved to a new branch already, ```release/1.2.3```.

### Set release versions

It's time to set a proper release version in the pom.xml files. For example,

        mvn org.codehaus.mojo:versions-maven-plugin:2.1:set -DgenerateBackupPoms=false -DnewVersion="1.2.3"

Also, if you have ```demo``` folder underneath, set the release version in the demo folder as well:

        cd demo
        mvn org.codehaus.mojo:versions-maven-plugin:2.1:set -DgenerateBackupPoms=false -DnewVersion="1.2.3"
        cd ..

**NOTE**: ```demo``` is a child folder managed in the same git repository, but not a maven subproject.
            That's why you set the maven versions separately.

Let's commit the version changes. For example,

        git commit -a -m "Setting release version to 1.2.3."

### (Optional) Publish the release branch

You can skip this step if you are releasing it alone.
But if you want your team to do final hardening in the shared release branch, you should do this step.

        git flow release publish RELEASE

For example,

        git flow release publish 1.2.3

Now, your release branch was published. You can inform the team of this new release branch for final hardening work.

### Finish the release

Finally, let's finish the release. This step includes making a tag, merging changes to master and develop branches
and removing the release branch.

        git flow release finish RELEASE

For example,

        git flow release finish 1.2.3

This finishing step removes the ```release/1.2.3``` branch *locally*, and moves to ```develop``` branch.

### Push ```master``` branch and bump up version in ```develop``` branch and push it.

In the finishing step above, every change was already merged to ```master``` and ```develop``` branch from the
release branch.
So, don't forge to push local commits to the remote.

For example, let's push the commits in ```master``` branch to the remote.

        git checkout master
        git push origin master

Before pushing the commits in ```develop``` branch, let's bump up the version for next development cycle with ```-SNAPSHOT```
in ```develop``` branch.

For example,

        git checkout develop
        mvn org.codehaus.mojo:versions-maven-plugin:2.1:set -DgenerateBackupPoms=false -DnewVersion="1.2.4-SNAPSHOT"

Again, don't forget bumping up versions in the demo folder if it exists.

        cd demo
        mvn org.codehaus.mojo:versions-maven-plugin:2.1:set -DgenerateBackupPoms=false -DnewVersion="1.2.4-SNAPSHOT"

        cd ..
        git commit -a -m "bump up version for next dev cycle."

        git push origin develop

### Push the newly created tag.

In the previous finishing step, a tag was made for the release version. e.g, ```1.2.3```

You can check all the tags by the following command:

        git tag -l

**Tip**: You can also run the following command to fetch all the tags pushed by someone else in the remote:

        git fetch --all --tags --prune

Now, let's push the newly created tag generated in the finishing step:

        git push origin 1.2.3

If you've published the release branch in the previous optional step, you may now remove the remote (temporary) release branch:

        git push origin :release/1.2.3

Now, your normal release process is done. Your new release was made as a tag and published.

### (Optional) Deploy the release tag to Hippo Forge Maven Repository

Check out the release tag like the following example. You may name the local branch to anything else.

        git checkout tags/1.2.3 -b tag-1.2.3

Now, deploy it to Hippo Forge Maven repository from the tag branch:

        mvn deploy

You may remove the local tag branch.

        git checkout develop
        git branch -D tag-1.2.3

Congratulations! Great work!
