
## Support Release Process Using git

This article explains how you can cut a release from a ```support``` branch,
using ```git``` commands and deploy the release artifacts.

- Reference: [https://gitversion.readthedocs.io/en/latest/git-branching-strategies/gitflow-examples/#support-branches](https://gitversion.readthedocs.io/en/latest/git-branching-strategies/gitflow-examples/#support-branches)

This instructions were inspired by the branching guidelines in the following articles:

- http://danielkummer.github.io/git-flow-cheatsheet/
- https://gitversion.readthedocs.io/en/latest/git-branching-strategies/gitflow-examples/
- http://nvie.com/posts/a-successful-git-branching-model/

However, since the 'support' feature of ```git-flow``` features are still *experimental* and not really recommended yet, this instruction simply uses normal ```git``` commands only, not using a
specific ```git-flow``` feature, unlike [Standard Release Process Using git-flow](standard-release-process.html).

### Create a new release branch from the support branch and bump up versions

Suppose you have a support branch for an older major version, ```support/1.x```.
And, now you want to cut a release (e.g, v1.1.2) from it.

        git checkout support/1.x -b release/1.1.2
        mvn org.codehaus.mojo:versions-maven-plugin:2.1:set -DgenerateBackupPoms=false -DnewVersion="1.1.2"
        cd demo
        mvn org.codehaus.mojo:versions-maven-plugin:2.1:set -DgenerateBackupPoms=false -DnewVersion="1.1.2"
        cd ..
        git commit -a -m "<ISSUE_ID>: bump up version for release."

### (Optional) Publish the release branch

You can skip this step if you are releasing it alone.
But if you want your team to do final hardening in the shared release branch, you should do this step.

        git push -u origin release/1.1.2

### Tag and publish the Tag

        git tag -a 1.1.2 -m "<ISSUE_ID>: Tagging 1.1.2"
        git tag -l
        git push origin 1.1.2

### Merge the release branch to the base branch

        git checkout support/1.x
        git merge release/1.1.2

### Bump up the version in the base branch

        git checkout support/1.x
        mvn org.codehaus.mojo:versions-maven-plugin:2.1:set -DgenerateBackupPoms=false -DnewVersion="1.1.3-SNAPSHOT"
        cd demo
        mvn org.codehaus.mojo:versions-maven-plugin:2.1:set -DgenerateBackupPoms=false -DnewVersion="1.1.3-SNAPSHOT"
        cd ..
        git commit -a -m "<ISSUE_ID>: bump up version for next dev cycle."
        git push origin support/1.x

### Remove release branch

        git branch -D release/1.1.2

If you've published the release branch in the previous optional step, you may now remove the remote (temporary) release branch:

        git push origin :release/1.1.2

### (Optional) Deploy the release tag to Maven Repository

Check out the release tag like the following example. You may name the local branch to anything else.

        git checkout tags/1.1.2 -b tag-1.1.2

Now, deploy it to Hippo Forge Maven repository from the tag branch:

        mvn deploy

You may remove the local tag branch.

        git checkout support/1.x
        git branch -D tag-1.1.2

Congratulations! Great work!
