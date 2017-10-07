# Five times the same app #

Choosing a technology to create a simple web-application can be really tough. Many tools are at our disposal and each of them seems to promise us the Holy Grail. Let's try some of them to get a better idea.

Through this article, we are going to overview different frameworks and libraries by creating the same To-Do List app five times. We are going to try _Angular_, _Aurelia_, _Ember.js_, _React_ and _Polymer 2_. We are not going to focus on _TDD_ or how to write tests. We are just going to create a really simple basic app to see how each of these tools work.

## Angular
_Angular_ is a development platform for building mobile and desktop web applications using _Typescript/JavaScript_.

### Setting up a project

#### Installing Angular CLI

First, we need to install _Angular CLI_. We can do it like so:

	npm install -g @angular/cli

_Installing Anguar CLI with npm_

#### Generating the project
We are now going to generate a new project:

	ng new todo
	cd todo

_Generating the project_

We now have to install the Node modules and run our project:

	npm install
	ng serve

_Installing dependencies and running the server_

Now our app is available at _localhost:4200_. We are going to create a class for our tasks and a service:

	ng g class task
	ng g service task
	
_Creating our class and our service_

### Let's code
For the sake of this example, we are also going to create mock data.

	import { Task } from './task';

	export const TASKS: Task[] = [
	  { id: 1, title: 'Take out the trash', status: false },
	  { id: 2, title: 'Fix the roof', status: false },
	  { id: 3, title: 'Clean the house', status: false }
	];

_mock-tasks.ts file_

Now, let's define first our _Task class_:

	export class Task {
		id: number;
		title: string;
		status: boolean;

		constructor(id?: number, title?: string, status?: boolean) {
			this.id = id;
			this.title = title || '';
			this.status = status || false;
		}

	}

_task.ts file_

Now we can create our service:

	import { Injectable } from '@angular/core';

	import { Task } from './task';
	import { TASKS } from './mock-tasks';

	@Injectable()
	export class TaskService  {

	  tasks: Task[] = TASKS;

	  constructor() { }

	  getTasks(): Promise<Task[]> {
		return Promise.resolve(this.tasks);
	  }

	  add(title: string): Promise<Task> {
		return new Promise(() => {
			const newTask = new Task(this.tasks.length + 1, title, false);
			this.tasks.push(newTask);
			return newTask;
		  }
		);
	  }

	  delete(task: Task): Promise<number> {
		return new Promise(() => {
			const index = this.tasks.findIndex((obj => obj.id === task.id));
			this.tasks.splice(index, 1);
			return task.id;
		  }
		);
	  }

	  changeStatus(task: Task): Promise<Task> {
		return new Promise(() => {
			task.status = true;
			return task;
		  }
		);
	  }

	}

_task.service.ts file_

Now that our main class and our service are ready, we can set our _app.component.ts_ like so:

	import { Component, OnInit } from '@angular/core';

	import { Task } from './task';
	import { TaskService } from './task.service';

	@Component({
	  selector: 'app-root',
	  templateUrl: './app.component.html',
	  styleUrls: ['./app.component.css'],
	  providers: [TaskService]
	})

	export class AppComponent implements OnInit {

	  title = 'To-do List';
	  tasks: Task[];

	  constructor(private taskService: TaskService) { }

	  ngOnInit(): void {
		this.getTasks();
	  }

	  getTasks(): void {
		this.taskService.getTasks().then(tasks => this.tasks = tasks);
	  }

	  addTask(title: string): void {
		title = title.trim();
		if (!title) { return; }
		this.taskService.add(title)
		  .then(task => {
			this.tasks.push(task);
		  });
	  }

	  deleteTask(task: Task): void {
		this.taskService.delete(task)
		  .then(() => {
			const index = this.findTaskIndex(task.id);
			this.tasks.splice(index, 1);
		  });
	  }

	  changeStatus(task: Task): void {
		this.taskService.changeStatus(task)
		  .then(updatedTask => {
			const index = this.findTaskIndex(task.id);
			this.tasks[index] = updatedTask;
		  });
	  }

	  findTaskIndex(id: number): number {
		return this.tasks.findIndex((obj => obj.id === id));
	  }

	}

_app.component.ts.file_

