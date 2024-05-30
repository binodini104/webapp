pipeline {
    tools {
        jdk 'JAVA_HOME'
        maven 'M2_HOME'
    }
    agent any
    
    stages {
        stage("checkout") {
            steps {
                git 'https://github.com/nishankainfo/webapp.git'
            }
        }
        stage("compile") {
            steps {
                sh 'mvn compile'
            }
        }
        stage("test") {
            steps {
                sh 'mvn test'
            }
        }
        stage("package") {
            steps {
                sh 'mvn clean package'
                sh "mv target/*.war target/myweb.war"
            }
        }
        stage("deploy") {
            steps {
                sshagent(['deploy']) {
    sh """
                 
            scp -o StrictHostKeyChecking=no target/myweb.war ec2-user@13.234.112.253:/home/ec2-user/tomcat8/webapps/

              ssh ec2-user@13.234.112.253 /home/ec2-user/tomcat8/bin/shutdown.sh
               ssh ec2-user@13.234.112.253 /home/ec2-user/tomcat8/bin/startup.sh
            
          
          """

}
            }
        }       
        stage("backup") {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'idream-it-solutions', classifier: '', file: 'target/myweb.war', type: 'war']], credentialsId: 'nexus', groupId: 'com.idream.webapp', nexusUrl:' 13.201.11.86:8080/nexus%20(1) ', nexusVersion: 'nexus2', protocol: 'http', repository: 'repoR', version: '1.1'
            }
        }
    }
}

