# Nest-first-project

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

### Parameters : 
- **Providers** les providers qui seront instanciés par l'injecteur Nest et qui peuvent être partagés au moins sur ce module.
- **Controllers** l'ensemble des contrôleurs définis dans ce module
- **Imports** la liste des modules importés qui exportent les providers requis dans ce module.
- **Exports** Les providers fournis par ce module et qui peuvent être utilisés par d’autres modules


## Controllers 

Controller is a class annotated with ``@Controller`` that contains a list of actions to accept the client's requests according to the route

### creation
```js
nest g mo ModuleName
```

### Annotations 
 |name|description|
 |---|---|
 |@Controller|to annotate a controller|
 |@Global|to make a module visible from all the application |
 
 
## Routes

A route will identify the uri associated to an action.

### Annotations 
 |name|description|
 |---|---|
 |``@Post`` , ``@Get`` , ``@Delete``,  ``@Put`` , ``@Patch``|Accepts a HTTP request|
 |``@Body`` | retrieve the POST request body |
 |||




