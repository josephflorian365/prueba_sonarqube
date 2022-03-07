#!groovy
pipeline {
    agent any
     tools {
        jdk 'jdk'
        maven 'mavenTool'
       
    }
    stages {
        stage("Git Clone"){ 
            steps { 
                cleanWs()
                    checkout([$class: 'GitSCM', 
                              branches: [[name: '*/main']], 
                              doGenerateSubmoduleConfigurations: false, 
                              extensions: [[$class: 'CleanCheckout']], 
                              submoduleCfg: [], 
                              userRemoteConfigs: [
                                    [url: 'https://github.com/josephflorian365/prueba_sonarqube.git', credentialsId: 'jenkins_github']
                                    ]]) 
                           sh 'pwd'
                           sh'ls -l' 
            } //steps 
        } //stage
        
            stage('Build Project') {
                agent any 
                steps {
                        sh 'mvn clean install'
                }
            }
            stage('SonarQube Analysis1') {
                agent any 
                steps {
                    withSonarQubeEnv('SonarQubePruebas') {
                         // Tenga en cuenta que los parámetros en withSonarQubeEnv () deben ser los mismos que la configuración de Nombre en los servidores SonarQube antes
            withMaven(maven: 'mavenTool') {
                                 sh "mvn clean package sonar:sonar -Dsonar.host.url=http://localhost:9000 -Dsonar.login=a426fdc6c4a00b5bbfbdda01350e548776e42ad2"
            }
        }
                }
               }
        
            stage('SonarQube Analysis') {
                agent any 
                steps {
                    sh 'mvn clean package sonar:sonar -Dsonar.host.url=http://localhost:9000 -Dsonar.login=a426fdc6c4a00b5bbfbdda01350e548776e42ad2'
                }
               }
    }
}
