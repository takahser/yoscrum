# yoscrum
scrumboard app created with the yeoman angular-fullstack generator

Link: https://yoscrum.herokuapp.com/task

## intro
I decided to use the great yeoman angular-fullstack generator, whose purpose it is to create MEAN stack applications, using MongoDB, Express, AngularJS, and Node. It's also capable of scaffolding lots of boilerplate code, which saves time and resources at the development.

## architecture

### technologies

- Web Server: [node.js](https://nodejs.org/)
- REST API: [node.js](https://nodejs.org/)
- Frontend Application: [Jade](http://jade-lang.com/), [AngularJs](https://angularjs.org/), [Materialize](http://materializecss.com/)
- Websockets: [socket.io](http://socket.io/)
- Persistence: [mongoDB](https://www.mongodb.org/)
- Testing: [Jasmine](http://jasmine.github.io), [PhantomJs](http://phantomjs.org), [Karma](http://karma-runner.github.io)

### frontent framework

The main reasons I prefered AngularJs over BackboneJs are:

- it provides two-way data binding
    - traditional way without two-way data binding
    ```
    <div id="greet-form">
        <input type="text" class="user-name" />
        <div class="user-name"></div>
    </div>
    <script>
        $('#greet-form input.user-name').on('value', function() { 
            $('#greet-form div.user-name').text('Hello ' + this.val() + '!'); 
        });
    </script>
    ```

    - with two-way data binding and AngularJs
    ```
    <input ng-model="user.name" type="text" />
    Hello {{user.name}}!
    ```

- bigger popularity & community
- cleaner & slimer markup
    - AngularJs
    ```
    <ul> 
        <li ng-repeat="framework in frameworks" title="{{framework.description}}">
          {{framework.name}} 
        </li> 
    </ul>
    ```

    - BackboneJs
    ```
    <ul> 
        <% _.each(frameworks, function(framework) { %> 
            <li title="<%- framework.description %>"> 
                <%- framework.name %> 
            </li> 
        <% }); %> 
    </ul>
    ```

## steps taken

### install yeoman & generator

1) install yeoman
`npm install -g yo`

2) install `angular-fullstack` generator
`npm install -g generator-angular-fullstack`

### scaffold the app

1) Make a new directory, and cd into it:
`mkdir scrumseraya && cd $_`

2) Scaffold the base app
`yo angular-fullstack scrumseraya`

3) Interactively choose the technologies you want to use. I decided to use
- Client
    - Scripts: JavaScript
    - Markup: Jade
    - Stylesheets: CSS
    - Angular Router: ui-router
- Server
    - Database: MongoDB
    - Authentication boilerplate: Yes
    - oAuth integrations: Facebook Twitter Google
    - Socket.io integration: Yes

4) Scaffold the Endpoint
`yo angular-fullstack:endpoint task`

This will create the model and API on the server:
```
server/api/task/index.js
server/api/task/task.spec.js
server/api/task/task.controller.js
server/api/task/task.model.js  (optional)
server/api/task/task.socket.js (optional)
```

5) Scaffold the route
`yo angular-fullstack:route task`

This will create the client files:
```
client/app/task/task.js
client/app/task/task.controller.js
client/app/task/task.controller.spec.js
client/app/task/task.jade
client/app/task/task.css
```

6) Extend the schema by editing the `/server/api/task/task.model.js` file
```
var TaskSchema = new Schema({
  name: String,
  info: String,
  estimation: Number,
  status: String,
  personResponsible: String,
  active: Boolean
});
```

7) Add to the controller on the client `client/app/task/task.controller.js`

- populate `$scope.tasks = [];` array by consuming the `/api/tasks` api
- methods for CRUD functionality

8) Complete by implementing the view `client/app/task/task.jade` and stylesheets

## deployment

1) clone `yoscrum` and `cd` into it:

    git clone https://github.com/takahser/yoscrum && cd yoscrum

2) scaffold a heroku app:

    yo angular-fullstack:heroku

As we are using mongodb, add an addon that provides a mongodb. Instead of the `mongohq` addon mentioned in the documentation we use `mongolab` as it provides a free plan.

3) add the `mongolab` heroku addon that provides mongodb by changing into the `dist/` directory and running

    heroku addons:add mongolab:sandbox

4) change back to the root of the yoscrum application folder and run

    grunt build

to build the app to the `dist/` directory

5) deploy the app using

    grunt buildcontrol:heroku
