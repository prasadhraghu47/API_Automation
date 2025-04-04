# BenefitsAPIAutomationFramework

* This framework is created in java with Cucumber and RestAssured.

* Framework will read configurations from config.yaml file present in the config folder of project directory

## Config Variables
        env: stage
        yamlFilesDirectory: <path of yaml files directory>
        testDataFilesDirectory: <path of test data files directory>

* Based on the env, the globalProps file will be defined. For example, if env is set as stage, then globalProps value will be read from stageGlobalProps.yaml file provided inside config folder and the values can be accessed via getGlobalPropsMap() reference

* Environment can be passed in the system property also. System.setProperty("env", "dev"); or can be passed from maven command -Denv=dev

* Similarly, the framework will provide you store and access values with various options

        config props - getConfigPropsMap()
        env global props - getGlobalPropsMap()
        test case props - getTCPropsMap()
        api props - getAPIPropsMap("apiServiceName")

* Framework provides support for Akeyless secrets. To download secrets during runtime, add secrets.yml file in the project directory with secrets list

  For Example:

      serverUrl: https://akeyless.gw.non-prod.glb.us.walmart.net:8300/v2

      secrets:
        stage:
          - sourcePath: /Non-Prod/secret/BenefitsQA/AVP/stage/globalProps
            destinationPath: /config/stageGlobalProps.yaml
          - sourcePath: /Non-Prod/secret/BenefitsQA/AVP/stage/secretsProps
            destinationPath: /secrets/stageSecretsProps.yaml
          - sourcePath: /Non-Prod/secret/BenefitsQA/DatabaseFiles/WMRMTO2
            destinationPath: /secrets/WMRMTO2.p12
            base64Decode: true
          - sourcePath: /Non-Prod/secret/BenefitsQA/DatabaseFiles/wmtca_truststore
            destinationPath: /secrets/wmtca_truststore.jks
            base64Decode: true
        dev:
          - sourcePath: /Non-Prod/secret/BenefitsQA/AVP/${env}/globalProps
            destinationPath: /config/${env}GlobalProps.yaml
          - sourcePath: /Non-Prod/secret/BenefitsQA/AVP/${env}/secretsProps
            destinationPath: /secrets/${env}SecretsProps.yaml
          - sourcePath: /Non-Prod/secret/BenefitsQA/DatabaseFiles/WMRMTO2
            destinationPath: /secrets/WMRMTO2.p12

* Any secrets stored in 'secrets/<env>SecretsProps.yaml' file can be accessed with  'getSecretsPropsMap()'

* Automation report will present inside target directory

## To run

```bash
  mvn clean test -Dcucumber.filter.tags="@yourTagName"
```

## To run in parallel

```bash
  1. Update the parallel.thread.count property in pom.xml or for looper update DEFAULT_THREAD_COUNT=<count> in .looper.yml file
  2. mvn clean test -Dcucumber.filter.tags="@yourTagName"
```
## To expose report in testburst

#### Note: As a first step, we need to onboard the project in 'https://testburst.walmart.com/headerIconContent?contentKey=21'

    1. Create testburst.properties file in the project directory
    2. Add env, appName, track and squad details of the project similar to below
            appName=<appName>
            env=<env>
    3. Execute mvn clean test -Dcucumber.filter.tags="<@yourTagName>" -Dcucumber.plugin="com.walmart.testburst.listener.cucumberListener"

## Looper configurations

Update the DEFAULT_ENV, DEFAULT_TAGS, DEFAULT_EMAIL_ID & DEFAULT_THREAD_COUNT with respective to the project in .looper.yml file

Refer below Example:-

    envs:
      global:
        variables:
          NODE_OR_MAVEN: MAVEN           # NODE or MAVEN
          DEFAULT_ENV: stage             # dev or stage or prod
          DEFAULT_TAGS: '@AVP'
          DEFAULT_EMAIL_ID: Pratheep.MK@walmart.com,firstname.lastname@walmart.com
          DEFAULT_THREAD_COUNT: 10       # FOR SEQUENTIAL RUN, KEEP THE VALUE AS 0
