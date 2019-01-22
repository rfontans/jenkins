node {
   
   stage('Environment') {
      env.COMPONENT_NAME = '${JOB_NAME}'
      env.JAVA_HOME = tool 'IIB JDK'
      env.MQSI_BASE_FILEPATH = "C:\\Program Files\\IBM\\IIB\\10.0.0.15"
      env.PATH = "$MQSI_BASE_FILEPATH;$MQSI_BASE_FILEPATH\\tools;$MQSI_BASE_FILEPATH\\server\\bin;$PATH";
   }
  
   def COMPONENT_BASE_DIR = "${WORKSPACE}\\${COMPONENT_NAME}"
   
   stage('Checkout') {
      cleanWs()
      git 'https://github.com/rfontans/iib-projects.git'   
   }

   def POM_ARTIFACT_ID
   def POM_VERSION

   stage('Build') {
      def mvnHome = tool 'M3'
      def POM_FILE = "${COMPONENT_BASE_DIR}\\pom.xml"
      def pom = readMavenPom file: POM_FILE
      POM_ARTIFACT_ID = pom.artifactId
      POM_VERSION = pom.version
      bat("${mvnHome}\\bin\\mvn -f ${POM_FILE} -Dmaven.test.failure.ignore clean package")
   }
   
   stage('Archive'){
       archiveArtifacts artifacts: "${COMPONENT_NAME}\\target\\iib\\*.bar", onlyIfSuccessful: true, fingerprint: true
   }

}