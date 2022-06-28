pipeline {
  agent any
  stages {

  stage('Stage 1- Deploy Backend API') {
      steps {
        script {
          echo 'Stage 1 - Deploy the Star Wars API using the RAW github URI containing SWAPI OAS3 specification document towards Axway API Manager via the API Manager REST API'
		
   		  def jsonString2 = httpRequest url: "https://apimanager.axwaydemo.co.uk/api/portal/v1.4/apirepo/importFromUrl" ,
										customHeaders:[[name:'Authorization' , value:"Basic YXBpYWRtaW46U3BhY2UqMTE4"],
										[name:'Content-Type'  , value:"application/x-www-form-urlencoded"]],
			  httpMode: "POST",
			  requestBody: "url=https://raw.githubusercontent.com/axwaydemouseruk/swapi/main/swapi_swagger.yaml&type=swagger&organizationId=eda42491-578a-4024-ae1d-c767f33a90fd&name=Star WarsU API"
			  
		  jsonObj2 = readJSON text: jsonString2.content

        }
      }
    }
  
  stage('Stage 2- Deploy Frontend API and virtualise SWAPI') {
      steps {
        script {
          echo 'Stage 2 - Deploy the Backend API into a Frontend API proxy'
		
   		  httpRequest url: "https://apimanager.axwaydemo.co.uk/api/portal/v1.4/proxies" ,
			  customHeaders:[[name:'Authorization' , value:"Basic YXBpYWRtaW46U3BhY2UqMTE4"],
					 [name:'Content-Type'  , value:"application/json"]],
			  httpMode: "POST",
			  requestBody: "{\"image\": \"/api/portal/v1.4/discovery/swagger/apis/1b8ede64-1b2c-49e2-8b64-c524e6d5bbf7/image\",\"organizationId\": \"eda42491-578a-4024-ae1d-c767f33a90fd\",\"apiId\": \"${jsonObj2.id}\",\"name\": \"Star Wars API Virtual Frontend\",\"version\": \"${jsonObj2.version}\",\"retired\": false,\"expired\": false,\"path\": \"/swapi/v${jsonObj2.version}\",\"securityProfiles\": [{\"name\": \"_default\",\"isDefault\": true,\"devices\": [{\"name\": \"Pass Through\",\"type\": \"passThrough\",\"order\": 1,\"properties\": {\"subjectIdFieldName\": \"Pass Through\",\"removeCredentialsOnSuccess\": \"true\"}}]}]}"
        }
      }
    }
  }
}
