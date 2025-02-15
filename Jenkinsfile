pipeline {
    agent {}     // Agent block defines the machine or environment where the pipeline stages will run.

    environment {
        BUILD_VERSION = '1.0' // CSIM LIBRARY used to define environment variables that can be used within pipeline.
    }

    options {                     //you can configure additional options in Jenkinsfile
        timestamps() // Add timestamps to build console output
        buildDiscarder(logRotator(numToKeepStr: '10', daysToKeepStr: '30')) // Discard old builds
    }
    parameters {                 //it will ask for parameter when building jobs for customizations. can add default para for auto.
        string(name: 'ENVIRONMENT', description: 'Environment to deploy to', defaultValue: 'production')
        choice(name: 'DATABASE', choices: ['MySQL', 'PostgreSQL', 'Oracle'], description: 'Select the database type')
        booleanParam(name: 'CLEAN_BUILD', defaultValue: true, description: 'Clean build workspace')


    stages {                          //stages are steps that will be executed in pipeline.
        stage('Checkout') {
            steps {
                checkout scm // Checkout stage will set the environment
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package' // exact thing you want to do like executing shell script, python code, build docker product or (deploy it to k8 architecture)
            }
        }

        stage('Test') {
            when {
                expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') } // Conditional stage
            }
            steps {
                sh 'mvn test' // unit tests. regression tests, static code checks etc
            }
        }

        stage('Deploy') {
            when {
                expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
            }
            steps {
                input 'Do you want to deploy?' // Manual input step
                sh './deploy.sh' // Deploy your application
            }
        }
    }

    post {                       //post build actions such as aborted, unstable etc.
        always {
            echo 'This will always run at the end'
        }
        success {
            archiveArtifacts artifacts: 'target/*.jar', allowEmptyArchive: true // Archive build artifacts
            junit '**/target/test-reports/*.xml' // Publish JUnit test results
        }
        failure {
            mail to: 'admin@example.com', subject: 'Build failed!', body: 'The build has failed.' // Send email on failure
        }
    }
}


Theory:
Pre-defined variables:
BUILD_NUMBER: The current build number, which increments with each build.

BUILD_ID: The timestamp when the build was started, in the format "YYYY-MM-DD_hh-mm-ss."

BUILD_TAG: A string representing the unique tag for the current build.

BUILD_URL: The URL of the current build in Jenkins.

EXECUTOR_NUMBER: The unique number identifying the executor (agent) where the current build is running.

JOB_NAME: The name of the job that is currently being built.

BUILD_DISPLAY_NAME: A human-readable name for the current build.

WORKSPACE: The absolute path to the workspace where the build is being executed.

NODE_NAME: The name of the agent (node) where the build is running.

JOB_URL: The URL of the job in Jenkins.

JENKINS_HOME: The absolute path to the Jenkins master's home directory.


#Use "manage jenkins" for plugins and tool or any other things you want to configure.