And finally, we can define our component's template:

	<h1>{{title}}</h1>

	<div>
	  <label>New task:</label> <input #taskTitle />
	  <button (click)="addTask(taskTitle.value); taskTitle.value=''">
		Add
	  </button>
	</div>

	<h2>Tasks</h2>
	<ul class="tasks">
	  <li *ngFor="let task of tasks">
		{{task.id}} - {{task.title}}

		<div *ngIf="task.status; else notDoneBlock">Done</div>
		<ng-template #notDoneBlock>
		  <p>Not done<br />
		  <button (click)="changeStatus(task)">Done!</button></p>
		</ng-template>

		<p><button (click)="deleteTask(task)">Delete</button></p>
	  </li>
	</ul>

_app.component.html_

### Checkpoint
If we go to _localhost:4200_, we can note that we have a really basic app where we can see some default tasks, create new ones and delete some others. We can argue that our code isn't good enough, that there is no data persistence or that the tests are missing. But that is not the point.

With this simple project, we saw how we can initialize an _Angular_ project and how it is structured. We can also see that with just a few lines of code written with _TypeScript_, we are able to generate something very quickly. We now have a better idea of how much time we need to set up a basic project and how we can go further.

### Building for production
We can build our project for production like so:

	ng build -prod --base-href ./
	
_Building for production_

This last command will generate a _bundle_ for us.

### Some numbers
#### GitHub

**Angular:** 481 contributors; 144 releases; 8147 commits; 1551 issues

**Angular CLI:** 270 contributors; 100 releases; 1698 commits; 419 issues

#### Stack Overflow

**Angular:** 61654 tagged questions

## Aurelia
_Aurelia_ is a _JavaScript_ client framework for mobile, desktop and web leveraging simple conventions.

### Setting up a project

#### Installing Aurelia CLI
First, we need to install _Aurelia CLI_. We can do it like so:

	npm install aurelia-cli -g

_Installing Aurelia CLI_

#### Generating the project

We are now going to generate a new project:

	au new todo
	cd todo

_Generating the project_

A list of questions will be presented to us. For the sake of this example, we will select default values. We can now run the project that will be available at _localhost:9000_ with the following command:

	au run --watch
	
_Running the server_

### Let's code
For the sake of this example, we are also going to create mock data.

	export const TASKS = [
	  { id: 1, title: 'Take out the trash', status: false },
	  { id: 2, title: 'Fix the roof', status: false },
	  { id: 3, title: 'Clean the house', status: false }
	];

_mock-tasks.js file_

Now, we are going to create a _task.js_ file:

	export class Task {
	  constructor(id, title) {
		this.id = id;
		this.title = title;
		this.status = false;
	  }
	}

_task.js file_

We also need to create a service:

	import { Task } from './task';
	import { TASKS } from './mock-tasks';

	export class TaskService  {

	  tasks = TASKS;

	  constructor() { }

	  getTasks() {
		return Promise.resolve(this.tasks);
	  }

	  add(title) {
		return new Promise(() => {
			const newTask = new Task(this.tasks.length + 1, title, false);
			this.tasks.push(newTask);
			return newTask;
		  }
		);
	  }

	  delete(task) {
		return new Promise(() => {
			const index = this.tasks.findIndex((obj => obj.id === task.id));
			this.tasks.splice(index, 1);
			return task.id;
		  }
		);
	  }

	  changeStatus(task) {
		return new Promise(() => {
			task.status = true;
			return task;
		  }
		);
	  }

	}

_task-service.js file_

Now we can fill our _app.js_ file:

	import { Task } from './task';
	import { TaskService } from './task-service';
	import { inject } from 'aurelia-framework';

	@inject(TaskService)
	export class App {

	  constructor(taskService) {
		this.taskService = taskService;
		this.title = 'To-do List';
		this.tasks = [];
		this.taskTitle = '';
	  }

	  created() {
		this.taskService.getTasks().then(tasks => this.tasks = tasks);
	  }

	  addTask() {
			const title = this.taskTitle.trim();
		if (!title) { return; }
		this.taskService.add(title)
		  .then(task => {
					this.tasks.push(task);
					this.taskTitle = '';
		});
	  }

	  deleteTask(task) {
		this.taskService.delete(task)
		  .then(() => {
			const index = this.findTaskIndex(task.id);
			this.tasks.splice(index, 1);
		  });
	  }

	  changeStatus(task) {
		this.taskService.changeStatus(task)
		  .then(updatedTask => {
			const index = this.findTaskIndex(task.id);
			this.tasks[index] = updatedTask;
		  });
	  }

	  findTaskIndex(id) {
		return this.tasks.findIndex((obj => obj.id === id));
	  }

	}

_app.js file_

