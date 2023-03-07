  <!-- TABLE OF CONTENTS -->
  <details open="open">
    <summary>Table of Contents</summary>
   <ol>
    <li>
      <a href="#about-the-project">Web Service Testing</a>
      <ul>
        <li><a href="#Framework features">Framework features</a></li>
      </ul>
    </li>
    <li>
      <a href="#built-with">Tech Stack</a>
    </li>
    <li>
      <a href="#getting-started">Getting started</a>
        <ul>
                       <li><a href="#prerequisites">Prerequisites</a></li>
                       <li><a href="#installation">Installation</a></li>      
        </ul>
    </li>
    <li>
    <a href="#usage">Usage</a>
        <ul>
            <li><a href="#writing-a-test">Writing a test</a></li>
            <li><a href="#testing-environment-information">Testing environment information</a></li>
            <li><a href="#api-under-test-information">API test Information</a></li>
            <li><a href="#sample-feature-file-to-understand-the-keywords"> Understanding the keywords</a></li>
            <li><a href="#key-words-used-in-Behave-feature-files">Key Words used in Behave feature files</a></li>
            <li><a href="#running-tests">Running tests</a></li>
            <li><a href="#Test Reports">Test Reports</a>
                <ul>
                <li><a href="#default-cucumber-report">Default Cucumber report</a>
                <li><a href="#allure-reporting">Allure reporting</a>
                </ul>
            </li>
        </ul>
    </li>
    <li><a href="#faq">FAQs</a></li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#contact">Contact</a></li>
  </ol>
</details>

## Web Service Testing

This framework is an API automation testing tool used to perform the functional testing of API's based on Rest & GraphQL Services. 

It also give user a capability to compare large CSV files, csv-excel, excel to DB query result, DB query to JSON file etc..

This BDD (Behavioral-Driven Development) Methodology based framework provide capability to write feature file in Gherkin language, Since Gherkin uses plain English language to define the test, it is very easy for technical and non-technical team members to understand the test or behaviour of the feature file. 
 

### Framework features
  - Tests both REST and GraphQL based services.
  - Predefined Class / Methods to interact with REST / GraphQL APIs
  - Supports data parsing and processing of CSV, JSON, Excel,.gz, ZIP, and XML files.
  - Support data Reconciliation based on composite key (unique key with a combination of multiple columns/fields)
  - Noise filter in difference report -- space, character, decimal threshold.
  - Predefined methods to integrate with Snowflake, SQL Server, Athena databases. 
  - Generating test reports - Allure based & Custom Excel based difference report. 
  - Support for Email notification with attachment. 
  - Bitbucket pipeline integration.
  - Fully integrated with AWS services (S3, Batch, ECR, CloudWatch, Step function, Secret Manager)
  - Environment agnostic
  - Integration with Jira - can raise bug if differences are more than a given percentage. 
  - Parallel execution

### Tech Stack
* **BEHAVE** - BDD framework https://behave.readthedocs.io/en/stable/
* **Request** - for testing the API services https://pypi.org/project/requests/
* **PIP** - standard package manager/Installer for Pyhton libraries.
* **Allure** - for reporting the test results https://docs.qameta.io/allure/
* **Other Python Libraries** - check requirement.txt file for all other used python libraries.

 
## Getting Started

Follow these instructions to install this framework, configure and testing on your local machine.

### Prerequisites

- Python 3.7 + 
- IDE (IntelliJ or Eclipse or pycharm with BEHAVE Plugin)

### Installation

- Install Python 3.7 + (but not 3.9 onwards) and set path.
- Clone the repository using git.
    - git clone <url>
- Launch IDE and open the project repository.
- Now make a Python virtual environment (MAC Steps)-
    1. with command - python -m venv <path/to/new/virtual/environment>
    2. Activate you virtual env,  source <virtual env path>/bin/activate
    3. To deactivate virtual environment
        - run 'deactivate' command in your IDE terminal. 
    4. for windows, please do google or follow steps from this site -
        - https://www.geeksforgeeks.org/creating-python-virtual-environment-windows-linux/
- Set up all required python libraries to use framework- 
    1. Once you activate python virtual environment, run below steps in your IDE terminal or cmd
    2. pip install -r ./requirement.txt  [requirement.txt file has details about all libraries (with their version, like pom file in Java projects) 
       which are required to set up this framework and execute any test] 
    3. Above command will download all libraries from Internet and store in your virtual environment folder. 

    
 
 
<!-- USAGE EXAMPLES -->
## Usage

This framework is specifically designed to perform functional testing  of REST or GraphQL based API's:

 
1. Perform functional test for given Webservice or APIs - validate response status, selected attributes from response or complete response.
2. Perform Schema validation
3. Provide sample fetaure test to start with functional testing of a REST API. 
4. Provide sample fetaure test to start with functional testing of a GraphQL based API. 
5. Provide sample fetaure test to do a CSV file comparision [This test can be extended to Excel, JSON and XML files] 
 
