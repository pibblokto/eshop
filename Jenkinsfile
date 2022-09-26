pipeline {
    agent any
    stages {
        stage(“Build_Application”) {
            parallel {
                stage(“build_services”) {
                    agent any
                    steps {
                        sh '''
                            sudo chmod +x /usr/local/bin/docker-compose
                            sudo chmod 777 /var/run/docker.sock
                            cd src
                            sudo /usr/local/bin/docker-compose build identity-api basket-api catalog-api orderingapi paymentapi webhooks-api
                            sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                            docker push gcr.io/eshop-infrastructure/identity.api:1
                            docker push gcr.io/eshop-infrastructure/basket.api:1
                            docker push gcr.io/eshop-infrastructure/catalog.api:1
                            docker push gcr.io/eshop-infrastructure/ordering.api:1
                            docker push gcr.io/eshop-infrastructure/payment.api:1
                            docker push gcr.io/eshop-infrastructure/webhooks.api:1
                        '''
                        }
                }
                stage(“build_web1”) {
                    agent any
                    steps {
                        sh '''
                        sudo chmod +x /usr/local/bin/docker-compose
                        sudo chmod 777 /var/run/docker.sock
                        cd src
                        sudo /usr/local/bin/docker-compose build webhooks-client
                        sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                        docker push gcr.io/eshop-infrastructure/webhooks.client:1
                    '''
                    }

                }
                stage(“build_web2”) {
                    agent any
                    steps {
                        sh '''
                        sudo chmod +x /usr/local/bin/docker-compose
                        sudo chmod 777 /var/run/docker.sock
                        cd src
                        sudo /usr/local/bin/docker-compose build webmvc
                        sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                        docker push gcr.io/eshop-infrastructure/webmvc:1
                    '''
                    }

                }
                stage(“build_web3”) {
                    agent any
                    steps {
                        sh '''
                        sudo chmod +x /usr/local/bin/docker-compose
                        sudo chmod 777 /var/run/docker.sock
                        cd src
                        sudo /usr/local/bin/docker-compose build webspa
                        sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                        docker push gcr.io/eshop-infrastructure/webspa:1
                    '''
                    }

                }
                stage(“build_web4”) {
                    agent any
                    steps {
                        sh '''
                        sudo chmod +x /usr/local/bin/docker-compose
                        sudo chmod 777 /var/run/docker.sock
                        cd src
                        sudo /usr/local/bin/docker-compose build webstatus
                        sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                        docker push gcr.io/eshop-infrastructure/webstatus:1
                    '''
                    }

                }

                stage(“build_dockerhub”) {
                    agent any
                    steps {
                        sh ''' 
                            sudo chmod +x /usr/local/bin/docker-compose
                            sudo chmod 777 /var/run/docker.sock
                            cd src
                            sudo /usr/local/bin/docker-compose build seq nosqldata basketdata rabbitmq mobileshoppingapigw webshoppingapigw
                            sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                            docker push gcr.io/eshop-infrastructure/seq:1
                            docker push gcr.io/eshop-infrastructure/mongo:1
                            docker push gcr.io/eshop-infrastructure/redis:1
                            docker push gcr.io/eshop-infrastructure/rabbitmq:1
                            docker push gcr.io/eshop-infrastructure/mobileshoppingapigw:1
                            docker push gcr.io/eshop-infrastructure/webshoppingapigw:1
                        '''
                        }
                }
                stage(“build_apigw”) {
                    agent any
                    steps {
                           sh '''
                           sudo chmod +x /usr/local/bin/docker-compose
                           sudo chmod 777 /var/run/docker.sock
                           cd src
                           sudo /usr/local/bin/docker-compose build ordering-backgroundtasks mobileshoppingagg webshoppingagg ordering-signalrhub
                           echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                           docker push gcr.io/eshop-infrastructure/ordering.backgroundtasks:1
                           docker push gcr.io/eshop-infrastructure/mobileshoppingagg:1
                           docker push gcr.io/eshop-infrastructure/webshoppingagg:1
                           docker push gcr.io/eshop-infrastructure/ordering.signalrhub:1
                       '''
                       }
                }
            }
        }
        stage("Update_files") {
            steps {
                sh '''
                sudo yum install -y zip unzip
                cd docker_files
                sudo echo $GOOGLE_CLOUD_ACCOUNT > gcrkey.json
                cd ..
                sudo zip -r docker_files docker_files
                sudo aws s3 cp ./docker_files.zip s3://${BucketName}
                '''
                }
        }
        stage("Deploy") {
            steps {
                sh '''
                sudo aws ssm send-command --instance-ids $MasterNodeId --region "us-east-1" --document-name "DeployDockerStack"
                '''
            }
    }
    }
}