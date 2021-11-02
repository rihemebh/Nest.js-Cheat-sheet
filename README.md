# Nest Cheat Sheet

- [1. Let's start](#lets-start)
  - [1.1 Installation](#installation)
  - [1.2 New Project ](#new-project)
  - [1.3 Launch Project](#launch-project)
- [2. Modules](#module)
  - [2.1 Creation](#creation)
  - [2.2 Parameters](#parameters)
- [3. Controllers](#controllers)
  - [3.1 Creation](#creation-1)
- [4. Routes](#routes)
  - [4.1 Params](#params)
  - [4.2 Annotations](#annotations)
  - [4.3 Generic URI](#generic-uri)
- [5. DTO](#dto)
- [6. Dependency Injection (DI)](#dependency-injection-di)
- [7. Providers](#providers)
  - [7.1 Injection types](#injection-)
  - [7.2 Services](#services)
- [8. Request Lifecycle](#request-lifecycle)
  - [8.1 Middlewares](#middlewares)
  - [8.2 Pipes](#pipes)
  - [8.2 Interceptors](#interceptors)
- [9. Filters](#filters)
  - [9.1 Custom Filters](#custom-filters) 
- [10. Configuration](#configuration-variables)
- [11. ORM: Database Acess](#database-access)

## Let's Start

### Installation 
    npm i -g @nestjs/cli

### New Project 
    $nest new <project-name>
    
### launch project 
    npm run start:dev
   Or 

    nest start –watch
    
## Module

- A Modules is an isolated part from the project that does a certain role 
- The default module of nest is the root module 
- Modules are annotated with ``@Module(parameters)``

### Creation
```js
nest g mo ModuleName
```

### Parameters
- **Providers**: which will be instantiated by the Nest injector and which can be shared at least on this module.
- **Controllers**:all the controllers defined in this module 
- **Imports**: the list of imported modules which export the providers required in this module.
- **Exports**: The providers provided by this module and which can be used by other modules


## Controllers 

Controller is a class annotated with ``@Controller`` that contains a list of actions to accept the client's requests according to the route

### Creation
```js
nest g co controllerName
```

*To make a module visible from all the application you need to annotate it by  ``@Global`` *
 
 
## Routes

A route will identify the uri associated to an action.
### Params 

```js 
@Get('uri/:param/:param1?')

```

***Param?*** : Means that this parameter is optional 

### Annotations 

 |name|description|
 |---|---|
 |``@Post()`` , ``@Get()`` , ``@Delete()``,  ``@Put()`` , ``@Patch()``|Accepts a HTTP request|
 |``@Body()`` | retrieve the POST request body |
 |``@Res()``| response |
 |``@HttpCode(code)``|customize the HTTP code |
 |``@Header()``||
 |``@Param(<name>)``|retirive params from the uri|
 
#### Generic URI 
``*`` : 0 or plus
```js
@Get('test*') : //any uri that begins with test
```

``+`` : 1 or plus 

``?`` : 0 or plus 


## DTO

- It is an object that allows to define how the data is sent via the network.
- DTOs are not the models, in many cases the model and data you wish to receive is different.

|They can be defined using classes or interfaces, but Nest recommends using classes as TypeScript does not save metadata for generics and interfaces|
|---|

## Dependency Injection (DI)
 
 Nest is built around the strong design pattern commonly known as Dependency injection
 
| Dependencies are services or objects that a class needs to perform its function. Dependency injection, or DI, is a design pattern in which a class requests dependencies from external sources rather than creating them.|
|---|

### How the DI container works ?
Providers normally have a lifetime ("scope") synchronized with the application lifecycle.

1 - When bootstraping the application, the DI container registers all the classes.

2 - For each class, the container will identify its dependencies.

3 - The container will generate all the dependencies to create the desired instance for us.

4 - The created instance will be saved and reused if necessary

When the application shuts down, each provider will be destroyed.



## Providers
- Providers are a fundamental concept in Nest, Many of the basic Nest classes may be treated as a provider
    - Services
    - Repositories
    - Factories
    - Helpers

- It can be injected as dependency with the annotation : ``@Injectable()``

***Documentation Hint*** : Since Nest enables the possibility to design and organize dependencies in a more OO-way, we strongly recommend following the SOLID principles.

### Injection : 

#### 1. Constructor-based injection 
```typescript 
constructor(private catsService: CatsService) {}
```
The CatsService is injected through the class constructor.
Notice the use of the private syntax.
This shorthand allows us to both declare and initialize the catsService member immediately in the same location.


#### 2. Property-based injection

 In some very specific cases, property-based injection might be useful. For instance, if your top-level class depends on either one or multiple providers, passing them all the way up by calling ``super()`` in sub-classes from the constructor can be very tedious. In order to avoid this, you can use the ``@Inject()`` decorator at the property level.
 
 ```typescript 
 @Injectable()
export class HttpService<T> {
  @Inject('HTTP_OPTIONS')
  private readonly httpClient: T;
}
```
If your class doesn't extend another provider, you should always prefer using constructor-based injection.

### Services 
The only role of a controller should be : **Accept the client requests**
what we will going to do with this request should be transfered to the business layer that's why we have **Services**
          
     $ nest generate service <service name>
   or
  
     $ nest g s <service name>
     
 -> Encapsulates some functionnality 
 
 -> Reduces redundant code : different controllers could use the same service 
 
 *The service must be provided by the parent module or exported by imported modules.*


## Request Lifecycle

 A request flows through middleware to guards, then to interceptors, then to pipes and finally back to interceptors on the return path (as the response is generated).
 
<img src="https://github.com/rihemebh/Nest.js-Cheat-sheet/blob/main/lifecycle.png" width="800" height="400" />

### Middlewares
A Middleware is quite simply a function called before the request is processed by the controller
- Role : 
   - Execute any code.
   - Make changes to the request and the response objects.
   - End the request-response cycle.
   - Call the next middleware function in the stack
   - if the current middleware function does not end the request-response cycle, it must call ``next()`` to pass control to the next middleware function. Otherwise, the request will be left hanging.


To use middlewares: 
1. AppModule should implement NestModule
2.  Implement the ``configure`` method to specify which middleware to use in this app ana for which Route 
  ```typescript 
     configure(consumer: MiddlewareConsumer): MiddlewareConsumer | void {
       consumer.apply(FirstMiddlewar)
       .forRoutes('courses');
     }
```
We could also restrict the use of middlewares to specific HTTP functions 
  ```typescript 
.forRoutes(
{path: 'courses', method: RequestMethod.GET},
);

// forRoutes(...routes: (string | Type <any> | RouteInfo)[]): MiddlewareConsumer
  ``` 






 
A Middleware could be a class or a function 
- Class : 
    - the should implement ``NestMiddleware`` interface then implement its use function that accepts ``Request`` , ``Response`` and the ``Next`` method.
       ```typescript
           import { NestMiddleware } from '@nestjs/common';
           export class FirstMiddlewar implements NestMiddleware{
           use(req: any, res: any, next: () => void): any {
            }
           }
        ```
    - 

- Function : create a function that accepts ``Request`` , ``Response`` and the ``Next`` method.

And you could use all express middlewares like : 
- [Morgan](https://www.npmjs.com/package/morgan): offers a logging feature on the requests.and it has several log formats that you can configure and
adapt. 
  -``app.use(morgan('dev'));``
- [Helmet](https://www.npmjs.com/package/helmet): secure your requests by adding HTTP headers. 
  - ``app.use(helmet());``
- [Cors](https://www.npmjs.com/package/cors#enable-cors-for-a-single-route): allows you to manage permissions to resources from another domain.
  - ``app.enableCors()`` : this function accepts an optional object where we could specify the origin domain / HTTP methods / allowed headers / optionsSuccessStatus : (200-201..)
  - Alternatively, enable CORS via the create() method's options object: ``const app = await NestFactory.create(AppModule, { cors: true });`` : only applies to **REST** endpoints
  
  
#### Global Middlewares 

   You can declare the middleware in the ``use(<middlewarename>)``  function of main.js
   
 ------------------------------------------
### Pipes

- Nest calls the pipe just before invoking a method to transform or evaluate its params

We have 2 different types of pipes 
||Transformation Pipes|Validation Pipes|
|---|---|---|
|Explanation| Transform input data to the desired form (e.g., from string to integer)|Evaluate the input data and throw an exception if invalid  |
|Installation| auto |``npm i --save class-validator class-transformer``|
|Where ?|Add it to the property that we want to pipe (Body , Param , Query ...)|Annotate properties|
|Activation|auto |Globally : ``app.useGlobalPipes(new ValidationPipe({transform: true, whitelist: true}))`` <br/> In a specific route: ``@UsePipes(PipeClass1, PipeClass2,…)``|
|Example of use |1. with dependecy injection <br/> ``@Param('id', ParseIntPipe)`` <br/>2. with class instantiation ( where we could customize the error message ) <br/>``@Param('id', new ParseIntPipe({errorHttpStatusCode: HttpStatus.NOT_ACCEPTABLE})`` <br/>|1. specify the error message with a simple string : ``@MinLength(20, {message: "$property should has at least $constraint1 characters "})`` <br/> ``title: string;`` <br/>1. specify the error messages with a function :``@MinLength(20, {message: (validationData: ValidationArguments) => { return `the size of ${validationData.property} ${validationData.value} is too small, you should at least have  ${validationData.constraints[0]} characters `}`` |

#### Mapped Type
``@nestjs/mapped-types``

- **PartialType** : Returns the targeted class by setting all the fields to Optional.

- **PickType** : Allows you to create a new type (a class) by choosing one set of fields of an existing class.

- **OmitType** : Allows you to create a new type (a class) by removing a set of fields of an existing class.

- **IntersectionType** :  Allows you to create a new type (a class) fr designated fields that exist in two types.

#### Custom pipes
We have some Built-in pipes exported from the ``@nestjs/common`` package but we can also create our own ones :

- A pipe is a class that implements the ``PipeTransform,`` interface. This interface asks you to implement the ``transform()`` method, this method takes the value to be transformed as a parameter and metadata.
- MetaData : ``@nestjs/common``
  - **Type**: indicates the type of the argument which can be a body with ``@Body``, a queryParam with ``@Query``, a parameter with ``@Param``.
  - **Metatype**: indicates the type of the parameter, for example String.
  - **Data**: the data passed to the decorator

```typescript 
import { ArgumentMetadata, PipeTransform } from '@nestjs/common';

export class FusionUpperPipe implements PipeTransform{
transform(value: any, metadata: ArgumentMetadata): any {
console.log(metadata);
return value;
}
}
```
 ------------------------------------------
### Interceptors 
Interceptors are classes annotated with ``@Injectable()`` and Implement the interface ``NestInterceptor``

**Role**: 
- Add additional logiqic before and after the method execution
- Transform the response 
- Transform the thrown exception 


1. Context : execution context of the query (exp rest query )
2. next : CallHandler is a method that has the method handle as param and return observable

```typescript 
import {Injectable, NestInterceptor, ExecutionContext, CallHandler} from '@nestjs/common';
import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';

@Injectable()
export class MyFirstInterceptor implements NestInterceptor {

intercept(context: ExecutionContext, next: CallHandler): Observable<any>
{
 // code executed before the request 
 
return next.handle().pipe(tap(() => 

// code executed after the request 

));
}
}
```

Interceptors has 3 scopes : 
1. Routes 
```typescript 
@Get('')
@UseInterceptors(RequestDurationInterceptor)
getAllProducts() {
return this.produitService.getAllProduits();
}
```
2. Controller 
```typescript 
@UseInterceptors(RequestDurationInterceptor)
@Controller('post')
export class PostController {
constructor(
public service: PostService
) {
}
```
3. Global
```typescript 
app.useGlobalInterceptors(
new ErrorHandlerInterceptor(),
new RequestDurationInterceptor(),
new TransformInterceptor(),
new ExcludNullInterceptor() );
```
## Filters
 Filters are used to manage exceptions
 
       NestJs comes with his own exception management layer that handles all the HTTP exceptions .
       - If you don't handle the exception Nest will do it for you 
       - If an exception isn't recognized by this filter a default exception is triggered : the famous "Internal  server error" with the "500 status"
  HttpException is a class given by Nest to handle all the HTTP exceptions, its constructor takes 2 arguments : response and status
  
  Example
 ```typescript
   throw new HttpException("You couldn't mention an id ", 400);
 ```
 -> All the standard Exceptions are inherited from this class 
 
 ### Custom filters 
 
  - Implement th ``ExceptionFilter`` interface 
  - Annotate the class with ``@Catch(<exception type>)``
  - Implement the ``catch(<exception>,ArgumentHost)`` method : ArgumentHost imported from ``@nestjs/common``
  
  Example : 
  ```typescript
 import { ArgumentsHost, Catch, ExceptionFilter, HttpException } from '@nestjs/common';
import { Request, Response } from 'express';
@Catch(HttpException)
export class CustomFilter implements ExceptionFilter{
catch(exception: HttpException, host: ArgumentsHost): any {
  const ctx = host.switchToHttp(); // give the error context like the response , the request ..
  const response = ctx.getResponse<Response>();
  const request = ctx.getRequest<Request>();
  const exceptionResponse = exception.getResponse();
  
  response
  .status(status)
  .json({
    message: 'custom response',
    statusCode: exception.getStatus(),
    timestamp: new Date().toISOString(),
    path: request.url,
  });
  
return response;
}
  ```
 
- Make this filter global for all requests by 

 1. Adding it to the list of providers in the main module :
 ```typescript
 providers: [
  {
    provide: APP_FILTER,
    useClass: CustomFilter,
  } ],
 ```
  2. Using ``useGlobalFilters`` in our app of main.js
  ```typescript
  app.useGlobalFilters(new CustomFilter())
  ```
  
  - To attach a filter to a specific method use ``@UseFilter(<filter instance/class>)`` : note that if we don't write any parameter this method will use all filters 
       
         Prefer applying filters by using classes instead of instances when possible. 
         It reduces memory usage since Nest can easily reuse instances of the same class across your entire module.
  
## Configuration Variables
// to be added 


## Database Access 


### ORM 

Reference : [Decorators](https://github.com/typeorm/typeorm/blob/master/docs/decorator-reference.md)
