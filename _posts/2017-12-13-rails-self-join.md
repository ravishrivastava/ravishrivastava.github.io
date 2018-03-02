---
layout: post
title: "Rails Self join explained"
date: 2017-12-13
---
Here in your migrations/schema, you will add a references column to the model itself.

class CreateUsers < ActiveRecord::Migration[5.0]
  def change
    create_table :users do |t|
      t.references :parent, index: true
      t.timestamps
    end
  end
end

class User < ApplicationRecord
  has_many :children ,class_name: "User", foreign_key: "parent_id"
  has_many :grandchildren, :through => :children, :source => :children
  belongs_to :parent, class_name: "User"
end







1. Parent-id ,self join
2.10 Self Joins
Ans : In designing a data model, you will sometimes find a model that should have a relation to itself. For example, you may want to store all employees in a single database model, but be able to trace relationships such as between manager and subordinates. This situation can be modeled with self-joining associations:

class Employee < ApplicationRecord
  has_many :subordinates, class_name: "Employee",
                          foreign_key: "manager_id"
 
  belongs_to :manager, class_name: "Employee"
end

With this setup, you can retrieve @employee.subordinates and @employee.manager.

In your migrations/schema, you will add a references column to the model itself.

class CreateEmployees < ActiveRecord::Migration[5.0]
  def change
    create_table :employees do |t|
      t.references :manager, index: true
      t.timestamps
    end
  end
end


Practical:

class User < ApplicationRecord
  has_many :children ,class_name: "User", foreign_key: "parent_id"
  has_many :grandchildren, :through => :children, :source => :children
  belongs_to :parent, class_name: "User"
end


2. Include vs Join
3. Module include ,exclude
4. Design pattern
Answer: https://github.com/hackerschoolmty/rails-patterns#single-responsibility-principle
5. Proc vs Lambda vs &block
6. rails security
7. rspec => association
8.What is Rake?
https://thoughtbot.com/upcase/videos/rack
http://guides.rubyonrails.org/rails_on_rack.html
9.Fireman
10. what is cucumber?
11. Resque,Memcache?
Resque: Pros: does not require thread safety (works with pretty much any gem out there);
        Cons: 1. runs a process per worker (uses more memory);
              2.does not retry jobs (out of the box, anyway).
Sidekiq:Sidekiq allows you to move jobs into the background for asynchronous processing. It uses threads instead of forks so it is much more efficient with memory compared to Resque.
         Pros: runs thread per worker (uses much less memory);
         Cons: [huge] requires thread-safety of your code and all dependencies. 
               If you run thread-unsafe code with threads, you're asking for trouble;
12. Rails API implementation Restful Api, single respionse object

What makes an API RESTful?

+ Statelessness: Client state should not be stored on the server between requests. Said another way, each individual request should have no context of the requests that came before it.
+ Resource identification per request: Each request should uniquely identify a single resource. Said differently, each request that modifies the database should act on one and only one row of one and only one table.Requests that only fetch information should get zero or more rows from one table.
+ Representational state transfer: The resource endpoints should return representations of the resource as data, usually XML or JSON. The returned information should be sufficient for the client to uniquely identify and manipulate the database row(s) in question


1. API versioning is more important:
Even when you just started developing your product and are not sure if you will ever have the chance to create the next version of your API, version your API. It comes at no cost and will make it so much easier to add that new version when your product succeeds. If you have a successful product, you will almost always have to adapt your API interface and thus need to release a v2 of your API!

The background of this is simple: Changing a published and used API interface is bad. Really bad. Consumers of your API rely on its interface. They might have important business logic based on your API and it might fail we you change your endpoints. The consequences may long from minor inconvenience to customers suing your company for financial reimbursement.

When using Rails a simple namespace in your routing is enough to define the different versions:

namespace :api do
  namespace :v1 do
    resources :projects
  end
end


2.Use HTTP Status Codes Correctly
HTTP provides you with a great mechanism to tell your API consumers if their request was successful or not: HTTP status codes.

Use these status codes correctly and provide your consumers with useful information. They will thank you. The following table notes our use of the HTTP status codes in a typical REST-API.

Code	Name	Meaning
200	OK	Everything went fine. I return the resource you requested.
201	Created	Voilá. We successfully created a new resource for you.
204	No Content	There is nothing to see here. Useful if you just deleted an object successfully.
401	Unauthorized	You did not provide valid credentials.
404	Not found	Return this if a requested object could not be found. (Or if you want to conceal the existence of an invalidly requested resource)
422	Unprocessable Entity	Resource cannot be saved. Maybe a validation error?
Rails controllers make it really convenient to use the correct status codes, i.e:

render json: @object, status: :created

Single Responsibility Principle (SRP)::
is the “S” of well-known SOLID principle. It sounds simple, but it is very powerful. According to this principle –
“There should not be more than one reason for a class to change.”

It means that each class/module should have only one responsibility. If it seems like a single class is performing multiple types of task then we should try to separate those functionalities and implement them in other classes or modules.

When we implement SRP in our project, the class/module becomes highly cohesive and less coupled with each other. As a result the classes/modules do not depend too much on each other. This has two benefits:

a) they become reusable, and
b) it becomes super easy to write test code for them.


WHEN TO IMPLEMENT SRP IN RAILS MODEL:::

For a well maintained project, it is very important to implement SRP. However, it is also not a good idea to overdo this principle. It depends on project structure and other factors. Here are some checkpoints for implementing SRP to a project. There may be others. But I follow these 3 checkpoints –

