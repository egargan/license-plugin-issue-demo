Simple project to demo an issue with [gradle-license-plugin](https://github.com/jaredsburrows/gradle-license-plugin).

Running `gradle build` in the root directory should display the problem. The project has two build variants, debug and release, each of which has a license task generated for it, licenseDebugTask and licenseReportTask. When running either buikd variant individually, the plugin works - it creates `licenseDebug/ReleaseReport.json` + the html file. When running both variants in turn, the 'poms' configuration created by the first conflicts with the second task, which attempts to create the same config. 

Here's the line that creates the 'poms' configuration' in the license task source:
```
  /**
   * Setup configurations to collect dependencies.
   */
  private def setupEnvironment() {
    // Create temporary configuration in order to store POM information
    project.configurations.create(POM_CONFIGURATION)
```
(https://github.com/jaredsburrows/gradle-license-plugin/blob/master/src/main/groovy/com/jaredsburrows/license/LicenseReportTask.groovy#L67)


If it is the case that the license task doesn't expect to be run multiple times in sequence, then the fix for us here is to just to limit its execution across the different build variants. If it is supposed to handle this, then it looks like something's going wrong somewhere
