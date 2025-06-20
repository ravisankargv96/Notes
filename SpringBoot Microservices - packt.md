

## Chapter03
```
1-spring-init
	microservices (it's a gradle project: gradle/wrapper, )
		product-service : @springbootApp (spring-boot-starter-actuator,webflux,test, reactor-test)
		product-composite-service : @springbootApp (same dependencies)
		review-service : @springbootApp (same dependencies)
		recommendation-service : @springbootApp (same dependencies)
```

```
2-basic-rest-services
	microservices : 4 x @springbootApp
		product-composition-service (@sbA)
			services/
				ProductCompositeIntegration.java
				ProductCompositeServiceImpl.java
			ProductCompositionServiceApplication.java
		
		product-service (@sbA)
			service/
				ProductServiceImpl.java
			ProductServiceApplication.java
		
		recommendation-service (@sbA)
			service/
				RecommendationServiceImpl.java
			RecommendationServiceApplication.java
		
		review-service (@sbA)
			service/
				ReviewServiceImpl.java
			ReviewServiceApplication.java
			
	api : @sbA
		composite/product
			ProductAggregate.java
			ProductCompositeService.java
			RecommendationSummary.java
			ReviewSummary.java
			ServiceAddress.java
		
		core
			product
				Product.java
				ProductService.java
			recommendation
				Recommendation.java
				RecommendationService.java				
			review
				Review.java
				ReviewService.java
				
		exceptions
			InvalidInputException.java
			NotFoundException.java
			
	util : @sbA
		GlobalControllerExceptionHandler.java
		HttpErrorInfo.java
		ServiceUtil.java
```

###### Util:
```
GlobalControllerExceptionHandler
	- LOG : Logger
	+ handleNotFoundException():HttpErrorInfo
	+ handleInvalidInputException(): HttpErrorInfo
	+ createHttpErrorInfo(): HttpErrorInfo

HttpErrorInfo
	- timestamp : ZonedDateTime
	- path : String
	- httpStatus : HttpStatus
	- message : String
	%% constr(), constr(allArgs), getter() & setter() %%

 ServiceUtil (:@component)
	 ~ LOG : Logger
	 - port : String
	 - serviceAddress : String
	 + getServiceAddress() : String
	 + findMyHostname() : String
	 + findMyIpAddress() : String
```

###### api:
```
ProductAggregate
	- productId : int
	- name : String
	- weight : int
	- recommendations : List<RecommedationSummary>
	- review: List<ReviewSummary>
	- serviceAddresses : ServiceAddresses

	%% constr(), constr(allArgs) %%


ProductCompositeService (I)
	+ getProduct(int) : ProductAggregate

RecommendationSummary
	- recommendationId : int
	- author : String
	- rate : int
	%% constr(), constr(allArgs) %%

ReviewSummary.java
	reviewId : int
	author: String
	subject : String
	%% constr(), constr(allArgs) %%

ServiceAddress.java
	- cmp : String
	- pro : String
	- rev : String
	- rec : String
```


```
Product
	productId : int
	name : String
	weight : int
	serviceAddress : String
	%% constr(), constr(allArgs) %%

ProductService (I)
	getProduct(int) : Product

Recommendation
	productId : int
	recommendationId : int
	author : String
	rate : int
	content : String
	serviceAddress : String

RecommedationService
	getRecommendations(int) : List<Recommendation>


Review
	productId : int
	reviewId : int
	author : String
	subject : String
	content : String
	serviceAddress : String

ReviewService
	+ getReviews(int) : List<Review>
```

```
InvalidInputException
	constr()
	constr(String)
	constr(String, Throwable)
	constr(Throwable)
```

###### ToBeContinued...
