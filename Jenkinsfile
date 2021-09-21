//CODE_CHANGES = getGitChanges()
def gv

pipeline {

    agent any
    //environment {
        //NEW_VERSION = '1.0.0'
        //SERVER_CREDENTIALS = credentials('//use reference or id of the credential created on jenkins')
    //}
    tools { //access build tools for the project
        //maven '//provide name installation in the jenkins server'
        gradle 'Gradle'
        //jdk
    }
    parameters{
        //type(name: '', defaultValue: '', description: '')
        string(name: 'VERSION', defaultValue: '', description: 'version to deploy on prod')
        choice(name: 'VERSION', choices: ['1.0.0', '1.1.0', '1.1.1'], description: 'version to deploy in prod')
        booleanParam(name: 'executeTests', defaultValue: true, description: '')
    }
    stages {

        stage("init") {
            
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }    

        stage("build") {
            
            when {
                expression {
                    BRANCH_NAME == 'dev' //&& CODE_CHANGES == 'true'
                }
            }

            steps {
                echo 'building the application ..'
                //echo "building the application version ${NEW_VERSION}"
                sh "mvn install" //possible only if the tool maven is defined in tools 

                script {
                    gv.buildApp()
                }
            }
        }

        stage("test") {

               when {
                expression {
                    BRANCH_NAME == 'dev'
                    params.executeTests
                }
               }

            steps {
                echo 'testing the application ...'

                script{
                    gv.testApp()
                }
            }
        }

        stage("deploy") {

            steps {
                echo 'deploying the application ...'
                //echo "deploying version ${params.VERSION}"
                //echo "deploying with ${SERVER_CREDENTIALS}" //double quote when wanted to use veariable value

                //we can also use the credentials directly in the stage
                //withCredentials([  //wrapper syntax, groovy object
                    //usernamePassword(credentials: '//reference or id of the credential created in jenkins', usernameVariable: USER, passwordVariable: PWD) //we store credentials user and password into USER and PWD
                //]) {
                    //sh "scp file.dot ${USER} ${PWD}"
                //}

                script {
                    gv.deployApp()
                }

            }
        }
    
    
        stage ("run frontend") {
            steps {
                echo 'executing yarn...'
                nodejs('NodeJS 10.17') {
                    sh 'yarn install'
                }
            }
        }
    
        //stage ("run backend") {
            //steps {
               // echo 'executing gradle...'
                //withGradle() {
                    //sh 'gradle wrapper --gradle-version  6.9.1'
                //sh './gradlew -v'
                //echo '${?}'
                //}
           // }
        //}
    }


    //post { //executed after stages
        //always {
            //always executed
       // }
        //faillure {
            //executed in case of faillure
       // }
       // success{
            //executed in case of success
        //}

   // }
}

//node {
    //groovy script (write the wohl config here)
//}
