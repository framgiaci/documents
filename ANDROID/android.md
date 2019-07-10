## Bắt đầu

Để bắt đầu sử dụng SunCI, bổ sung file `framgia-ci.yml` ở thư mục gốc của dự án.
```yaml
#framgia-ci.yml

project_type: android

build:
  configurations:
    image: framgiaciteam/android:latest
    prepare:
      - fastlane android Build
      - framgia-ci run --logs
test:
  run_unit_test:
    ignore: false
    command: ./gradlew -PciBuild=true :app:testDebugUnitTest --no-daemon --max-workers 2
  check_style:
    ignore: false
    command: fastlane android checkstyle
  check_pmd:
    ignore: false
    command: fastlane android check_pmd
  check_lint:
    ignore: false
    command: fastlane android check_lint
  test_build:
    ignore: false
    command: fastlane android BuildFastlane
```

Cấu hình các command cho fastlane.
```java
/*
 * for ci
 */
apply plugin: 'checkstyle'
apply plugin: 'findbugs'
apply plugin: 'pmd'

check.dependsOn 'checkstyle', 'findbugs', 'pmd', 'cpd'

dependencies {
    checkstyle 'com.puppycrawl.tools:checkstyle:7.6'
}

task checkstyle(type: Checkstyle) {
    def conf
    if (project.hasProperty('checkStyleConfigFile')) {
        conf = file(checkStyleConfigFile)
    } else {
        conf = file("${project.rootDir}/config/checkstyle/android-style.xml")
    }
    configFile conf
    configProperties.checkstyleSuppressionsPath = file("${project.rootDir}/config/checkstyle/suppressions.xml")
    source 'src'
    include '**/*.java'
    exclude '**/gen/**'
    exclude '**/R.java'
    exclude '**/BuildConfig.java'
    reports {
        xml.enabled = true
        html.enabled = true
        xml {
            destination "${project.rootDir}/.framgia-ci-reports/checkstyle/checkstyle.xml"
        }
        html {
            destination "${project.rootDir}/.framgia-ci-reports/checkstyle/checkstyle.html"
        }
    }
    classpath = files()
    ignoreFailures = true
}

task pmd(type: Pmd) {
    ignoreFailures = true

    def conf
    if (project.hasProperty('pmdRuleSetFiles')) {
        conf = file(pmdRuleSetFiles)
    } else {
        conf = files("${project.rootDir}/config/pmd/ruleset.xml")
    }
    ruleSetFiles = conf
    ruleSets = []

    source 'src'
    include '**/*.java'
    exclude '**/gen/**'

    reports {
        xml.enabled = true
        html.enabled = true
        xml {
            destination "${project.rootDir}/.framgia-ci-reports/pmd/pmd.xml"
        }
        html {
            destination "${project.rootDir}/.framgia-ci-reports/pmd/pmd.html"
        }
    }
}

task cpd doLast {
    File outDir = new File("$project.buildDir/reports/pmd/")
    outDir.mkdirs()
    ant.taskdef(name: 'cpd',
            classname: 'net.sourceforge.pmd.cpd.CPDTask',
            classpath: configurations.pmd.asPath)
    ant.cpd(minimumTokenCount: '100',
            format: 'xml',
            outputFile: new File(outDir , 'cpd.xml')) {
        fileset(dir: "src/main/java") {
            include(name: '**/*.java')
        }
    }

}
task copyMd( type:Copy ) {
    from "${project.buildDir}/reports/test"
    include '**/*.html'
    into "${project.rootDir}/.framgia-ci-reports/buidahnma/"
}

```
