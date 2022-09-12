# Jenkinsfile Template For Terraform Project

This pipeline supports Continuous Delivery. Hence, it waits for the user's approval before doing the final `terraform apply`.

```aidl
		stage('Approve Cluster Infrastructure') {
			steps {
				script {
					def userInput = input(id: 'confirm', message: 'Apply Terraform?', parameters: [ [$class: 'BooleanParameterDefinition', defaultValue: false, description: 'Apply terraform', name: 'confirm'] ])
				}
			}
		}
```

## Variables

1. `<node_label>` -> Node label (the label provided to the Jenkins node while node configuration).
2. `<branch_name>` -> Branch name from where you want to fetch the Terraform code (Make sure you have the `GitSCM` plugin configured in Jenkins).
3. `<credentials_id>` -> Git credentials ID (an unique ID that is generated when you create the credential in Jenkins).
4. `<repo_link>` -> Git repo link.

## Parameters

Additional parameters can be added to the pipeline by adding the following code. It creates a dropdown parameter. This parameter can be accessed in your Jenkinsfile using `$`.

For more parameters' syntax, [click here](https://www.jenkins.io/doc/book/pipeline/syntax/#parameters).

```aidl
	parameters {
		choice choices: ['dev', 'ts1', 'ts2', 'ts3', 'ts4', 'qa', 'qa1', 'qa2', 'qa3', 'prod'], description: 'Environment to run on', name: 'environment'
	}
```