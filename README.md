This org contains projects related to spring cloud and step needs to follow in order to successfully configure them.


1) Monolithic Application :
	A application or service which is single code base, tightly coupled and difficult to scale called as "Monolithic Application".

2) Microservice:
	A service which was developed as single component which is not tightly coupled to any other service and easy to scale called as "Microservice".  

3) Spring Cloud:
	To manage microservice we can you Spring Cloud.

4) Spring Cloud Projects:

  a) Spring Cloud Open Feign - For Inter microservice communication using HTTP (Open Feign provides declarative to invoke service)
          #Follow below steps to create Feign client
	  step 1: include feign dependancy in pom.xml
		<!-- For Feign Client -->
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-openfeign</artifactId>
			<version>${spring.cloud.version}</version>
		</dependency>          
	  step 2: create package for Feign clients (eg: com.krugol.feignclients)
          step 3: Create interface for Feign clients as below in com.krugol.feignclient package as below
		@FeignClient(value="ADDRESS-SERVICE", path="/api/address")
		public interface AddressFeignClient {
		
			@GetMapping("/getById/{id}")
			public AddressResponse getById(@PathVariable long id);
		}
          step 4: Enable Feign client into ApplicationMain class using @EnableFeignClients("com.krugol.feignclients") 

-----------------------------------------------------------------------------------------------------------------------------------------

  b) Spring Cloud Netflix Eureka - For Service discovery and Registry(Communicate using service name)
	   #Follow below steps for setting up Netflix Eureka server and registering microservice with Eureka server
	  step 1: include Eureka Dependency in pom.xml
		   <dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
		   </dependency>
	  step 2: Add @EnableEurekaServer to main bootstrap class
	  step 3: To avoid self registeration of Eureka server add below properties to application.propertes file
			eureka.client.register-with-eureka=false
			eureka.client.fetch-registry=false	
---------------------------------------------------------------------------------------------------------------------------------------

  c) Spring Cloud Load Balancer - For Client side load balancing 
	   #Dividing load coming for perticular microservice
	 step 1: include dependency in pom.xml
		  <!-- For spring cloud loadbalancer -->
		  <dependency>
		        <groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-loadbalancer</artifactId>
			<version>${spring.cloud.version}</version>
		 </dependency>
	 step 2: Add configuration class as below 
		 @LoadBalancerClient(value="ADDRESS-SERVICE")
		 public class AdrSerLoadBalConfig {

			@LoadBalanced
			@Bean
			public Feign.Builder fiegnBuilder(){
			return Feign.builder();
			}
		}
-------------------------------------------------------------------------------------------------------------------------------------

  d) Spring Cloud API Gateway  - For performing common login(Authentication, Request headers ,logging)
------------------------------------------------------------------------------------------------------------------------------------
  e) Fault Tolerance - If one service is down it should not impact other services
                     - We are going to use Resilience4j dependency for managing failure
		     - There are 3 state in circuit breaker Open,Close,Half Open
 		     - slidingWindowSize (How many call we want to considered)
		     - failedRateThreshold (After how many failed called circuit should open)
		     - waitDurationInOpenState =30s
		     - Automatic from open to half-open
		     - permittedNumberOfCallsHaldOpenState
		     
------------------------------------------------------------------------------------------------------------------------------------
  f) Sleuth and Zipkin - Used for distributed tracing
		       - To capture failure across multiple microservice
		       - Format Followed : {Service-Name, TraceId(Unique across all api), SpanId(unique for a request within same service)}
		       - Zipkin : Dashboard for log  (We need to download jar from internet)
------------------------------------------------------------------------------------------------------------------------------------ 
		
  g) Config Server - To manage enviroment specific properties 
		    
