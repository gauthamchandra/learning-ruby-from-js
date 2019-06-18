# Rails introduction

Rails is a great MVC framework written on top of Ruby.

This is not an exhaustive guide. Not even close. For full docs, go [here](http://api.rubyonrails.org/)

Right now, this guide is wholly unfinished.

## Basic Terminal Commands

All commands can be run with ```-h``` or ```---help``` to list more information.

For a full guide, see [Rails Official Commandline Guide](http://guides.rubyonrails.org/command_line.html)

### Rails commands

To create a new app, use the command

```rails new <app-name>```

To open up the Ruby WEBrick server to host your rails app, run:

```rails server```

To generate specific scaffolding or use specific generators to get models, views, routes, etc., run:

```rails generate <options>```

To interact with the rails app from command line, you can use the rails console via:

```rails console```


#### Generating Models

To generate a new model, the command is:

```rails generate model <model-name-capitalized> <column-name>:<data-type> ...```

This generates the model file as well as the database table with the specified columns.

An example would be:

```rails generate model Article title:string text:text```

The above statement is telling rails to create:

* an ```app/models/article.rb``` file containing a model ```Article```
* a ```db/migate/<timestamp>_create_articles.rb``` file needed for making changes, updates, etc. to the DB.

When the database migration file is created, it will look something like this:

```
class CreateArticles < ActiveRecord::Migration
  def change
    create_table :articles do |t|
      t.string :title
      t.text :text
 
      t.timestamps null: false
    end
  end
end
```

This file is used for database migrations and updates and is totally reversible. Rails prefixes the file with a timestamp to make sure they are processed in the order they are created.

Now to create the table, run the command:

```rake db:migrate```


### Rake commands

What is Rake? Rake is simply a build tool. It can be thought of as a Makefile with Ruby code. Rake is a superset of Ruby. It allows developers to define build tasks via the `task` keyword.

Rake automatically picks up on any 'task' defined in a file ending in the `.rake` extension inside the current subdirectory. 

To get a list of all tasks

`rake -T`

Rails relies heavily on Rake by adding some default tasks. 

One of which is:

```rake routes```

This is used to get a list of all routes in the app.

Example result of command:

```
$ rake routes
       Prefix Verb   URI Pattern                  Controller#Action
     articles GET    /articles(.:format)          articles#index
              POST   /articles(.:format)          articles#create
  new_article GET    /articles/new(.:format)      articles#new
 edit_article GET    /articles/:id/edit(.:format) articles#edit
      article GET    /articles/:id(.:format)      articles#show
              PATCH  /articles/:id(.:format)      articles#update
              PUT    /articles/:id(.:format)      articles#update
              DELETE /articles/:id(.:format)      articles#destroy
welcome_index GET    /welcome/index(.:format)     welcome#index
         root GET    /                            welcome#index
```


## Basic Structure of a Rails app

<table>
	<thead>
		<tr>
			<td>Folder/File</td>
			<td>Purpose</td>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>app/</td>
			<td>Contains the models, controllers, helpers, views, assets and mailers for your application. Our focus will be on this folder in this guide.</td>
		</tr>
		<tr>
			<td>bin/</td>
			<td>Includes the rails script will start up your app and may contain other types of scripts used by you to deploy/ run your application.</td>
		</tr>
		<tr>
			<td>config/</td>
			<td>Configure your application's database, routes, and more. This is will covered in detail later on in this book.</td>
		</tr>
		<tr>
			<td>config.ru</td>
			<td>Contains the Rack configuration for servers that are Rack-based and are used to start an app.</td>
		</tr>
		<tr>
			<td>db/</td>
			<td>Includes your current database schema along with the database migrations.</td>
		</tr>				
		<tr>
			<td>Gemfile,<br>Gemfile.lock</td>
			<td>Using these files, you can specify the gem dependencies that your application needs. The Bundler gem uses these files.</td>
		</tr>
		<tr>
			<td>lib/</td>
			<td>Contains extended modules for applications</td>
		</tr>				
		<tr>
			<td>log/</td>
			<td>Contains application log files.</td>
		</tr>
		<tr>
			<td>public/</td>
			<td>The only folder that can be viewed by the world. Includes compiled assets and static files.</td>
		</tr>
		<tr>
			<td>Rakefile</td>
			<td>This particular file locates & loads tasks that are able to run from the command line. Throughout components of Rails, the task definitions are defined. Instead of making any changes to the Rakefile, you should consider adding your own tasks by adding files to the "lib/tasks" directory of your app.</td>
		</tr>				
		<tr>
			<td>test/</td>
			<td>Contains fixtures, tests, and numerous other types of test apparatus</td>
		</tr>		
		<tr>
			<td>tmp/</td>
			<td>Temporary files such as pid, cache, and session files.</td>
		</tr>				
		<tr>
			<td>vendor/</td>
			<td>A folder that contains all third-party code. This includes vendored gems in a typical Rails application.</td>
		</tr>	
	</tbody>
</table>


## Routing

### Configuring Routes

A list of all routes in an application can be found in ```config/routes.rb```

To specify a general route, you can just use:

```get '<controller>/<action>'``` 

If it needs to map to a specific controller and action then we can use:

```get '<route-name>' '<controller>#<action>'```

To specify the root (i.e default controller and action for route: ```/```), then use:

```root '<controller>#<action>```


### Configuring resources

For data to be queried back and forth by your application to serve the user, there need to be REST endpoints set up for the resources.

We need to do 2 things:

1. Define the resource in ```config/routes.rb```
2. Check to see if exists via the ```rake routes``` command


To define the resource, we add an entry and use the following notation:

```resources :<resource-name>```

For Example:

```
Rails.application.routes.draw do
	#Define a resource called articles
	resources :articles 
	root 'welcome#index'
end
```

This will have rails setup the appropriate endpoints when the app is run next time. 

Running ```rake routes``` yields:

```
$ rake routes
       Prefix Verb   URI Pattern                  Controller#Action
     articles GET    /articles(.:format)          articles#index
              POST   /articles(.:format)          articles#create
  new_article GET    /articles/new(.:format)      articles#new
 edit_article GET    /articles/:id/edit(.:format) articles#edit
      article GET    /articles/:id(.:format)      articles#show
              PATCH  /articles/:id(.:format)      articles#update
              PUT    /articles/:id(.:format)      articles#update
              DELETE /articles/:id(.:format)      articles#destroy
welcome_index GET    /welcome/index(.:format)     welcome#index
         root GET    /                            welcome#index
```




## Naming Convention

Given the database table name ```tweets``` with the data below:

<table>
	<thead>
		<tr>
			<td>id</td>
			<td>status</td>
			<td>name</td>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>1</td>
			<td>Where can I get a good bite to eat?</td>
			<td>Ash</td>
		</tr>
		<tr>
			<td>2</td>
			<td>I just ate some serious brains</td>
			<td>Jim</td>
		</tr>
	</tbody>
</table>

To access it, the naming convention would be an uppercase and singular class```Tweet.find(1)```.

In other words, the models in rails use a singular name while the actual corresponding databases tables use a plural name. 



## Glossary

* ```DSL``` - Domain Specific Language
* ```resource```- an object/collection of objects that can be queried by the user via REST.
* ```controller action``` - the method in the controller that does some operation
* ```migrations``` - ruby classes designed to make it simple to create and modify database tables. They are used for database migrations and updates and all changes they do are reversible.

