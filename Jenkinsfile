pipeline {
    agent none
    parameters{
    	string defaultValue: 'debian', description: 'nombre del nodo del ambiente QA, ', name: 'LABEL_QA_NODE', trim: false
    	string defaultValue: '', description: 'nombre del nodo del ambiente DEV, ', name: 'LABEL_DEV_NODE', trim: false
    	string defaultValue: 'produccion', description: 'nombre del nodo del ambiente PRODUCCION, ', name: 'LABEL_PRODUCCION_NODE', trim: false
    }
    environment{
    	QA_NODE="${params.LABEL_QA_NODE}"
    	DEV_NODE="${params.LABEL_DEV_NODE}"
    	PRODUCCION_NODE="${params.LABEL_PRODUCCION_NODE}"
    }
    stages {
        stage('cloning') {
            agent{label PRODUCCION_NODE}
            steps {
                git branch: 'main', url: 'https://github.com/diegoperales7/laboratorio4.git'
                sh "echo Cloned!"
            }
        }
        stage('build docker') {
	    agent{label PRODUCCION_NODE}
            steps {
                sh "docker-compose build"
            }
        }
        stage('Pruebas QA') {
       	    agent{label QA_NODE}
            steps {
                sh "curl 192.168.100.158"
            }
        }
        stage('Deployment Produccion') {
       	    agent{label PRODUCCION_NODE}
            steps {
                sh "docker-compose up -d"
            }
        }
    }
}
