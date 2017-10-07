# Quickly start an Angular 2 project #

In this small post, I will tell you about how you can initialize a new Angular 2 project.

First of all, install the _Angular CLI_ with _npm_:
    
    
    npm install -g @angular/cli

Then, navigate to the directory where you’d like to start you project and create the project with:
    
    
    ng new MY_AWESOME_PROJECT

Finally, enter in the folder and start the server:
    
    
    cd MY_AWESOME_PROJECT
    ng serve
    

You can now do some helpful things like create a component:
    
    
    ng generate component my-component

If you want to build your fancy project to run it everywhere you want, do something like this:
    
    
    ng build -prod --base-href ./
