# Nest-first-project

### Instalation 

    npm i -g @nestjs/cli

### New Project 
    nest new NomProjet. 
    
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