<a id="writing-a-test" >Writing a Test</a>

* The test case (BEHAVE feature file) goes in the `/tests/features` library and should have the ".feature" extension.[tests folder is required]

* You can start writing test using reference at `features/createuser.feature`. You can extend this feature or make your own features using some of the predefined steps and reusable methods.

### Testing environment information

This framework is environment agnostic. You can provide the environment information under **./environment.yml** file. 
 
```
qa:
   ui_cache_refresh:
           cache_base_url: 'https://central-qa-caching'
           context_path: '/comments'
           follow_redirects: 'false'
   email_config:
           to_addresses: []
           from_address: ''
uat:
   ui_cache_refresh:
           cache_base_url: 'https://central-uat-caching'
           context_path: '/comments'
           follow_redirects: 'false'
   email_config:
           to_addresses: ['']
           from_address: ''
```

### API test information

As mentioned in the sample, the accelerator file will know which API to test. We just need to provide the API information.

* **API code is structred in modularized way.**

    * **Gateway.py** file have all the basic or reusable methods realted to REST & GraphQL API like Get, Post, Delete etc.
        * ```
          def __init__(self, baseurl):
                  self._baseurl = baseurl
          
              def post_request(self, path_url, data=None):
                  return self.__handlerequest(path_url, requests.post, data=data)
          
              def get_request(self, path_url):
                  return self.__handlerequest(path_url, requests.get)
          
              def get_request_no_jsonresponse(self, path_url):
                  return self.__handlerequest_nojsonresponse(path_url, requests.get)
          ```
       
        * **yourservice**_service.py file have all the methods specific to your test API and will inherited all methods of Gateway.py during execution. E.g. - SSLcertification , Timeout , change Service response to Dataframe , any custom tranformation logic.
           
           ```
          def __init__(self, baseurl):
                  super().__init__(baseurl)
          
              def get_dates(self):
                  path_url = '/today?timezone=est'    # get timezone from config or feature file
          
                  return self.get_request(path_url)
           ```
             
    * **./config/request_response_mapping.yml** this file contains request key mapping information against the response and verification of the expected matching key  and actual value in test
        * ```
          CreateUser:
            'name': 'name'
            'Income': 'Salary'
          ```
    * **./data/Service/ServiceFile.yml** this file contains the test data information to run an API test (feature file)
        * ```
          User1:
            name: 'microservice_test_accelerator'
          ```
         
### Sample Feature file to understand the keywords

```
Feature: User registration
# In this example we're setting the API name to 'jsonplaceholder' - the test will automatically pick the 'User1' data from testdata.yml file and replace the values in request template and hit the endpoint based on the 'host_uri' in 'envconfig.yml' file under 'resources/config' and base_path in request.json template. base_path value will also drive from envconfig.yml

# In the last step we are verifying the response against the complete request. The test will pick the key from 'request_response_mapping.yml' under the resources/schema/<env> and will verify the response value againt the request

  @regression @Sanity
  Scenario Outline: User Post Details
    Given A User access User registration API endpoint <API> --> This step will Understand which API to test. Make sure your API name here should match with Dictionay name in config file. 
    And set request body <RequestBody> --> This step will Understand which Test data to use for API to test. API Test data information Exists under the ./data/Service/ServiceFile.yml and will user the "User1" Testdata from ServiceFile.yml file
    When I call method POST --> This step will perofrm the API POST Operation
   Then I verify response code is <scode>
    Then I verify "CreateUser" in Response --> This step will verify the reponse verfication against the simple request being sent, This step will understand to pick the request reponse mapping from ./config/UserService.yml  and perform the verification for each key mapping present in request_response_mapping.yml file.
    Examples:
      | API             | RequestBody |scode|
      | jsonplaceholder | User1       |201|
```

 
```
@recontest
Feature: Compare CSV1 with CSV2 and save result file

Scenario Outline: Compare 2 csv file
   When I save CSV1 <filepath1> data as actual dataframe
   And save CSV2 <filepath2> data as expected dataframe
   Then compare both CSV based on key <fieldnames>
   And Save differences to result file <resultfilename>
   Examples:
         |filepath1 | filepath2 |fieldnames|resultfilename|
         |./data/actual/file/<filename1.csv>|./data/actual/file/<filename2.csv>|SOURCEID,METRICKEY|./result/difference.csv|

```

```
** In graphQL, we send the requested attributes part of request body and response will contain only those required attribute not complete API response. 

Feature: Fetch all films with their title from Star wars graphql api

Scenario Outline: Get films title from Star war graphql api
   Given A User access Star war api endpoint <API> 
   And set request body <RequestBody>
   When I call method GET
   Then I verify response code is <scode>
   Then I verify service Response with expected result <filepath>
   Examples:
         |API | RequestBody |scode|filepath|
         |Starwar_netlify| {allFilms {films {title } } }|200|./AllfilmsTitle.csv|

```

