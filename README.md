# Nest Cheat Sheet

- [1. Let's start](#1-introduction)
  - [1.1 Instalation](#11document-purpose)
  - [1.2 New Project ](#12product-scope)
  - [1.3 Launch Project](#13intended-audience-and-document-overview)

- [2. Modules](#2overall-description)
## Let's Start

### Instalation 
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

## Middlewares

## Providers 

## Pipes
## Filters
## Interceptors 
## Configuration Variables

