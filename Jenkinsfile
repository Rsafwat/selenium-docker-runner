pipeline{

agent any
stages{
stage('Run Test'){
    steps{
   sh "docker-compose up"
    }
}
stage('Bring down grid'){
steps{
sh "docker-compose down"
}

}

}