#!/usr/bin/env groovy
def isAccountChanged = true
def isCustomerChanged = true
def isDiscoveryChanged = false
def isGatewayChanged = false
node {  
        stage('checkout') 
{         
checkout scm     
}
        stage('Check for CHANGELOG update') {
                    sshagent(['Credential Name']) {
                       // sh "git config --add remote.origin.fetch +refs/heads/master:refs/remotes/origin/master"
                        //sh "git fetch --no-tags"
                        AUTHOR_NAME = sh(returnStdout: true, script: "git show -s --format='%an' HEAD").split()
                        echo "The last commit was written by ${AUTHOR_NAME}."
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
            }
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

def notifySlack(String buildStatus = 'STARTED') {
            buildStatus = buildStatus ?: 'SUCCESS'
            def color
            def image_url
            if (buildStatus == 'STARTED') {
                color = '#D4DADF'
                 image_url = "https://upload.wikimedia.org/wikipedia/commons/b/b4/JPEG_example_JPG_RIP_100.jpg"
            } else if (buildStatus == 'SUCCESS') {
                color = '#00FF00'
                image_url = "https://upload.wikimedia.org/wikipedia/commons/b/b4/JPEG_example_JPG_RIP_100.jpg"
            } else if (buildStatus == 'UNSTABLE') {
                color = '#FFFE89'
                image_url = "https://upload.wikimedia.org/wikipedia/commons/b/b4/JPEG_example_JPG_RIP_100.jpg"
            } else {
                color = '#FF0000'
                image_url = "https://upload.wikimedia.org/wikipedia/commons/b/b4/JPEG_example_JPG_RIP_100.jpg"
            }
            def msg = "${buildStatus}: `${env.JOB_NAME}` #${env.BUILD_NUMBER}:\n${env.BUILD_URL} \n ${AUTHOR_NAME}"
            slackSend(color: color, icon_emoji: ":robot_face:", message: msg)
}
     try { 
         stage('Test') {
                  if (isAccountChanged == true) {

                    node{
                       dir('account-service'){         
                       micro()
                       }
                   }
                 }

                  if (isCustomerChanged == true) {
                    node{
                        dir('customer-service'){
                        micro()
                        }
                   }
                 }
         }
 
        notifySlack()
        // Existing build steps.
    } catch (e) {
        currentBuild.result = 'FAILURE'
        throw e
    } finally {
        notifySlack(currentBuild.result)
        node{
        stage('DelDir') {
        mail    body: "project build error is here: ${env.BUILD_URL}" ,
                from: 'saiida1.sghaier@gmail.com',
                subject: 'project build failed',
                to: 'saiida1.sghaier@gmail.com'
        echo "** deldir ***"
        deleteDir() 
        }

           }
    }

    

