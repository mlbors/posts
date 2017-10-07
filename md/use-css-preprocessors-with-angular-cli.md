## [Use CSS preprocessors with Angular CLI](https://mlbors.tumblr.com/post/159498646630/use-css-preprocessors-with-angular-cli)

_Angular CLI_ supports all major CSS preprocessors. Here is how you can use them easily.

If you have an existing project, you can use the following command to set the default style:
    
    
    ng set defaults.styleExt scss

If you want to generate a new project, you can set the extension that you want like so:
    
    
    ng new my-project --style=sass

Now, in your component, you can simply do something like that:
    
    
    @Component({
      selector: 'app-root',
      templateUrl: './app.component.html',
      styleUrls: ['./app.component.scss']
    })
    export class AppComponent {
      title = 'This app is awesome!';
    }
