pipeline {
    options{
        timeout(time:1,unit:'HOURS')
    }
    environment {
        docker_image_name = "django-test"
    }
    
    agent {
        dockerfile {
            filename 'Dockerfile'
            dir 'centos7/'
            label env.docker_image_name
        }
    }
       stages {
        stage('UnitTest & StaticAnalysis') {
            steps {
                script {
                    dir('Chapter01/mysite') {
                        sh 'pwd'
                        sh 'ls -lrt /'
                        sh 'pip3.6 --version'
                        sh 'python3.6 manage.py jenkins'
                    }
                }
            }
        }
        }
}
