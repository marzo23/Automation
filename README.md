# Continuous Integration: Jenkins

## JMeter Performance tests Pipeline
```
pipeline {
    agent any
    stages {
        stage('Performance tests') {
            steps {
                bat '"C:\\apache-jmeter-5.0\\bin\\jmeter.bat" -n -t C:\\qa-academy-performance\\api_tests.jmx -l results.jtl'
            }
        }
        stage('Generate dashboard') {
            steps {
                bat '"C:\\apache-jmeter-5.0\\bin\\jmeter.bat" -g results.jtl -o dashboard'
            }
        }
        stage('Publish') {
            steps {
                publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, keepAll: true, reportDir: 'dashboard', reportFiles: 'index.html', reportName: 'Dashboard', reportTitles: ''])
                perfReport percentiles: '0,50,90,100', sourceDataFiles: 'results.jtl'
            }
        }
    }
}
```

## ScreenShots

```
![alt Charts img](https://octodex.github.com/images/yaktocat.png)

```

## Generate dashboard
```
jmeter -g results.jtl -o dashboard
```

## Jenkins plugins

* HTML Publisher
* Performance Plugin

## Jenkins pipeline
```
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                cleanWs()
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/wizelineacademy/qa-academy-performance.git']]])
            }
        }
        stage('Performance tests') {
            steps {
                withEnv(['JMETER_HOME=/Users/john.connor/apache-jmeter-4.0/bin']) {
                    sh '$JMETER_HOME/jmeter -n -t api_tests.jmx -l results.jtl'
                }
            }
        }
        stage('Generate dashboard') {
            steps {
                withEnv(['JMETER_HOME=/Users/john.connor/apache-jmeter-4.0/bin']) {
                    sh '$JMETER_HOME/jmeter -g results.jtl -o dashboard'
                }
            }
        }
        stage('Publish') {
            steps {
                publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, keepAll: true, reportDir: 'dashboard', reportFiles: 'index.html', reportName: 'Dashboard', reportTitles: ''])
                perfReport percentiles: '0,50,90,100', sourceDataFiles: 'results.jtl'
            }
        }
    }
}
```

## Useful links

* [JMeter Plugins manager](https://jmeter-plugins.org/wiki/PluginsManager/)