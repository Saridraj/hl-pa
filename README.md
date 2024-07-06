# HL-PA

## Backend Questions
### Answer of Question 1.

For an overview of the system follow the question. So, the design architecture diagram is shown in Fig. 1.1 below. The system starts with 3 microservices: customer service, master data service, and transaction data service. Each microservice is the necessary database owner like a customer database, master data database, and transaction data service respectively. When an external request is received, it is first handled by the API Gateway. This setup aligns with the microservice architecture pattern.

To provide API for display data from 3 microservices above nearly real-time. The design adds a new microservice named Data Aggregate Service to handle aggregate from the 3 microservices above which get data via message broker and return data aggregated back to the client. For the nearly real-time provided data aggregated necessary to have caching data at the Data Aggregate Service.

![architecture diagram](https://res.cloudinary.com/dmdxfjunb/image/upload/v1720244389/HLAB-Architecture_Diagram_oopsbw.jpg) \
Fig. 1.1 : Architecture diagram 


The component of each microservice is shown in the class diagram in Fig. 1.2 below. API Gateway has the APIGatewayController class which routes the request to internal microservices. All microservices include data aggregate service consisting of controller class handle request, service class handle about business logic as same as. However 3 microservices: customer service, master data service, and transaction data services have entity classes of data that each microservice owns. Every microservices and API gateway has the authorization class with concern about the security of the system.
![class diagram](https://res.cloudinary.com/dmdxfjunb/image/upload/v1720244482/HLAB-Class_Diagram_nkw46w.jpg) \
Fig. 1.2 : Class diagram 



![sequence diagram](https://res.cloudinary.com/dmdxfjunb/image/upload/v1720244488/Untitled_19_sylyvk.png) \
Fig. 1.3 : Sequence diagram 




   

