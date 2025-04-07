## Overview

This project is an example of using Azure DevOps for a java/maven project. It demonstrates how to set up a CI/CD pipeline for a java/maven application.  
We have multiple CI/CD Pipeline examples, one for running the SonarScanner and sending the results to SonarQube Server and the other for sending the results to SonarQube Cloud.  

PLEASE READ OUR SONARQUBE DOCUMENTATION FOR WORKING WITH AZURE DEVOPS PIPELINES  
[Azure DevOps - SonarQube Server Integration](https://docs.sonarsource.com/sonarqube-server/latest/devops-platform-integration/azure-devops-integration/)  
[Azure DevOps Pipelines - SonarQube Cloud](https://docs.sonarsource.com/sonarqube-cloud/advanced-setup/ci-based-analysis/azure-pipelines/)

## Important Information in Pipelines
- Triggers are set to only execute on changes to main branch and a specific directory in the project, this can be modified with whatever you would want to specify.
- They have shallow fetch set to 0. this is required for SonarScanner to properly analyze your project.  
- For more information on how to limit your analysis scope and parameters available, please check **SonarScanner Analysis Scope** and **SonarScanner Analysis Parameters** in the Important Links section.
- Please remember that there are different tasks for SonarQube Server and SonarQube Cloud. Examples for both are provided.
    - SonarQube Cloud Example: SonarQube-Cloud.yml  
    - SonarQube Server Example: SonarQube-Server.yml 
- If your Java project is using a different version than the one executing the analysis. Please check [Project's specific JDK](https://docs.sonarsource.com/sonarqube-server/latest/analyzing-source-code/languages/java/#project-specific-jdk).   
- In the example we have the Sonar Maven plugin version specified in the `pom.xml` file under the following plugin setting.  
``` sh
        <plugin>
          <groupId>org.sonarsource.scanner.maven</groupId>
          <artifactId>sonar-maven-plugin</artifactId>
          <version>version-to-change</version>
        </plugin>
```

## Important Links
[SonarQube Server - SonarQubePrepare](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/sonar-qube-prepare-v7?view=azure-pipelines)   
[Azure DevOps - SonarQube Server Integration](https://docs.sonarsource.com/sonarqube-server/latest/devops-platform-integration/azure-devops-integration/)  
[Azure DevOps Pipelines - SonarQube Cloud](https://docs.sonarsource.com/sonarqube-cloud/advanced-setup/ci-based-analysis/azure-pipelines/)  
[Azure DevOps SonarQube Server Extension](https://docs.sonarsource.com/sonarqube-server/latest/analyzing-source-code/scanners/sonarqube-extension-for-azure-devops/)  
[Azure DevOps SonarQube Cloud Extension](https://docs.sonarsource.com/sonarqube-cloud/advanced-setup/ci-based-analysis/sonarcloud-extension-for-azure-devops/)  
[SonarScanner Analysis Scope](https://docs.sonarsource.com/sonarqube-server/latest/project-administration/analysis-scope/)  
[SonarScanner Analysis Parameters](https://docs.sonarsource.com/sonarqube-server/latest/analyzing-source-code/analysis-parameters/)  

## Example to fail the entire pipeline if Quality Gate fails
There may be situations or branches in which you will like to fail the pipeline if the SonarQube Quality Gate fails in order to stop any other steps in the pipeline.  
This can be done by adding `sonar.qualitygate.wait=true` to the `extraProperties` section in the `SonarQubePrepare/SonarCloudPrepare` task.  

Example
``` sh
    extraProperties: |
      # Additional properties that will be passed to the scanner, 
      sonar.verbose=true
      sonar.qualitygate.wait=true
```