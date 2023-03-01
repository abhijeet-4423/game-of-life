pipeline {
    agent {
        label {
            label "built-in"
            customWorkspace "/mnt/war"
        }
    }
    stages {
        stage ("build-war"){
            steps {
                sh "rm -rf *"
                sh "git clone https://github.com/abhijeet-4423/game-of-life.git"
                dir ("game-of-life/"){
                    sh "mvn clean install"
                }
                sh "scp -i /mnt/test-1.pem game-of-life/gameoflife-web/target/gameoflife.war ec2-user@172.31.3.181:/mnt/gameoflife/"
            }
        }
        stage ("deploy on slave-1"){
            agent {
                label {
                    label "slave-1"
                    customWorkspace "/mnt/war"
                }
            }
            steps {
                sh "rm -rf docker"
                sh "git clone https://github.com/abhijeet-4423/docker.git"
                dir ("docker/"){
                    sh "docker build -t tomcat:1 ."
                }
                sh "docker run -itdp 8080:8080 -v /mnt/gameoflife:/usr/local/tomcat/webapps tomcat:1"
            }
        }
    }
}
