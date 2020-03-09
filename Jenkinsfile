def SENGINE = ["carshopex", "flightshopex", "hotelshopex", "carshopze", "restaurantshopot", "restaurantshopmgr", "shopmgr", "shopnorm", "activityshopvi"]
def BENGINE = ["activitybookvi", "bookmgr", "carbookze", "mbrdbsvc", "pointsearnschedjob", "pointsmgr", "profilemgr", "restaurantbookot"]

pipeline{
    agent{
        label 'docker-slave'
    }
    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        timeout(time: 1, unit: 'HOURS')      
    }
    parameters {
        string(name: 'ansible_git_ref', defaultValue: 'master', description: '')
        string(name: 'application_env', defaultValue: 'qa', description: '')
        string(name: 'TAG', defaultvalue:'', description: '')
        string(name: 'application_git_ref', defaultValue: '$TAG', description: '')
        string(name: 'dependencies_git_ref', defaultValue: '$application_git_ref', description: '')
        string(name: 'dockerfile', defaultValue: 'Dockerfile', description: '')
        string(name: 'querulous_host', defaultValue: 'querulous-qa.iseatz.com', description: '')
        string(name: 'region', defaultValue: 'us-east-1', description: '')
        string(name: 'configmap', defaultValue: 'true', description: '')
        string(name: 'application', defaultValue: '', description: '')
        string(name: 'docker_identifier', defaultValue: '', description: '')
    }

    stages{
        stage('Prepare Application Stack to deploy'){
            steps {
                script {
                    switch(application) {
                        case "SENGINE":
                            def sengineStages = [:]
                            SENGINE.each {
                                sengineStages["$it"] = {
                                    node {
                                        stage("Build sengine app ${it}") {
                                            echo "Building app ${it}"
                                        }
                                    }
                                }
                            }                           
                            parallel sengineStages 

                        case "BENGINE":
                            BENGINE.each {
                                println "Item: $it"
                            }                            
                            break
                    }
                }
            }
        }
        
