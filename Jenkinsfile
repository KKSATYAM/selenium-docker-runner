pipeline{

agent any

parameters {
  choice choices: ['chrome', 'firefox'], description: 'Select Browser', name: 'BROWSER'
}


stages{

stage('Start Grid'){

steps{

sh "docker-compose -f grid.yaml up --scale ${params.BROWSER}=1 -d"

}

}

stage('Run Test'){

steps{

sh "docker-compose -f test-suite.yaml up"

script{

	if(fileExists('/output/flight-reservation/testng-failed.xml') || fileExists('/output/vendor-portal/testng-failed.xml')){
                        error('failed tests found')

}


}

}

}




}

post{

always{

sh "docker-compose -f test-suite.yaml down --remove-orphans"
sh "docker-compose -f grid.yaml down --remove-orphans"
sh "docker image rm keshrsa/selenium-docker-via-git"
sh "pwd"
archiveArtifacts artifacts: 'output/flight-reservation/emailable-report.html', followSymlinks: false
archiveArtifacts artifacts: 'output/vendor-portal/emailable-report.html', followSymlinks: false


}


}






}
