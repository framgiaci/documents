fastlane documentation
================
# Installation

Make sure you have the latest version of the Xcode command line tools installed:

```
xcode-select --install
```

Install _fastlane_ using
```
[sudo] gem install fastlane -NV
```
or alternatively using `brew cask install fastlane`

# Available Actions
## Android
### android Build
```
fastlane android Build
```
Build app  .
   Example:  fastlane android  buildApp buildType:develop ticketNumber:1234 
### android checkstyle
```
fastlane android checkstyle
```
lane check style code
### android check_pmd
```
fastlane android check_pmd
```
lane PMD code
### android check_lint
```
fastlane android check_lint
```
lane check Lint code
### android run_unitTest
```
fastlane android run_unitTest
```
lane run Unit Test
### android beta_distribute
```
fastlane android beta_distribute
```
Beta Distribution 
### android buildApp
```
fastlane android buildApp
```
Build app  .
   Example:  fastlane android  buildApp buildType:develop ticketNumber:1234 
### android uploadFabric
```
fastlane android uploadFabric
```
Upload to Fabric.
   Example:  fastlane android  uploadFabric 
### android notifyCW
```
fastlane android notifyCW
```
notification on chatwork
 Example : fastlane android  notifyCW 

----

This README.md is auto-generated and will be re-generated every time [fastlane](https://fastlane.tools) is run.
More information about fastlane can be found on [fastlane.tools](https://fastlane.tools).
The documentation of fastlane can be found on [docs.fastlane.tools](https://docs.fastlane.tools).
