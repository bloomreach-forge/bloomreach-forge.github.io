
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

<MTMarkdownOptions output='raw'>
<pre class="brush: plain">
git checkout support/1.x -b release/1.1.2 &#13;
mvn org.codehaus.mojo:versions-maven-plugin:2.1:set -DgenerateBackupPoms=false -DnewVersion="1.1.2" &#13;
cd demo &#13;
mvn org.codehaus.mojo:versions-maven-plugin:2.1:set -DgenerateBackupPoms=false -DnewVersion="1.1.2" &#13;
cd .. &#13;
git commit -a -m "bump up version for release." &#13;
</pre>
</MTMarkdownOptions>

### (Optional) Publish the release branch

You can skip this step if you are releasing it alone.
But if you want your team to do final hardening in the shared release branch, you should do this step.

<MTMarkdownOptions output='raw'>
<pre class="brush: plain">
git push -u origin release/1.1.2
</pre>
</MTMarkdownOptions>

### Tag and publish the Tag

<MTMarkdownOptions output='raw'>
<pre class="brush: plain">
git tag -a 1.1.2 -m "Tagging 1.1.2" &#13;
git tag -l &#13;
git push origin 1.1.2 &#13;
</pre>
</MTMarkdownOptions>

### Merge the release branch to the base branch

<MTMarkdownOptions output='raw'>
<pre class="brush: plain">
git checkout support/1.x &#13;
git merge release/1.1.2 &#13;
</pre>
</MTMarkdownOptions>

### Bump up the version in the base branch

<MTMarkdownOptions output='raw'>
<pre class="brush: plain">
git checkout support/1.x &#13;
mvn org.codehaus.mojo:versions-maven-plugin:2.1:set -DgenerateBackupPoms=false -DnewVersion="1.1.3-SNAPSHOT" &#13;
cd demo &#13;
mvn org.codehaus.mojo:versions-maven-plugin:2.1:set -DgenerateBackupPoms=false -DnewVersion="1.1.3-SNAPSHOT" &#13;
cd .. &#13;
git commit -a -m "bump up version for next dev cycle." &#13;
git push origin support/1.x &#13;
</pre>
</MTMarkdownOptions>

### Remove release branch

<MTMarkdownOptions output='raw'>
<pre class="brush: plain">
git branch -D release/1.1.2
</pre>
</MTMarkdownOptions>

If you've published the release branch in the previous optional step, you may now remove the remote (temporary) release branch:

<MTMarkdownOptions output='raw'>
<pre class="brush: plain">
git push origin :release/1.1.2
</pre>
</MTMarkdownOptions>

### (Optional) Deploy the release tag to Maven Repository

Check out the release tag like the following example. You may name the local branch to anything else.

<MTMarkdownOptions output='raw'>
<pre class="brush: plain">
git checkout tags/1.1.2 -b tag-1.1.2
</pre>
</MTMarkdownOptions>

Now, deploy it to Hippo Forge Maven repository from the tag branch:

<MTMarkdownOptions output='raw'>
<pre class="brush: plain">
mvn deploy
</pre>
</MTMarkdownOptions>

You may remove the local tag branch.

<MTMarkdownOptions output='raw'>
<pre class="brush: plain">
git checkout support/1.x &#13;
git branch -D tag-1.1.2 &#13;
</pre>
</MTMarkdownOptions>

Congratulations! Great work!
