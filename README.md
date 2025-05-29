# Compress Artifacts

Keeps build artifacts compressed to save disk space on the controller.

Adds an option to compress build artifacts (currently in a ZIP file only) when stored on the controller.
To use, you must request this option in the Jenkins global configuration screen (*Artifact Management for Builds* section).

Artifacts produced before the plugin was installed/configured will not be compressed, though they will be served correctly.

### Compatibility issues

Some other plugins might not yet support what they see as nonstandard artifact storage (requiring use of Jenkins ArtifactManager API introduced in Jenkins 1.532).

For an illustrative example, Copy Artifact was broken for several years on Jenkins controllers where this plugin was used, due to legacy use of direct file system access: [JENKINS-22637](https://issues.jenkins-ci.org/browse/JENKINS-22637) (fixed since 2018).
