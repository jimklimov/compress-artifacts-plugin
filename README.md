# Compress Artifacts

Keeps build artifacts compressed to save disk space on the controller.

Adds an option to compress build artifacts (currently in a ZIP file only) when stored on the controller.
To use, you must request this option in the Jenkins global configuration screen (*Artifact Management for Builds* section).

Artifacts produced before the plugin was installed/configured will not be compressed, though they will be served correctly.

NOTE: It may be possible to post-process existing artifacts and compress them manually, to conserve the Jenkins controller storage without losing build history of builds you do not want to remove by automatic clean-up (e.g. iterations of main or supported release branch evolution). One caveat is that the metadata of each processed build (in corresponding `build.xml`) must mention use of this plugin (and Jenkins reloaded or restarted after adding this information). For example:

* Compress existing data: `zip -r archive.zip archive/ && mv archive{,.x}` -- note that after renaming/removing the `archive` sub-directory, Jenkins immediately stops showing artifacts associated with that build
* Add a second-level XML tag in `build.xml` (directly inside the `<flow-build>` tag in case of pipeline builds) like this: `<artifactManager class="org.jenkinsci.plugins.compress_artifacts.CompressingArtifactManager" plugin="compress-artifacts@112.v52b_808b_85a_e8"/>` -- you can produce an up to date example by running a build which uses the `archiveArtifacts` step after you install this plugin and enable it in your Jenkins global configuration, and reviewing the `build.xml` file of that build.
* Rinse and repeat for all historic data
* Restart Jenkins (or for minimal disruption -- visit its `$JENKINS_URL/reload`, or click the "Reload Configuration from Disk" in the "Manage" page of Jenkins Web UI)
* Revise that artifacts in your processed builds are again visible, and can be viewed in Jenkins UI
* When everything works satisfactorily, you can remove all the `archive.x` directories made in the first step, e.g. `find ${JENKNS_HOME}/jobs -type d -name archive.x -exec rm -rf '{}' \;`

### Compatibility issues

Some other plugins do not yet support nonstandard artifact storage.
In particular, Copy Artifact will be broken. ([JENKINS-22637](https://issues.jenkins-ci.org/browse/JENKINS-22637))
