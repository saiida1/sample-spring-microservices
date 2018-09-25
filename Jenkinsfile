#!/usr/bin/env groovy
def isAccountChanged = true
def isCustomerChanged = true
def isDiscoveryChanged = false
def isGatewayChanged = false
def micro() {
            stage('checkout') {
                sh "pwd ;chmod +x script.sh"
                sh "pwd ; ./script.sh "                   
            }
            stage('scrip') {
                sh "pwd ;chmod +x scrip.sh"
                sh "pwd ; ./scrip.sh "                   
            }
           stage('checkout2') {
                sh "pwd ; chmod +x script.sh"
                sh "./script.sh "                   
            }
            stage('scrip2') {
                sh "pwd ; chmod +x scrip.sh"
                sh "pwd ; ./scrip.sh "                    
            }     
}  
node ('slave01'){  
        stage('checkout') 
        {         
                checkout scm     
        }
        stage('Check for CHANGELOG update') {
                    sshagent(['Credential Name']) {
                       // sh "git config --add remote.origin.fetch +refs/heads/master:refs/remotes/origin/master"
                        //sh "git fetch --no-tags"
                        List<String> sourceChanged = sh(returnStdout: true, script: "git diff --name-only HEAD^ origin/${env.BRANCH_NAME}").split()
                        for (int i = 0; i < sourceChanged.size(); i++) {
                            echo "** Here ***"
                            if (sourceChanged[i].contains("account")) {
                                isAccountChanged  = true
                            }
                            if (sourceChanged[i].contains("customer")) {
                                isCustomerChanged = true
                            }
                            if (sourceChanged[i].contains("discovery")) {
                                isDiscoveryChanged = true
                            }
                            if (sourceChanged[i].contains("gateway")) {
                                isGatewayChanged = true
                            }
                        }
                    }
            }
     
        stage('Test') {
                 if (isAccountChanged == true) {
                        dir('account-service'){         
                                micro()
                        }
                 }
                if (isCustomerChanged == true) {
                        dir('customer-service'){
                                 micro()              
                        }
      
                }           
         }
}


