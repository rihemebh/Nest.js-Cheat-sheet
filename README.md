# Nest Cheat Sheet

- [1. Let's start](#lets-start)
  - [1.1 Installation](#installation)
  - [1.2 New Project ](#new-project)
  - [1.3 Launch Project](#launch-project)
- [2. Modules](#module)
  - [2.1 Creation](#creation)
  - [2.2 Parameters](#parameters)
- [3. Controllers](#controllers)
  - [3.1 Creation](#creation)
- [4. Routes](#routes)
  - [4.1 Params](#params)
  - [4.2 Annotations](#annotations)
  - [4.3 Generic URI](#generic-uri)
- [5. DTO](#dto)
- [6. Dependency Injection (DI)](#dependency-injection-(di))
- [7. Providers](#providers)
  - [7.1 Injection types](#injection)
  - [7.2 Services](#services)
- [8. Request Lifecycle](#request-lifecycle)
  - [8.1 Middlewares](#params)
  - [8.2 Pipes](#pipes)
  - [8.2 Filters](#filters)
  - [8.2 Interceptors](#interceptors)
  - [8.2 Configuration files](#configuration-files)
- [8. ORM: Database Acess](#request-lifecycle)

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

- In Nest we use Modules which is an isolated part from the project that does certain role 
- the default module of nest is the root module 
- modules are annotated with ``@Module(parameters)``

### creation
```js
nest g mo ModuleName
```

### Parameters
- **Providers** les providers qui seront instanciés par l'injecteur Nest et qui peuvent être partagés au moins sur ce module.
- **Controllers** l'ensemble des contrôleurs définis dans ce module
- **Imports** la liste des modules importés qui exportent les providers requis dans ce module.
- **Exports** Les providers fournis par ce module et qui peuvent être utilisés par d’autres modules


## Controllers 

Controller is a class annotated with ``@Controller`` that contains a list of actions to accept the client's requests according to the route

### creation
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
- 
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
 
 -> Reduces redandant code : different controllers could use the same service 
 
 *The service must be provided by the parent module or exported by imported modules.*


## Request Lifecycle

 A request flows through middleware to guards, then to interceptors, then to pipes and finally back to interceptors on the return path (as the response is generated).
 
<img src="https://github.com/rihemebh/Nest.js-Cheat-sheet/blob/main/lifecycle.png" width="800" height="400" />

### Middlewares

### Pipes

- Nest calls the pipe just before invoking a method to transfor or evaluate its params

We have 2 different types of pipes 
||Transformation Pipes|Validation Pipes|
|---|---|---|
|Explanation| Transform input data to the desired form (e.g., from string to integer)|Evaluate the input data and throw an exception if invalid  |
|Installation| --|``npm i --save class-validator class-transformer``|
|Where ?|Add it to the property that we want to pipe (Body , Param , Query ...)|Annotate properties|
|Activation|auto |Gloabally : ``app.useGlobalPipes(new ValidationPipe({transform: true, whitelist: true}))`` <br/> In a specific route: ``@UsePipes(PipeClass1, PipeClass2,…)``|
|Example of use |1. with dependecy injection <br/> ``@Param('id', ParseIntPipe)`` <br/>2. with class instantiation ( where we could customize the error message ) <br/>``@Param('id', new ParseIntPipe({errorHttpStatusCode: HttpStatus.NOT_ACCEPTABLE})`` <br/>|1. specify the error message with a simple string : ``@MinLength(20, {message: "$property should has at least $constraint1 characters "})`` <br/> ``title: string;`` <br/>1. specify the error messages with a function :``@MinLength(20, {message: (validationData: ValidationArguments) => { return `the size of ${validationData.property} ${validationData.value} is too small, you should at least have  ${validationData.constraints[0]} characters `}`` |

#### Mapped Type
``@nestjs/mapped-types``

- **PartialType** : Returns the targeted class by setting all the fields to Optional.

- **PickType** : allows you to create a new type (a class) by choosing one set of fields of an existing class.

- **OmitType** : allows you to create a new type (a class) by removing a set of fields of an existing class.

- **IntersectionType** :  allows you to create a new type (a class) fr designated fields that exist in two types.
#### Custom pipes
We have some Built-in pipes exported from the ``@nestjs/common`` package but we can also create our own ones :

- A pipe is a class that implements the ``PipeTransform,`` interface. This interface asks you to implement the ``transform()`` method, this method takes the value to be transformed as a parameter and metadata.
- MetaData : 
  - type: indicates the type of the argument which can be a body with @Body, a queryParam with @Query, a parameter with @Param.
  - metatype: indicates the type of the parameter, for example String.
  - data: the data passed to the decorator
<
### Interceptors 
## Filters
## Configuration Variables

