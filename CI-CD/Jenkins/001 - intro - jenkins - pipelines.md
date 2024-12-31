# Pipeline

Here, by providing `options {}` we can access some specific utility functions of Jenkins.

```groovy
pipeline {
	agent any
	options { skipDefaultCheckout()}

	stages {
		stage('Checkout') {
			steps {
				git url: '<url of the github repo>', branch: '<required branch>'
				sh '<any shell command>'
			}
		}
	}
}
```

By default Jenkins `pulls` the source code from github or github, etc...
So no need to mention a `checkout` stage.

## Credentials in Jenkins

## Nested and Parallel Stages

### Nested Stages

We can have `nested` stages i.e multiple stages within a stage in Jenkins.

```groovy
pipeline {
	agent any
	options { skipDefaultCheckout()}

	stages {
		stage('Checkout') {
			steps {
				git url: '<url of the github repo>', branch: '<required branch>'
				sh '<any shell command>'
			}
		}

		stage('Lint and Test') {
			stages {
				stage('Lint') {
					steps {
						sh 'echo Linting using py-lint'
					}
				}
				stage('Test') {
					steps {
						sh 'Units tests using pytest'
					}
				}
			}
		}
	}
}
```

### Parallel Stages

By using `parallel` inside a stage, Jenkins can execute all of the mentioned stages in parallel.

```groovy
pipeline {
	agent any
	options { skipDefaultCheckout()}

	stages {
		stage('Checkout') {
			steps {
				git url: '<url of the github repo>', branch: '<required branch>'
				sh '<any shell command>'
			}
		}

		stage('Lint and Test') {
			parallel {
				stage('Lint') {
					steps {
						sh 'echo Linting using py-lint'
					}
				}
				stage('Test') {
					steps {
						sh 'Units tests using pytest'
					}
				}
			}
		}
	}
}
```

## Pipeline Options

```groovy
pipeline {
	agent any
	options {
		timeout(time: 1, units: 'HOURS')
		skipDefaultCheckout()
		retry(3)
	}

	stages {
		stage('Checkout') {
			steps {
				git url: '<url of the github repo>', branch: '<required branch>'
				sh '<any shell command>'
			}
		}

		stage('Lint and Test') {
			parallel {
				stage('Lint') {
					steps {
						sh 'echo Linting using py-lint'
					}
				}
				stage('Test') {
					steps {
						sh 'Units tests using pytest'
					}
				}
			}
		}
	}
}
```

### timeout()

-> How long the job will be built before it timeout. Here we need to take a value along with a unit. For example, we have taken `1 Hour` as our timeout limit.

For example, if we have set `1 minute` so, Jenkins will wait for 1 minute and if a build takes more than a minute, so it gets automatically cancelled.

### skipDefaultCheckout()

-> Skips the automatic checkout of code

## Pipeline Parameters

```groovy
pipeline {
	agent any
	parameters {
		string(name: 'ENVIRONMENT', defaultValue: 'dev', description: 'Specify the environment for deployment')
		booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Run Tests in pipeline')
	}
	options {
		timeout(time: 1, units: 'HOURS')
		retry(3)
	}

	stages {
		stage('Test') {
			when {
				expression {
					params.RUN_TESTS == true
				}
			}
			steps {
				sh 'echo Linting using py-lint'
			}
		}

		stage('Deploy') {
			steps {
				sh 'Deploying to ${params.ENVIRONMENT} environment'
			}
		}
	}
}
```

In the Jenkins GUI, we have options for build with parameters, we have the fields for the parameters specified in the Jenkinsfile.

### Types of parameters

```groovy
pipeline {
	agent any
	parameters {
		string(name: 'ENVIRONMENT', defaultValue: 'dev', description: 'Specify the environment for deployment')
		booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Run Tests in pipeline')
		text(name: '', defaultValue: '', description: '')
		choice(name: '', defaultValue: '', description: '')
		password(name: '', defaultValue: '', description: '')
	}
}
```

Here, in the pipeline you can see we have about five types of parameters.

## Input

-> Provides manual intervention(kind of `approval` or allows us to `pause`) for certain stages(or steps). For example, we can have this `input` before, say, deploying to prod, as it should require approval from the Team Lead or so.

```groovy
pipeline {
	agent any
	parameters {
		string(name: 'ENVIRONMENT', defaultValue: 'dev', description: 'Specify the environment for deployment')
		booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Run Tests in pipeline')
	}
	options {
		timeout(time: 1, units: 'HOURS')
		retry(3)
	}

	stages {
		stage('Test') {
			when {
				expression {
					params.RUN_TESTS == true
				}
			}
			steps {
				sh 'echo Linting using py-lint'
			}
		}

		stage('Deploy') {
			steps {
				sh 'Deploying to ${params.ENVIRONMENT} environment'
			}
			input {
				message "Do you want to proceed with deployment?"
				ok "Yes, proceed!"
			}
		}
	}
}
```
