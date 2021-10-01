pipeline {
  
	agent {
    label "vm-developer"
	}

	environment {
    registry = "guizanelato/flask_review03"
		registryCredentials = 'docker_registry'

	}

	stages {
    stage('checkout repo') {
		  steps{
			  cleanWs()
				git "https://github.com/guizanelato/7813_review03.git"

			}

		}

    stage('build da app'){
		  steps{
			  script{
						imagem = docker.build(registry+"$BUILD_NUMBER")

				}
			}
		}
		stage('testes'){
      steps{
			  script{
				    imagem.inside("--name pyapp "){
              sh " cd app && python -m unittest tests/teste_rota.py"
					}		
				}
			}
		}
		
		stage('packaging'){
		  steps{
			  script{
				   docker.withRegistry("", registryCredentials){
             imagem.push()
				  }
				}
			}
		}
		
	}
	  post {
       cleanup{
        sh "docker image rmi $registry:$BUILD_NUMBER"
			}
		}
}
