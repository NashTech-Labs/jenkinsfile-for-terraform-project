pipeline {
	agent { label '<node_label>' }
	stages {
		stage('Pull Github project') {
			steps {
				checkout([$class: 'GitSCM', branches: [[name: '*/<branch_name>']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'CleanBeforeCheckout', deleteUntrackedNestedRepositories: true]], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '<credentials_id>', url: '<repo_link>']]])
			}
		}
		stage('Plan Cluster Infrastructure') {
			steps {
				sh '''
				terraform init -no-color
				terraform plan -out clusterplan -no-color
				'''
			}
		}
		stage('Approve Cluster Infrastructure') {
			steps {
				script {
					def userInput = input(id: 'confirm', message: 'Apply Terraform?', parameters: [ [$class: 'BooleanParameterDefinition', defaultValue: false, description: 'Apply terraform', name: 'confirm'] ])
				}
			}
		}
		stage('Apply Cluster Infrastructure') {
			steps {
				sh '''
				terraform apply -input=false clusterplan -no-color
				'''
			}
		}
}