#!/usr/bin/env groovy
import net.sf.json.JSONArray;
import net.sf.json.JSONObject;
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
def notifySlack() {
  JSONObject attachment = new JSONObject();
  attachment.put('text','I find your lack of faith disturbing!');
  attachment.put('fallback','Hey, Vader seems to be mad at you.');
  attachment.put('color','#ff0000');
  JSONArray attachments = new JSONArray();
  attachments.add(attachment);
  println attachments.toString()

  slackSend(color: '#00FF00', channel: channel, attachments: attachments.toString())

}

node {
    try {
        notifySlack()

        // Existing build steps.
    } catch (e) {
        currentBuild.result = 'FAILURE'
        throw e
    } /*finally {
        notifySlack()
    }*/
}
    