```

### Key Words used in Behave feature files

- Feature (A suitable name for your scenarios in the feature file)
- Scenario (A suitable name for the scenario you are trying to write)
- Given, When, Then, And, But (step keywords to describe the flow)
- Background (Common step to run for all scenarios in that feature file)
- Scenario Outline
- Examples (Using Scenario Outline and Examples, scenario can be repeated with multiple values from Examples table)
- | (Data Tables) - Data Tables can be used to pass multiple values in the same step
- @ (Tags) - Particular scenario/feature can be executed by running with tags.
- '#' can be used to provide Comments
BEHAVE Tutorials: https://behave.readthedocs.io/en/stable/
```


### Test Execution

Running tests in local [Test execution in aws can be discussed in later versions] 

```
There are couple of below mentioned ways to execute test in local- 
 
1. Run test from command line [Terminal in mac]

     *. Make sure if you are running test from command line then you should be in virtual env else you will get
         bash: behave: command not found error
         To activate virtual enviornment, please follow Installtion steps #2.
       
        
    *. Command to run all test cases under any domain folder :

        behave tests/features/service/ -o  allure_result_folder 
       
        a. In above command 'tests/features/service/' is path of folder which have .feature file and steps folder
        b. -o option is to provide the path to result json file which Allure can use and then make a report out of this.

    *. Command to run test cases on any given Environment and for just sanity testcases :
        
        behave tests/features/service/ -D ENV=dev -o allure_result_folder --tags @Sanity
        
        a. In above command -D is the option to set your run time variables and pass to test cases code during execution, like ENV=dev
        b. --tags or -t is the option to execute only test which have @position_sanity tag in all .feature files under tests/features/service/

 
 2. Run test from IDE [IntelliJ or PyCharm] -
    
    
        1. Set up Run/Debug Configuration
        2. Select test type as BEHAVE and select Script path as your feaature file which you want to execute. 
        3. Provide Envrionment variable as , ENV=Dev or other variables , semi colon seperated. 
        4. Use interpreter path as <your Virtual environment path> /bin/python.

 
 3. For parallel execution of feature files in same enviorment - 
 
        1. Mention all the feature file which you want to execute in feature_list.txt file [location - project root folder]
        2. then change env name in behave_parallel.py file
        3. from command line - python  behave_parallel.py
        4. This will run all feature files in parallel and make different logs file for each test.
    
```
<!-- Test Reports -->
### Test Reports
This accelerator supports test reporting using default BEHAVE reports and Allure reporting. Differene report is saved as excel report and can be shared via email.
#### Default Behave report
  You can find more details about Behave report details: 
    - https://behave.readthedocs.io/en/stable/formatters.html


  #### Allure reporting
  This accelerator also integrates with the Allure reporting framework. You can find the report after the Tests Execution
  * directory for Allure report in the current project, **./allure_result_folder** by default
  
 
## FAQs
**Q:** Does this Accelerator Support all HTTP status codes assertion
**A:** Yes, We can assert any HTTP status code using common step **Then I verify response code is 201**

**Q:** Does this Accelerator has Ability to manage test data across multiple Environment
**A:** Yes, You can manage the test data for each environments under **./data/service/env/**

**Q:** Does this Accelerator Environment Agnostic
**A:** Yes, You can provide multiple environments configuration in **src/test/resources/config/envconfig.yml**
##
**Q:** Does this Accelerator Support Parallel Execution of Tests
**A:** Yes, This Accelerator supports parallel Execution of Tests, Please follow steps writen above in section 'For parallel execution of feature files in same enviorment'
  
##
**Q:** Does this Accelerator has In-built Test Reporting

**A:** Yes, This Accelerator Support Default Behave Reporting as well As Allure Reporting. Better custom excel reporting is also an option which is preferred in data testing projects.
##
**Q:** Does this Accelerator has ability to evaluate headers and cookies
**A:** Yes, you can evaluate any headers and cookeis of api under test and save it in testcontext for further use.

##
**Q:** Does this Accelerator supports Schema Validation
**A:** Yes, This Accelerator Support Schema Validation using JSON Schema Validation with jsonschema library https://python-jsonschema.readthedocs.io/ 
 
<!-- CONTRIBUTING -->
## Contributing

Contributions are what make the open source community such an amazing place to be learn, inspire, and create. Any contributions you make are **greatly appreciated**.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/python_service`)
3. Commit your Changes (`git commit -m 'Add some change/addition related comments  '`)
4. Push to the Branch (`git push origin feature/python_service`)
5. Open a Pull Request
