properties([pipelineTriggers([githubPush()])])

node('linux'){   
    stage('Test'){
        git 'https://github.com/haissamk/java-project.git'
        sh 'ant -f test.xml -v'
        junit 'reports/resutls.xml'
    }
    
    stage('Build'){
        
        sh "ant -f build.xml -v"
    }
    
    stage('Deploy'){
        sh 'aws s3 cp /workspace/java-pipeline/dist/rectangle-${BUILD_NUMBER}.jar s3://elasticbeanstalk-us-east-1-671070857563'
            
    }
    
    stage('Reports'){
    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'AWS User for Jenkins', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
    // some block
        
        sh 'aws cloudformation describe-stack-resources --region us-east-1 --stack-name jenkins'
            
        }
    }
}