Finally, we have to edit our template _app.html_ like so:

	<template>
	  <h1>${title}</h1>

	  <form submit.trigger="addTask()">
		<input type="text" value.bind="taskTitle">
		<button type="submit">Add</button>
	  </form>

	  <ul>
		<li repeat.for="task of tasks">
		  ${task.id} - ${task.title}
		  <div show.bind="!task.status">
			<p>Not done<br />
			<button click.trigger="changeStatus(task)">Done!</button></p>
		  </div>
		  <div show.bind="task.status">
			<p><button click.trigger="deleteTask(task)">Delete</button></p>
		  </div>
		</li>    
	  </ul>

	</template>

_app.html file_

### Checkpoint

If we go to _localhost:9000_, we can note that we have a really basic app where we can see some default tasks, create new ones and delete some others. In fact, our app is pretty similar to the one we created with _Angular_.

With this simple project, we saw how we can initialize an _Aurelia_ project and how it is structured. We now have a better idea of how much time we need to set up a basic project and how we can go further. In fact, it was pretty easy because, even here if we don't use _TypeScript_ but only _JavaScript_, our code is quite similar to our previous app. 

### Building for production

We can build our project for production like so:

	au build --env prod
	
_Building for production_

This last command will generate a _bundle_ for us.

### Some numbers

#### GitHub

**Aurelia:** 86 contributors; 85 releases; 572 commits; 54 issues

**Aurelia CLI:** 53 contributors; 38 releases; 834 commits; 93 issues

#### Stack Overflow

**Aurelia:** 2370 tagged questions

## Ember.js

_Ember.js_ is an open-source _JavaScript_ web framework, based on the _Model–view–viewmodel_ (MVVM) pattern. Although primarily considered a framework for the web, it is also possible to build desktop and mobile applications in _Ember_.

### Setting up a project

#### Installing Ember CLI

First, we need to install _Ember CLI_. We can do it like so:

	npm install -g ember-cli

_Installing Ember CLI_

#### Generating the project

We are now going to generate a new project and run it:

	ember new todo
	cd todo
	ember serve

_Generating the project and running the server_

Our app will be available at _localhost:4200_. Now we are going to generate what we need:

	ember g model task title:string status:boolean
	ember g route tasks --path '/'
	ember g route tasks/index
	ember g http-mock tasks
	ember g adapter application
	ember generate serializer application
	ember g controller tasks

_Generating model, routes, adapter, serializer and controller_

### Let's code

Let's start by creating some fake data. We are going to place what we need into the file _server/mocks/tasks.js_:

	...
	tasksRouter.get('/', function(req, res) {
		res.send({
		  'tasks': [
			{ taskId: 1, title: 'Take out the trash', status: false },
			{ taskId: 2, title: 'Fix the roof', status: false },
			{ taskId: 3, title: 'Clean the house', status: false }
		  ]
		});
	});
	...

_server/mocks/tasks.js files_

Our _Task model_ should look like so:

	import DS from 'ember-data';

	export default DS.Model.extend({
		title: DS.attr('string'),
		isCompleted: DS.attr('boolean')
	});

_models/tasks.js file_

In our _controllers/tasks.js_ file, we are going to put the following code:

	import Ember from 'ember';

	export default Ember.Controller.extend({

	  actions: {

		addTask() {
			const title = this.get('newTitle');
			if (!title.trim()) { return; }
			const id = this.get('model.length') + 1 + new Date().getTime();
			const task = this.store.createRecord('task', {
				id,
				title,
				status: false
			});
			this.set('newTitle', '');
			task.save();
		},

		deleteTask(id) {
		  this.store.findRecord('task', id).then((task) => task.destroyRecord());
		},

		changeStatus(id){
		  this.store.findRecord('task', id).then((task) => task.set('status', true));
		}

	  }
		
	});

_controllers/tasks.js file_

Now we have to define our _Routes_. First, in our _routes/tasks.js_ file, let's place the following code:

	import Ember from 'ember';

	export default Ember.Route.extend({

		model: function() {
		return this.store.findAll('task');
		}

	});

_routes/tasks.js file_

Our _router.js_ file should also look like this:

	import Ember from 'ember';
	import config from './config/environment';

	const Router = Ember.Router.extend({
	  location: config.locationType,
	  rootURL: config.rootURL
	});

	Router.map(function() {
	  this.route('tasks', { path: '/' }, function() {});
	});

	export default Router;

_router.js file_

