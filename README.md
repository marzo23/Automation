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
![Charts](https://octodex.github.com/images/yaktocat.png)

```
