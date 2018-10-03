pipeline {
    options{
        timeout(time:1,unit:'HOURS')
    }
    environment {
        docker_image_name = "django-test"
    }
    
    agent {
        dockerfile {
            filename 'centos7/Dockerfile'
            dir '.'
            label env.docker_image_name
        }
    }
       stages {
        stage('UnitTest & StaticAnalysis') {
            steps {
                script {
                    dir('Chapter01/mysite') {
                        sh 'set HTTP_PROXY=$HTTP_PROXY'
                        sh 'set HTTPS_PROXY=$HTTP_PROXY'
                        sh 'pwd'
                        sh 'ls -lrt'
                        sh 'pip3.6 --version'
                        sh 'sudo pip3.6 install --upgrade pip --proxy=$HTTP_PROXY'
                        sh 'sudo pip3.6 install -r requirements.txt --proxy=$HTTP_PROXY'
                        sh 'python3.6 manage.py test'
                    }
                }
            }
        }
        }
}