1.Does this model have more than 500 lines?
If the file has more than 500 lines of code, then it seems like the model has become less maintainable. In that case it is a good idea to separate the responsibilities and extract them in separate classes.

2. Do I change this model frequently?
The more frequently a file is changed, the more risky it is. If you find that you change this file a lot then it is a good idea to implement SRP. There is gem named ‘Churn’ which can create report on how many times each file has been changed over a specific time period.

3. Is this a responsibility magnet?
There are some models in each project that act like responsibility magnet. For example, for most projects, ‘User’ model is a responsibility magnet. These models have a tendency to have a lot of diverse responsibilities. Another example is, if you have a website about movies then your ‘Movie’ model will probably be responsibility magnet. Before adding any method to a model like that, we should ask ourselves whether we can extract some common functionalities and separate them into a different place.

SOLID isn’t a set of rigid rules nor is the only guideline group you should use, but it’s a very good starting point and should change the way you design and code software right away. Here are the principles described by SOLID:

Single Responsibility Principle
Open-Closed Principle
Liskov Substitution Principle
Interface Segregation Principle
Dependency Inversion Principle
=========================================================================================================
13.Ruby class hierarchy?
14.Monkey patching in ruby?
15. Docker?
16. fabbonacci series in ruby?
=============================================================================================================
17. Differences between rails4 and rails5?
1. Rails 4 requires Ruby 1.9.3 or higher and prefers Ruby 2.0 while Rails 5 only work with Ruby 2.2.1 or better.
2. ActionMailer:
   1. deliver and deliver! methods have been removed. Instead that you can use deliver_now or deliver_later.
   2.*_path helper in email views.
3. 4. Goodbye, "rake"!**
    If you've used Rails before, you'll have to unlearn using the rake command in the terminal to run tasks . 
    In Rails 5, you can do everything with rails command.
    Example, rake db:migrate is now rails db:migrate or rake db:restart is now rails restart.
4.ActiveRecord Improvements or method in ActiveRecord::Relation
  1.#or method in query
  type1:
  Book.where('id = 1').or(Book.where('status = 3'))
  SELECT * FROM books WHERE (status = 1) OR (status = 3)
  
  type2:
  class Book < ActiveRecord::Base
   scope :new_coming, -> { where(status: 3) }
  end
  
  Book.where('id = 1').or(Book.new_coming)
  SELECT * FROM books WHERE (id = 1) OR (status = 3)
  2.From now on every Rails application will have a new configuration option config.active_record.belongs_to_required_by_default = true, 
    it will trigger a validation error when trying to save a model where belongs_to associations are not present.
    config.active_record.belongs_to_required_by_default can be changed to false and with this keep old Rails behavior or we can disable this validation on each belongs_to definition, just passing an additional option optional: true as follows:

    class Book < ActiveRecord::Base
     belongs_to :author, optional: true
    end
5.Rails 5 has implemented API only option - which would create minimalistic Rails application.
  Command to create Rails API application:
    rails new my_rails_api_app --api
  Convert Existing Rails Application to API application

    Open your config/application.rb file and add following line -

    config.api_only = true
    Edit your app/controllers/application_controller.rb

    instead of,

    class ApplicationController < ActionController::Base
    end
    do this: inherit from ActionController::API

    class ApplicationController < ActionController::API
    end

6. Rails 5: ActionCable

    ActionCable is the new thing in Rails 5, it is a framework for real time communication over web sockets.
    ActionCable is Framework for Real-Time communication over WebSockets. ActionCable is integrated websocket for Rails Applications. ActionCable is being implemented as a gem at Rails/ActionCable. This would be Merged with Rails once implementation is completed and stable.

    What problem is solved by ActionCable?

    Suppose you have a mailing application, where you would want to update a new email in the browser as soon as received.

    Possible Solution

    You can keep polling after some time interval to server side (backend) and if you get new email in response then update the DOM with the content received from backend via javascript. But, this would result in many number of polls void/useless.

    WebSocket is a protocol providing full-duplex communication channels over a single TCP connection. [Source]

    ActionCable Terminologies:

    Cable - Cable is just like a way to communicate between browser (client) and server through a connection which is made using websockets

    Connection - Connection is created using WebSockets

    Channel - A Cable can have multiple channels. Channels need to be created to send or receive for various functionalities that need solution to the similar problem discussed above.

    Broadcast - Server can broadcast to different channels particular data
7.Migration api changed for  Rails5  https://blog.bigbinary.com/2016/03/01/migrations-are-versioned-in-rails-5.html



Symbols were not getting garbage collected prior to Ruby 2.2.1. From the release of Ruby 2.2.1, symbols are garbage collected as well.
========================================================================================================================
18. Concern?
the ActiveSupport Concern module, which only recently made its debut in Rails 4.
http://vaidehijoshi.github.io/blog/2015/10/13/stop-worrying-and-start-being-concerned-activesupport-concerns/
=======================================================================================================================
19. Imgae upload using api?
https://github.com/pluralsight/guides/blob/master/published/ruby-ruby-on-rails/handling-file-upload-using-ruby-on-rails-5-api/article.md

=======================================================================================================================
20. props vs state in React? , React life cycle? Redux
21. View side performance?
22.  x = ["rasd", "erewa", "erewrewe", "trderb"] sort by last character
Ans: x.sort{|a,b| a[-1] <=> b[-1]}


