# HL-PA

## Backend Questions

### Answer of Question 1.

For an overview of the system follow the question. So, the design architecture diagram is shown in Fig. 1.1 below. The system starts with 3 microservices: customer service, master data service, and transaction data service. Each microservice is the necessary database owner like a customer database, master data database, and transaction data service respectively. When an external request is received, it is first handled by the API Gateway. This setup aligns with the microservice architecture pattern.

To provide API for display data from 3 microservices above nearly real-time. The design adds a new microservice named Data Aggregate Service to handle aggregate from the 3 microservices above which get data via message broker and return data aggregated back to the client. For the nearly real-time provided data aggregated necessary to have caching data at the Data Aggregate Service.

![architecture diagram](https://res.cloudinary.com/dmdxfjunb/image/upload/v1720244389/HLAB-Architecture_Diagram_oopsbw.jpg) \
Fig. 1.1: Architecture diagram 


The component of each microservice is shown in the class diagram in Fig. 1.2 below. API Gateway has the APIGatewayController class which routes the request to internal microservices. All microservices include data aggregate service consisting of controller class handle request, service class handle about business logic as same as. However 3 microservices: customer service, master data service, and transaction data services have entity classes of data that each microservice owns. Every microservices and API gateway has the authorization class with concern about the security of the system.

![class diagram](https://res.cloudinary.com/dmdxfjunb/image/upload/v1720244482/HLAB-Class_Diagram_nkw46w.jpg) \
Fig. 1.2: Class diagram 


When have a request to get customer data, master data, and transaction data. Begin The collaboration between the API gateway and each microservice follows the sequence diagram in Fig. 1.3 below. The request is sent to the API gateway first and authorized request by using the userId and token from the request. If the access type is true, the request and parameters like userId and token are sent to the data aggregate service. Before the data aggregate service executes must be authorized by using userId and token same. Next, the operation executed by DataAggregateService starts with call data from the cache. If the cache does not have the data, the service must have call data from  3 microservices via message broker by using a message pattern. Finally, after calling all data successfully the service just aggregates data before saving the data to the cache and return to the client

![sequence diagram](https://res.cloudinary.com/dmdxfjunb/image/upload/v1720244488/Untitled_19_sylyvk.png) \
Fig. 1.3: Sequence diagram 

Finally, the client can call API to get all data with API path name /getDataAggregated and must send the parameter userId and token within the request.

### Answer of Question 2.

![sequence diagram](https://res.cloudinary.com/dmdxfjunb/image/upload/v1720285851/HLAB-performance_test_js35gn.jpg) \
Fig. 2.1: Activity diagram of performance testing
The performance test in this release recommends that proceed to conform to the activity diagram in Fig. 2.1 above which consists of defining the objective, identifying the scope, creating a test plan, selecting tools, executing, analyzing, reporting, and optimizing software. The details are as follows:
1. Define performance test objective 
Form non-functional requirement can identify the KPI of performance testing. What is the indicator of the testing, speed, response time, throughput, load, resource usage, and stability? And define the criteria of each KPI. This process output is the test objective and criteria.
2. Identify scope
Identify which components, modules, and integrations will be tested. This process output is the test scope.
3. Create test plan
Design performance test cases of each scenario. Plan sequence and timing of test case by concern about coverage and config of test case. This process output is a test case and schedule.
4. Select test tools
Select tools for performance testing in each KPI like Gatling([https://docs.gatling.io/tutorials/scripting-intro-js/](https://docs.gatling.io/tutorials/scripting-intro-js/)), LoadRunner([https://www.opentext.com/en-gb/products/loadrunner-professional](https://www.opentext.com/en-gb/products/loadrunner-professional)), JMeter([https://jmeter.apache.org](https://jmeter.apache.org)), and etc.
5. Execute test
From the test case, test plan, and test tool execute the performance test and collect the test result. In this process, the output is the test result.
6. Analyze and reports
Compare actual performance with criteria of testing. Identify issues of performance to be optimized. Develop a report of performance test.
7. Optimize 
If the performance test result is not good or does not pass criteria can optimize the software and re-testing.

### Answer of Question 3.
Source code of the system can access with [https://github.com/Saridraj/hl-pa-product-api.git](https://github.com/Saridraj/hl-pa-product-api.git). 
- API endpoint \
  For create product use method POST to path /products and send request body like below structure.

   ```bash
  {
    translation: [
        {
            languageCode: string,
            name: string,
            description: string
        }
    ],
    createdBy: number
  }
  ```

   For search product by name in any language use method GET to path /products/search and send query like name, page, and limit. example in below

   ```bash
   http://localhost:3000/products/search?name=Phone&page=2&limit=5
  ```

- data validation handle by Type determination in entity class

- system design \
  The system consist of 3 important class is ProductController class handle routing when have the request about create product or search product, ProductService class handle about create product operation, and ProductTranslationService handle about addProductTranslation and search product. Both service class work with Product entity and ProductTranslation entity which have a field detail follow Fig 3.1

  ![](https://res.cloudinary.com/dmdxfjunb/image/upload/v1720383229/HLAB-Page-4_vahzgk.jpg) \
  Fig. 3.1 class diagram


  So, the system have 2 table on database is Product table show in Fig. 3.2 and ProductTranslation table show in Fig. 3.3.

    ![](https://res.cloudinary.com/dmdxfjunb/image/upload/v1720383224/Screenshot_2567-07-08_at_03.12.28_riuizl.png) \
   Fig. 3.2 Table of product

    ![](https://res.cloudinary.com/dmdxfjunb/image/upload/v1720383226/Screenshot_2567-07-08_at_03.12.13_iz58yg.png) \
   Fig. 3.3 Table of product translation

  Design of database use Additional Translation Table Approach that ease in adding a new language and easy to query.


- testing \
  plan to test the system in 3 level is unit test, integration test (end-to-end), and API test follows:
    1. unit test: Use jest execute unit test. The system have 2 file component need to execute unit test is product.service.ts and product.controller.ts by script in product.service.spec.ts and product.controller.spec.ts. In the product.service.spec.ts must have test case about creation of product and search product by name cover two class (ProductService, ProductTranslationService) in this service file. For product.controller.spec.ts must have test script about product creation and product search by name.
    2. e2e test: User jest e2e test. in test folder of the project have file app.e2e-spec.ts which collect the test script to test end-to-end form app module to product service. Test script must have cover to create product operation and search product operation.
    3. API test: Use postman execute API test. Send request with http method to path of API and send request body and query parameter within the request. this testing nearly request sending from the client and help to check avaliable of the API system.


## React Questions
### Answer of Question 1.
React useCallback is used to memorize callback functions reducing unnecessary re-renders to optimize performance.

The callback function is a function that passes as an argument to another function or child component. When used callback function without useCallback affects child component unnecessary re-render.

To prevent unnecessary re-rendering of child components can use useCallback. It avoids new functions in every render. But it caches the callback function and re-renders it when having dependency change only.

### Answer of Question 2.
Test script repository of unit test can access with 
[https://github.com/Saridraj/hl-pa-react-unit-testing.git](https://github.com/Saridraj/hl-pa-react-unit-testing.git)) 

The project consist of main component is src/app/page.tsx that is a Home page of the project and have a UserProfile component in folder components. Folder __testr__ collect the test script like Home.test.tsx and UserProfile.test.tsx.

For unit test of UserProfile component handle by script in UserProfile.test.tsx file which have 3 test case.



