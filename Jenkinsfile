node {
    def dockerImage = 'maven:3.9.0'

    properties([
        pipelineTriggers([
            pollSCM('*/2 * * * *')
        ])
    ])

    stage('Build') {
        docker.image(dockerImage).inside('-v /root/.m2:/root/.m2') {
            sh 'mvn -B -DskipTests clean package'
        }
    }

    stage('Test') {
        docker.image(dockerImage).inside('-v /root/.m2:/root/.m2') {
            sh 'mvn test'
            junit 'target/surefire-reports/*.xml'
        }
    }
}
