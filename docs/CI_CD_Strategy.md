## CI-CD Strategy

#### Develop <--> Test -> Deploy

After a user story is created, Product team will groom (prior to the sprint start) the story with Engineering and Designers would have (ideally) created the necessary designs and assets (preferably more vector drawables and less PNGs) for developers to start development.

##### 1 Develop

- dev follows coding standards (along with styling/formatting)
- creates unit tests
- creates automation tests
- executes code quality checks
- tests the feature on couple of devices running different OS versions/screen sizes
- creates a pull/merge request

###### 1.1 Branching strategy

- create a branch from `develop` (baseline code) and work on the user story
- once dev completes the user story/defect, put up a pull request (PR) for your peers to review the code
	- leverage PR template so we all use the same format (user story number, title, test accounts, among others)
- developer quashes all the commits to one but if the PR receives feedback, they don't squash the commits. This helps the peers to identify the feedback that was incorporated
- once this PR is approved (and `PR Builder job` passes), merge the branch to `develop`
- once we are ready to cut a release, create a `release candidate (RC)` branch from `develop`
- after `release candidate job` executes successfully and build is created, QA validates this `RC` build
- If QA finds a bug, we cut a new `RC` and create a separate PR for `develop`
- publish `RC` build to app store(s)
- merge `release candidate` into `master`
- tag this release
- delete `release candidate` branch

###### 1.2 Jenkins Job(s)
The following jobs would be ideal for various stages of development:

###### 1.2.1 Pull-Request (PR) Builder job
- This job, executes the code coverage, static analysis checks and unit tests, kicks off soon as a PR is put up by the developer
- After the job passes and once the necessary approvals are received, developer merges the branch to `develop`

###### 1.2.2 Develop on merge/commit job
- This job executes every time code is merged into `develop` and when successful, publishes the artifacts (`ipa` and `apk`) to [HockeyApp](https://hockeyapp.net/)

###### 1.2.3 Release Candidate job
- After a `RC` branch is cut, this job kicks off and publishes the artifacts to `HockeyApp`
 
###### 1.2.4 UI Testing (nightly) job
- Every night, this job would execute to validate all the screens using an emulator (more than one emulator would be ideal)
- This nightly job creates a report that the developers/product managers will validate for any anomalies

###### 1.3 Tooling
- Define a `gradle` configuration where all the quality checks are specified and hence creates a task for each check to run
- Static code analysis:
	- [Lint](https://developer.android.com/studio/write/lint)
	- [PMD](http://pmd.sourceforge.net/pmd-4.3/rules/android.html)
	- [FindBugs](http://findbugs.sourceforge.net/)
	- [Checkstyle](http://checkstyle.sourceforge.net/)
- [JaCoCo](https://www.eclemma.org/jacoco/) code coverage (instructions, line, branch, complexity, methods, types etc) 
	- not to be used as a **code quality** metric
- [Danger](https://github.com/danger/danger) plug-in
	- gem that validates the PR
	- shows the quality checks' numbers as well
- Secure the source code and shrink resources
	- [ProGuard](https://developer.android.com/studio/build/shrink-code) - free and baked into Android
	- [DexGuard](https://www.guardsquare.com/en/dexguard) - requires license
	- [Certificate pinning](https://developer.android.com/training/articles/security-ssl) - pins the certificate used by the server in the mobile client in order to prevent MITM attack

##### 2 Test
- A mixture of manual and automated tests along with emulator and actual devices but UI tests are run nightly (ideally) unlike unit tests which need to execute/pass before a PR is put up by the developer

###### 2.1 Unit Test
There are couple of options

- [Robolectric](http://robolectric.org/) - tests run within the JVM and independent of Android SDK so, no need for an emulator/device to run these tests
- [Mockito](https://github.com/mockito/mockito) - a mocking framework to mock the Android SDK, unlike Robolectric

###### 2.2 UI (Automation) Testing
While there are plenty of options, we need to drill down further into a few of these to determine if they would work for us here at GoCanvas

- [Espresso](https://developer.android.com/training/testing/espresso/) - enables us to write Android UI tests by creating robots that perform actions on a view
- [Spoon](https://github.com/square/spoon) - enables us to write and execute Android UI tests on multiple devices and along with [Falcon](https://github.com/jraska/Falcon), takes screenshots of various view components (scrollview, dialogs, among others) and generates a HTML view of the results
- [Applitools](https://applitools.com/) - enables us to do visual assertions without writing a lot of code and provides a portal to view the results and approve/reject the images to create a baseline
- [Firebase Test Lab](https://firebase.google.com/docs/test-lab/)
- [Amazon Device Farm](https://aws.amazon.com/device-farm/)

##### 3 Deploy
- `fastlane` integration with [TestFlight](https://developer.apple.com/testflight/) and [Google Play](https://play.google.com/apps/publish/)

While most of the tools/frameworks are Android specific, some are iOS-friendly as well and we should be able to find an equivalent tool/framework that accomplishes the same goal.
