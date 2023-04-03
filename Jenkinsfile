pipeline {
  agent{
        label "worker1"
    }
  stages {
     stage("ssm-jenkins"){
                steps{
                withAWS(credentials: 'aws_creds', region: 'us-east-1'){
                    // install xmlstarlet
                    sh 'aws ssm send-command --instance-ids "i-08cdea18050c1d20d" --document-name "AWS-RunShellScript" --parameters commands="sudo apt update && sudo apt install xmlstarlet -y"'
                    // change config.xml configuration(disable security)
                    sh 'aws ssm send-command --instance-ids "i-08cdea18050c1d20d" --document-name "AWS-RunShellScript" --parameters commands="sudo xmlstarlet edit -L -u "//useSecurity" -v "false" /var/lib/jenkins/config.xml 2>/dev/null"'
                    // restart jenkins service
                    sh 'aws ssm send-command --instance-ids "i-08cdea18050c1d20d" --document-name "AWS-RunShellScript" --parameters commands="sudo systemctl restart jenkins"'
                    // change config.xml configurtaion(enable security with jenkins database)
                    sh 'aws ssm send-command --instance-ids "i-08cdea18050c1d20d" --document-name "AWS-RunShellScript" --parameters commands="sudo xmlstarlet edit -L -u "//useSecurity" -v "true" -u "//authorizationStrategy/@class" -v "hudson.security.FullControlOnceLoggedInAuthorizationStrategy" -s //authorizationStrategy -t elem -n denyAnonymousReadAccess -v "false" -u "//securityRealm/@class" -v "hudson.security.HudsonPrivateSecurityRealm" -s //securityRealm -t elem -n disableSignup -v "false"  -s //securityRealm -t elem -n enableCaptcha -v "false" 2>/dev/null /var/lib/jenkins/config.xml"'
                    // restart jenkins service
                    sh 'aws ssm send-command --instance-ids "i-08cdea18050c1d20d" --document-name "AWS-RunShellScript" --parameters commands="sudo systemctl restart jenkins"'
                }
                
            post{
                always{
                    echo "be patient"
                }
                success{
                    echo "successfully executed"
                }
                failure{
                    echo "execution failed"
                }
                
            }
        }
  }
}
}