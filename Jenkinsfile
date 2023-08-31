node {
    def mvnImage = 'maven:3.9.0'
    def mvnArgs = '-v /root/.m2:/root/.m2'

    properties([
        pipelineTriggers([
            pollSCM('*/2 * * * *')
        ])
    ])

    stage('Build') {
        checkout scm
        docker.image(mvnImage).inside(mvnArgs) {
            sh 'mvn -B -DskipTests clean package'
        }
    }

    stage('Test') {
        checkout scm
        try {
            docker.image(mvnImage).inside(mvnArgs) {
                sh 'mvn test'
            }
        } finally {
            junit 'target/surefire-reports/*.xml'
        }
    }

    input message: 'Lanjutkan ke tahap Deploy? (Click "Proceed" to continue)'

    stage('Deploy') {
        checkout scm
        docker.image(mvnImage).inside(mvnArgs) {
            sh './jenkins/scripts/deliver.sh'
            sleep(60)
        }
    }
}
