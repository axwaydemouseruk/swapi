pipeline {
  agent any
  stages {
  stage('Stage 1- Discover Swagger') {
      steps {
        script {
          echo 'Stage 1 - Discover the Swagger spec raw URL by querying the Swagger API'
	
	      def jsonString = httpRequest url: "https://raw.githubusercontent.com/axwaydemouseruk/swapi/main/swapi_swagger.yaml" ,
		                               customHeaders:[[name:'Authorization', value:"Basic YXh3YXlkZW1vdXNlcnVrOjkwNTAzZGYzYmE0ZDRiOWY0NTYwMWVmZTFjZDFiMzczZmY4N2RkMDQ"]]

                  println("Content: "+jsonString.content)

		  jsonObj = readJSON text: jsonString.content
		
		  println("The URL to fetch our Swagger Spec is: "+jsonObj.download_url)
		  
		  println("The form body for the next request looks like: "+"url=${jsonObj.download_url}&type=swagger&organizationId=eda42491-578a-4024-ae1d-c767f33a90fd&name=testfile102")
				  
        }
      }
    }
  stage('Stage 2- Deploy Backend API') {
      steps {
        script {
          echo 'Stage 2 - Deploy the Pensions API Swagger spec towards Axway API Manager via the API Manager REST API'
		
   		  def jsonString2 = httpRequest url: "https://apimanager.axwaydemo.co.uk/api/portal/v1.3/apirepo/importFromUrl" ,
										customHeaders:[[name:'Authorization' , value:"Basic YXBpYWRtaW46U3BhY2UqMTE3"],
										[name:'Content-Type'  , value:"application/x-www-form-urlencoded"]],
			  httpMode: "POST",
			  requestBody: "url=${jsonObj.download_url}&type=swagger&organizationId=eda42491-578a-4024-ae1d-c767f33a90fd&name=Pensions Test API"
			  
		  jsonObj2 = readJSON text: jsonString2.content

        }
      }
    }
  
  stage('Stage 3- Deploy Frontend API') {
      steps {
        script {
          echo 'Stage 3 - Deploy the Backend API into a Frontend API proxy'
		
   		  httpRequest url: "https://apimanager.axwaydemo.co.uk/api/portal/v1.3/proxies" ,
			  customHeaders:[[name:'Authorization' , value:"Basic YXBpYWRtaW46U3BhY2UqMTE3"],
					 [name:'Content-Type'  , value:"application/json"]],
			  httpMode: "POST",
			  requestBody: "{\"organizationId\": \"eda42491-578a-4024-ae1d-c767f33a90fd\",\"apiId\": \"${jsonObj2.id}\",\"name\": \"The virtualized Pensions Test API\",\"version\": \"${jsonObj2.version}\",\"retired\": false,\"expired\": false,\"path\": \"/pensions/v${jsonObj2.version}\",\"securityProfiles\": [{\"name\": \"_default\",\"isDefault\": true,\"devices\": [{\"name\": \"Pass Through\",\"type\": \"passThrough\",\"order\": 1,\"properties\": {\"subjectIdFieldName\": \"Pass Through\",\"removeCredentialsOnSuccess\": \"true\"}}]}]}"
        }
      }
    }
  }
}
