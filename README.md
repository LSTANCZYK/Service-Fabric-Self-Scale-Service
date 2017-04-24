# Service-Fabric-Self-Scale-Service
Service Fabric use the approach to create new instances of service type. Using this approach, this project create a new instance of a service type based on some metrics inside of the running service. 

This first version uses a simple way to create a new instance of service type based on the number of calls to the running service. 

## Code Example
> This is a Stateless service who create a new instance of an another ServiceType 


```<language>
    private Task CreateService(string appName)
        {

             StatelessServiceDescription serviceDescription = new StatelessServiceDescription()

            {

                ApplicationName = new Uri(appName),

                PartitionSchemeDescription = new SingletonPartitionSchemeDescription(),

                InstanceCount = 1,

                ServiceTypeName = "MailboxType", // Just an example

                ServiceName = new Uri($"{appName}/Mailbox" + new Random(7).Next(100).ToString() ) // Improve the method to generate the service name. 

            };

            return _fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);

        }
```
 

# Nexts steps: 
Today the running service needs to invoke the SaaSManager2 service to ask it to create a new instance. The main go is to use the metrics generated to create a new instance with no coding on the running service. Implement Service Fabric metrics on the running service to change the way the service will scale.

```C#
 private const int ServiceRequestStartEventId = 5;
        [Event(ServiceRequestStartEventId, Level = EventLevel.Informational, Message = "Service request '{0}' started", Keywords = Keywords.Requests)]
        public void ServiceRequestStart(string requestTypeName)
        {
            WriteEvent(ServiceRequestStartEventId, requestTypeName);
        }


```

# License

This project is licensed under the MIT License - see the this link https://choosealicense.com/licenses/mit/ for details
