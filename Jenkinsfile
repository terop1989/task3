pipeline{
    agent any
    
    environment{
                INT_KEY = credentials('PAGERDUTY_JENKINS_INTEGRATION_KEY')
                }
    
    stages{
    
        stage('Build Image'){
            steps{
                sh '''
                    cmd=$(shuf -e case1 case2 -n 1)
                    
                    case $cmd in
                        case1) echo 'Building Image' ;;
                        case2) docker build . -t image01 ;;
                    esac
                    
                    '''
                }
        }
        
        stage('Test'){
            steps{
                sh '''
                    cmd=$(shuf -e case1 case2 -n 1)
                    
                    case $cmd in
                        case1) echo 'Testing' ;;
                        case2) test.sh ;;
                    esac
                    
                    '''
                }
        }
        
        stage('Push Image'){
            steps{
                sh '''
                    cmd=$(shuf -e case1 case2 -n 1)
                    
                    case $cmd in
                        case1) echo 'Push Image' ;;
                        case2) docker push image01 ;;
                    esac
                    
                    '''
                }
        }
        
        stage('Deploy App'){
            steps{
                sh '''
                    cmd=$(shuf -e case1 case2 -n 1)
                    
                    case $cmd in
                        case1) echo 'Deploy Application on Docker' ;;
                        case2) docker run -d image01 ;;
                    esac
                    
                    '''
                }
        }
    }
    
    post{
        
        success{
            echo 'Success!'
        }
        
        failure{
    
            sh """
               
               curl -X POST -H "content-type: application/json" \
                -d '{"routing_key":"${env.INT_KEY}","event_action":"trigger", \
                "payload":{"summary":"${env.JOB_NAME} job failed ${env.BUILD_URL}",\
                "source":"Jenkins", \
                "severity":"critical", \
                "component":"exploratory-stats", \
                "group":"prod-datapipe","class":"deploy"}}' \
                https://events.pagerduty.com/v2/enqueue
               
                """
        }
    }
}
