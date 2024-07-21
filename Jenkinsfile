pipeline{

agent any
stages{
stage('Start the Grid'){
    steps{
   sh "docker-compose -f grid.yaml up -d"
    }
}
stage('Run tests'){
 steps{
   sh "docker-compose -f test-suites up"
    }
}

}
post 
{
always
{
sh "docker-compose -f grid.yaml down"
sh "docker-compose -f test-suites down"
}


}
}