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

### ScreenShots


![Charts img](https://github.com/marzo23/Automation/blob/master/Screenshots/1.PNG)
![Charts img](https://github.com/marzo23/Automation/blob/master/Screenshots/2.PNG)
![Charts img](https://github.com/marzo23/Automation/blob/master/Screenshots/3.PNG)



## Protractor UI tests


I had some issues to run npm package commands directly from a pipeline. I took the follow workaround:
-Instead of create a pipeline, I created a "free style project"
-I configured the execution (under "enviromen execution" tab) as "Execute windows command". These are the commands that I set:

cd C:\Users\L440\AppData\Roaming\npm\node_modules\protractor\bin
node protractor "C:\qa-automation-reporter\confs\conf.js"



### ScreenShots


![Charts img](https://github.com/marzo23/Automation/blob/master/Screenshots/4.PNG)
![Charts img](https://github.com/marzo23/Automation/blob/master/Screenshots/5.PNG)
![Charts img](https://github.com/marzo23/Automation/blob/master/Screenshots/6.PNG)
![Charts img](https://github.com/marzo23/Automation/blob/master/Screenshots/7.PNG)
![Charts img](https://github.com/marzo23/Automation/blob/master/Screenshots/8.PNG)
![Charts img](https://github.com/marzo23/Automation/blob/master/Screenshots/9.PNG)

I am attaching too the report generated by jazmin in the follow path: https://github.com/marzo23/Automation/tree/master/Protractor%20Reports



## Newman API Tests


I had the same issue than above to execute newman as a Pipeline. Instead of that, The commands that I used to run my tests are:

cd C:\Users\L440\AppData\Roaming\npm\node_modules\newman\bin
node newman run "C:\Postman\WizelineAcademy.postman_collection.json" -e "C:\Postman\TestEnv.postman_environment.json" -g "C:\Postman\My Workspace.postman_globals.json"

You can find the JSON of my collections and variables at the follow path:
https://github.com/marzo23/Automation/tree/master/Postman

And, an example of the complete console log (from Jenkins) of one execution:
https://github.com/marzo23/Automation/blob/master/PostmanConsoleOutput.txt



### ScreenShots


![Charts img](https://github.com/marzo23/Automation/blob/master/Screenshots/10.PNG)