Now, we have to define our _Adapter_ and our _Serializer_. Let's do it like so:

	import DS from 'ember-data';

	export default DS.RESTAdapter.extend({
	  contentType: 'application/json',
	  dataType: 'json',
	  namespace: 'api'
	});

_adapters/application.js file_

	import DS from 'ember-data';

	export default DS.RESTSerializer.extend({
	  primaryKey: 'taskId'
	});

_serializers/application.js file_

We are almost done. We just have to set our templates.

	{{outlet}}

_templates/application.hbs_

	<h1>To-do List</h1>

	<div>
	  <label>New task:</label> {{input type="text" value=newTitle}}
	  <button {{action "addTask"}}>Add</button>
	</div>

	<h2>Tasks</h2>
	<ul class="tasks">
	  {{#each model as |task|}}
		<li>
		  {{task.id}} - {{task.title}}
		  {{#if task.status}}
			<p><button {{action "deleteTask" task.id}}>Delete</button></p>
		  {{else}}
			<p>Not done<br />
			<button {{action "changeStatus" task.id}}>Done!</button></p>
		  {{/if}}
		</li>
	  {{/each}}
	</ul>

_templates/tasks.hbs_

### Checkpoint

If we go to _localhost:4200_, we can note that we have a really basic app where we can see some default tasks, create new ones and delete some others.

With this simple project, we saw how we can initialize an _Ember.js_ project and how it is structured. We now have a better idea of how much time we need to set up a basic project and how we can go further. We can also see that, even if it has common concepts with the other frameworks, Ember.js is significantly different and we need to take another approach.

### Building for production

We can build our project for production like so:

	ember build
	
_Building for production_

This last command will generate a _bundle_ for us.

### Some numbers

#### GitHub

**Ember.js:** 673 contributors; 255 releases; 14762 commits; 242 issues

**Ember CLI:** 369 contributors; 140 releases; 7546 commits; 151 issues

#### Stack Overflow

**Ember.js:** 21528 tagged questions

## React

_React_ is a _JavaScript_ library for building user interfaces. _React_ allows us to create large web-applications that use data that can change over time, without reloading the page. It aims primarily to provide speed, simplicity and scalability. React processes only user interfaces in applications. This corresponds to _View_ in the _Model-View-Controller_ (MVC) template, and can be used in combination with other _JavaScript_ libraries or frameworks in _MVC_.

### Setting up a project

#### Installing Create React App

To start our project, we need to install _Create React App_. We can do it like so:

	npm install -g create-react-app
	
_Installing Create React App_

#### Generating the project

We are now going to generate a new project and run it:

	create-react-app todo
	cd todo
	npm start

_Generating the project and running the server_

Our project is now available at _localhost:3000_.

### Let's code

For the sake of this example, we are also going to create mock data. Let's create a file _mock/Tasks.js_:

	export const TASKS = [
	  { id: 1, title: 'Take out the trash', status: false },
	  { id: 2, title: 'Fix the roof', status: false },
	  { id: 3, title: 'Clean the house', status: false }
	];

	export default TASKS;
	
_mock/Tasks.js file_

Now we have to define a model for _Task_ and an _Interface_ like so:

	function guidGenerator() {
	  function S4() {
		return Math.floor((1 + Math.random()) * 0x10000)
		  .toString(16)
		  .substring(1);
	  }
	  return S4()+S4()+'-'+S4()+'-'+S4()+'-'+S4()+'-'+S4()+S4()+S4();
	}

	export default class Task {

		constructor(title, status, id) {
		  this.title = title || '';
		  this.status = status || false;
		  this.id = id || guidGenerator();
		}
		
	}

_lib/Task.js file_

	import Task from './Task';
	import TASKS from '../mock/Tasks'

	import { findIndex } from 'lodash';

	export default class TaskDataInterface {
	  constructor() {
		this.tasks = TASKS;
	  }

	  addTask(title) {
		  const newTask = new Task(title);
		  this.tasks.push(newTask);
		  return newTask;
	  }

	  changeStatus(taskId) {
		const taskIndex = findIndex(this.tasks, (task) => task.id === taskId);
		if (taskIndex > -1) {
		  this.tasks[taskIndex].status = !this.tasks[taskIndex].status;
		}
	  }

	  deleteTask(taskId) {
		const taskIndex = findIndex(this.tasks, (task) => task.id === taskId);
		if (taskIndex > -1) {
		  this.tasks.splice(taskIndex, 1);
		}
	  }

	  getAllTasks() {
		return this.tasks.map(task => task);
	  }
	}

_lib/TaskDataInterface.js file_

We can now create two components: one for the list itself and another for each item.

	import React from 'react';
	import SingleTask from './SingleTask'

	export default class TasksList extends React.Component {
	  render() {
		return (
		  <div>

			<ul>
			  {this.props.tasks.map(
				(task) =>
				  <SingleTask
					key={task.id}
					taskId={task.id}
					title={task.title}
					status={task.status}
					changeStatus={this.props.changeStatus}
					deleteTask={this.props.deleteTask}
				  />
			  )}
			</ul>
			  
		  </div>
		);
	  }
	}

_components/TasksList.js file_

import React from 'react';

	export default class TaskTask extends React.Component {
	  render() {
		return (
		  <li>

			{this.props.taskId} - {this.props.title}

			{this.props.status ? (

			  <p><button
				onClick={() => this.props.deleteTask(this.props.taskId)}>
				  Delete
			  </button></p>

			) : (

			  <p>Not done<br />
			  <button
				onClick={() => this.props.changeStatus(this.props.taskId)}>
				  Done!
			  </button></p>

			)}
			
		  </li>
		);
	  }
	}

_components/SingleTask.js file_

We can now edit the _App.js_ and _index.js_ files like so:

	import React, { Component } from 'react';
	import './App.css';

	import TasksList from './components/TasksList';

	class App extends Component {

	  constructor(props) {
		super(props);
		this.state = {
		  tasks: this.props.dataInterface.getAllTasks()
		};
	  }

	  addTask = () => {
		if (this._taskInputField.value) {
		  this.props.dataInterface.addTask(this._taskInputField.value);
		  this.setState({tasks: this.props.dataInterface.getAllTasks()});
		  this._taskInputField.value = '';
		}
	  }

	  changeStatus = taskId => {
		this.props.dataInterface.changeStatus(taskId);
		this.setState({tasks: this.props.dataInterface.getAllTasks()});
	  }

	  deleteTask = taskId => {
		this.props.dataInterface.deleteTask(taskId);
		this.setState({tasks: this.props.dataInterface.getAllTasks()});
	  }

	  render() {

		return (
		  <div>
			<h1>To-do List</h1>

			<div>
			  <label>New task: </label>
			  <input
				type="text"
				ref={(c => this._taskInputField = c)}
			  />
			  <button onClick={this.addTask}>Add</button>
			</div>

			<h2>Tasks</h2>
			<TasksList
			  tasks={this.state.tasks}
			  changeStatus={this.changeStatus}
			  deleteTask={this.deleteTask}
			/>

		  </div>
		);
	  }

	}

	export default App;

_App.js file_

	import React from 'react';
	import ReactDOM from 'react-dom';
	import './index.css';

	import TaskDataInterface from './lib/TaskDataInterface';
	import App from './App';

	import registerServiceWorker from './registerServiceWorker';

	const taskDataInterface = new TaskDataInterface();
	ReactDOM.render(<App dataInterface={taskDataInterface} />, document.getElementById('root'));
	registerServiceWorker();

_index.js file_

### Checkpoint

If we go to _localhost:3000_, we can note that we have a really basic app where we can see some default tasks, create new ones and delete some others.

With this simple project, we saw how we can initialize a _React_ project and how it is structured. We now have a better idea of how much time we need to set up a basic project and how we can go further. We can also see, even if it has common concepts with the other frameworks, how React differs and how, with just a few lines of code, we can quickly achieve something.

### Building for production

We can build our project for production like so:

	npm run build
	
_Building for production_

This last command will generate a _bundle_ for us.

### Some numbers

#### GitHub

**React:** 1039 contributors; 63 releases; 8811 commits; 580 issues

**Create React App:** 346 contributors; 127 releases; 1143 commits; 216 issues

#### Stack Overflow

**React:** 51656 tagged questions

## Polymer 2
_Polymer_ is an open-source _JavaScript_ library for building web applications using web components. _Polymer_ lets us build encapsulated, reusable elements that work just like standard _HTML_ elements, to use in building web applications.

### Setting up a project

#### Installing Polymer CLI

To start our project, we need to install _Polymer CLI_. We can do it like so:

	npm install -g polymer-cli

_Installing Polymer CLI_

#### Generating the project

We are now going to generate a new project and run it:

	cd todo
	polymer init
	polymer serve

_Generating the project and running the server_

During the process, we are going to be asked a few questions. In particular, we have to choose a template. We are going to select _polymer-2-application_. If everything is alright, our project will be available at _localhost:8081_.

### Let's code

For the sake of this example, we are also going to create mock data. Let's create a file _mock/tasks.js_:

	const TASKS = [
	  { id: 1, title: 'Take out the trash', status: false },
	  { id: 2, title: 'Fix the roof', status: false },
	  { id: 3, title: 'Clean the house', status: false }
	];

_mock/tasks.js file_

Now we are going to fill our _todo-app.html_ file like so:

	<link rel="import" href="../../bower_components/polymer/polymer.html">
	<link rel="import" href="../../bower_components/polymer/polymer-element.html">

	<script src="mock/tasks.js"></script>

	<dom-module id="todo-app">
	  <template>
		<style>
		  :host {
			display: block;
		  }
		</style>
		<h1>[[title]]</h1>

		<div>
		  <label>New task:</label> <input type="text" id="newTask" value="{{term::input}}" />
		  <button on-click="addTask">
			Add
		  </button>
		</div>

		<h2>Tasks</h2>

		<ul>
		  <template id="list" is="dom-repeat" items="[[tasks]]">
			<li>
			  {{item.id}} - {{item.title}}

			  <template is="dom-if" if="{{item.status}}">
				<p><button on-click="deleteTask">Delete</button></p>
			  </template>

			  <template is="dom-if" if="{{!item.status}}">
				<p>Not done<br />
				<button on-click="changeStatus">Done!</button></p>
			  </template>
			  
			</li>
		  </template>
		</ul>

	  </template>

	  <script>

		class TodoApp extends Polymer.Element {

		  static get is() { return 'todo-app'; }

		  static get properties() {

			return {

			  title: {
				type: String,
				value: 'To-do List'
			  },

			  term: {
				type: String,
				value: ''
			  },

			  tasks: {
				type: Array,
				value: []
			  }

			};

		  }

		  ready() {
			super.ready();
			this.set('tasks', TASKS);
		  }

		  addTask() {
			this.term = this.term.trim();
			if (!this.term) { return; }
			this.push('tasks', {id: this.guidGenerator(), title: this.term, status: false});
			this.term = '';
		  }

		  deleteTask(e) {
			const index = this.findTaskIndex(e.model.item.id);
			this.splice('tasks', index, 1);
		  }

		  changeStatus(e) {
			const index = this.findTaskIndex(e.model.item.id);
			const oldTask = this.tasks[index];
			this.set(`tasks.${index}.status`, true);
		  }

		  findTaskIndex(id) {
			return this.tasks.findIndex((obj => obj.id === id));
		  }

		  guidGenerator() {
			function S4() {
			  return Math.floor((1 + Math.random()) * 0x10000)
				.toString(16)
				.substring(1);
			}
			return S4()+S4()+'-'+S4()+'-'+S4()+'-'+S4()+'-'+S4()+S4()+S4();
		  }

		}

		window.customElements.define(TodoApp.is, TodoApp);
	  </script>
	</dom-module>

_todo-app.html file_

### Checkpoint

If we go to _localhost:8081_, we can note that we have a really basic app where we can see some default tasks, create new ones and delete some others.

With this simple project, we saw how we can initialize a Polymer project and how it is structured. We now have a better idea of how much time we need to set up a basic project and how we can go further. We can also see that, even if it has common concepts with the other frameworks, _Polymer_ has a different philosophy.

### Building for production

We can build our project for production like so:

	polymer build
	
_Building for production_

This last command will generate a _bundle_ for us. Most optimizations are disabled by default. To make sure the correct build enhancements are always used, we have to provide a set of build configurations via the "_builds_" field of our _polymer.json_ file:

	"builds": [{
	  "bundle": true,
	  "js": {"minify": true},
	  "css": {"minify": true},
	  "html": {"minify": true}
	}]

_polymer.json file_

### Some numbers

#### GitHub

**Polymer:** 119 contributors; 115 releases; 5433 commits; 668 issues

**Polymer CLI:** 34 contributors; 46 releases; 576 commits; 164 issues;

#### Stack Overflow

**Polymer:** 7105 tagged questions

### Conclusion
How can we conclude this article and can we really do it? That is a good question. Through our different tryouts we played with different tools that share same concepts, but have a different philosophy. Unfortunately for us, there is no magic formula that will tell us which one to pick.

It is sad, but true. We have to try the tools that we think could fit to our needs, then compare them on various points. Aside of performance and ease of use, looking at the community that may have grown around a specific tool can be a relevant point.
