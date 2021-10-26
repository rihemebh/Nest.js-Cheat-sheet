# Nest Cheat Sheet

- [1. Let's start](#lets-start)
  - [1.1 Installation](#installation)
  - [1.2 New Project ](#new-project)
  - [1.3 Launch Project](#launch-project)

- [2. Modules](#2overall-description)
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

### Parameters : 
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

### Annotations 
 |name|description|
 |---|---|
 |@Controller|to annotate a controller|
 |@Global|to make a module visible from all the application |
 
 
## Routes

A route will identify the uri associated to an action.
### Params 

```js 
@Get('uri/:param/:param1?')

```

param 1 : optional
### Annotations 
 |name|description|
 |---|---|
 |``@Post()`` , ``@Get()`` , ``@Delete()``,  ``@Put()`` , ``@Patch()``|Accepts a HTTP request|
 |``@Body()`` | retrieve the POST request body |
 |``@Res()``| response |
 |``@HttpCode(code)``|customize the HTTP code |
 |``@Header()``||
 |``@Param(<name>)``|retirive params from the uri|
 
 
Note : we can make a generique uri 

@Get('test*') : any uri that begins with test

``+`` : 1 ou plus 

``?`` : 0 ou plus 



## DTO

- It is an object that allows to define how the data is sent via the network.

## Services 



## Providers
## Request Lifecycle

 A request flows through middleware to guards, then to interceptors, then to pipes and finally back to interceptors on the return path (as the response is generated).
 
<img src="https://github.com/rihemebh/Nest.js-Cheat-sheet/blob/main/lifecycle.png" width="800" height="400" />
### Middlewares

### Pipes

- Nest calls the pipe just before invoking a method to transfor or evaluate its params
- 
We have 2 different types of pipes 
||Transformation Pipes|Validation Pipes|
|---|---|---|
|Explanation| Transform input data to the desired form (e.g., from string to integer)|Evaluate the input data and throw an exception if invalid  |
|Installation| --|``npm i --save class-validator class-transformer``|
|Where ?|Add it to the property that we want to pipe (Body , Param , Query ...)|Annotate properties|
|Activation|auto |Gloabally : ``app.useGlobalPipes(new ValidationPipe({transform: true, whitelist: true}))`` <br/> In a specific route: ``@UsePipes(PipeClass1, PipeClass2,…)``|
|Example of use |1. with dependecy injection <br/> ``@Param('id', ParseIntPipe)`` <br/>2. with class instantiation ( where we could customize the error message ) <br/>``@Param('id', new ParseIntPipe({errorHttpStatusCode: HttpStatus.NOT_ACCEPTABLE})`` <br/>|``@IsNotEmpty({message: “You must specify a title ”})`` <br/> ``@MinLength(20, {message: “La taille de votre $property $value est courte, la taille minimale de $property est $constraint1"})`` <br/> ``title: string;`` <br/>|


We have some Built-in pipes exported from the ``@nestjs/common`` package but we can also create our own ones :

### Interceptors 
## Filters
## Configuration Variables

