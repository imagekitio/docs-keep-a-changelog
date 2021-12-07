---
description: >-
 File uploading in Ruby on Rails application with activestorage gem using ImageKit.io.
---

# Ruby on Rails with ActiveStorage

This is a quick start guide to show you how to integrate ImageKit in the Ruby on rails application with active_storage gem. The code samples covered here are hosted on Github -  [https://github.com/imagekit-samples/quickstart/tree/master/ruby/sample-app-with-activestorage](https://github.com/imagekit-samples/quickstart/tree/master/ruby/sample-app-with-activestorage).

This guide walks you through the following topics:

* [Setting up ImageKit Ruby SDK](ruby_on_rails_with_activestorage.md#setting-up-imagekit-ruby-on-rails-sdk)
* [Upload images](ruby_on_rails_with_activestorage.md#uploading-images-in-ruby-on-rails-application)

## Setting up ImageKit Ruby on Rails SDK

For this tutorial, we will create a fresh rails application and work with it. If you already have an existing Rails app, it is also possible to use that, although you would need to modify the terminal commands and configurations in this tutorial as applicable.&#x20;

Let's create a new rails application. Create a new directory and enter the command:

```bash
rails new sample-app-with-activestorage
```

Now, navigate to the newly created directory, and run the app:


```bash
rails server
```

In your web browser, navigate to [`http://localhost:3000/`](http://localhost:3000)

You should see a generic welcome message (_Yay! You're on Rails!_).

![Welcome message for a fresh Ruby on Rails application](<../../../.gitbook/assets/image (16).png>)

Next, to use ImageKit functionality in our app, we will create a new controller with the following command:

```bash
rails generate controller Welcome
```

This will generate a few scaffolding files for you. Go to `config/routes.rb` and add the following lines:

```ruby
get 'welcome/index'
root 'welcome#index'
```

Here, we are doing two things:

1. One, creating a route at the path _welcome/index, _to direct requests coming on that path to go to the index action of the Welcome controller, which we will add soon.
2. Second, we are declaring the root path to be directed to _welcome/index. _This means that all requests on `http://localhost:3000/` will go to `http://localhost:3000/welcome/index`

## **Installing the SDK**

First, add this line to your Gemfile to add the 'imagekitio' gem as a dependency.

```
gem 'imagekitio'
```

Next, run the bundle install command to install the imagekitio gem from the RubyGems repository.

```bash
bundle install
```

#### **Initializing the SDK**

Before the SDK can be used, let's learn about and configure the requisite authentication parameters that need to be provided to the SDK.

Create a file `config/initializers/imagekitio.rb`, open it and add your public and private API keys, as well as the URL Endpoint as follows: (You can find these keys in the Developer section of your ImageKit Dashboard)

```ruby
ImageKitIo.configure do |config|
  if Rails.env.development?
    config.public_key = 'YOUR_PUBLIC_KEY'
    config.private_key = 'YOUR_PRIVATE_KEY'
    config.url_endpoint = 'YOUR_URL_ENDPOINT' #https://ik.imagekit.io/dgn23df2n
  end
  config.service = :active_storage
  # config.constants.MISSING_PRIVATE_KEY = 'custom error message'
end
#make sure to replace the YOUR_PUBLIC_KEY, YOUR_PRIVATE_KEY and YOUR_URL_ENDPOINT with your keys
```

Restart the application

```bash
rails server
```

The home screen will display an error message stating that the index action could not be found for the WelcomeController. Let's fix that.
Open up the project in your text editor of choice, and navigate to `app/controllers/welcome_controller.rb`. Edit it to make it look like the following:

```ruby
class WelcomeController < ApplicationController
  def index
  end
end
```

and create `app/views/welcome/index.html.erb` file and edit file with text `Hello world`. Now refresh the browser it renders the `Hello world`.

## **Uploading images in Ruby on Rails application**
#### **Attaching the image to a model (using active_storage)**

You can attach an image as an attribute of one of your models, making it convenient to write a model-oriented code. This method uses the ActiveStorage gem, which is a dependency of the imagekitio gem.
Note: From Rails version 5, rails support active_storage by default.


```ruby
rails active_storage:install
rails db:migrate
```

Let's add imagekit service on `config/storage.yml` as below:

```ruby
imagekitio:
    service: ImageKitIo
``` 

and on `config/environments/development.rb` file:
```ruby
config.active_storage.service = :imagekitio
```

Now, we generate a model and give it a picture attribute, which will be an image. If you already have a model in your project, you can simply add the attribute to the existing model.

```bash
rails generate model Post
```

Now go to the model file - `app/models/post.rb`, and add the following lines:

```bash
class Post < ApplicationRecord
    has_one_attached :picture
end
```

You are free to change the name of the attribute from :picture to something else.

Perform the database migrations. This will add two columns - title, picture to the Posts table of our database.

```bash
rails g migration AddTitleToPosts title:string
rails db:migrate
```

Now, create another controller named after the model. For example, we'll create a controller called Post.

```bash
rails generate controller Posts
```

Now, go to `app/controllers/posts_controller.rb`, and add the following methods:

```ruby
class PostsController < ApplicationController
    def index
        @posts = Post.all
    end
    def show
        @post = Post.find params[:id]
    end

    def new
        @post = Post.new
    end

    def create
        @post = Post.new post_params
        if @post.save
            redirect_to @post
        else
            render 'new'
        end
    end

    def post_params
        params.require(:post).permit(:title, :picture)
    end
end
```

Here, we create standard methods to show all posts, show a single post, and create a new post. These methods will have to be accompanied by some HTML markup as well. So go to `app/views/posts/`, and add the following files

_index.html.erb_

```markup
<%= @posts.each do |post| %>
    <div class="post">
        <div class="image">
            <%= image_tag post.picture.url(transformation: [{height: 200, width: 300}]) %>
        </div>
        <div class="content">
            <%= link_to "#{post.title}", post_path(post), class: "header" %>
        </div>
    </div>
<% end %>
<%= link_to 'Back', posts_path %>
```

_new.html.erb_

```markup
<%= form_with model: @post do |form| %>

    <%= label_tag(:title, "Enter title of post") %>
    <%= form.text_field :title %>
    
    <%= label_tag(:picture, "Upload a picture") %>
    <%= form.file_field :picture %>
    
    <%= submit_tag "Submit" %>
<% end %>

<%= link_to 'Back', posts_path %>
```

_show.html.erb_

```markup
<div class="post">
    <div class="image">
        <%= image_tag @post.picture.url %>
    </div>
    <div class="content">
        <%= link_to "#{@post.title}", post_path(@post), class: "header" %>
    </div>
</div>
<%= link_to 'Back', posts_path %>
```
And finally let's add the routes
`config/routes.rb`

```ruby
resources :posts
```

Now, restart the server and go to _http://localhost:3000/posts/new. _You should see two form fields, for _title_, and _picture_. Enter a text value for the title field, and upload an image/file for the picture field, and click on Submit. This should redirect you to _http://localhost:3000/posts/1, _displaying the title and the image.

This sums up how you can upload images to your Media Library using the SDK. The second method is more suited for RESTful applications with CRUD operations where the image is an attribute of a resource.

## Authentication parameter generation

You can use the SDK to generate [authentication](https://docs.imagekit.io/api-reference/upload-file-api/client-side-file-upload#signature-generation-for-client-side-file-upload) parameters that are required if you want to implement client-side upload functionality using javascript or some [front-end framework](../../api-reference/api-introduction/sdk.md#client-side-sdks).

```ruby
# app/controllers/welcome_controller.rb

def auth_params
  @imagekitio = ImageKitIo.client
  @auth_params = @imagekitio.get_authentication_parameters()
end
```

```ruby
# config/routes.rb

get 'welcome/auth_params'
```

```ruby
# app/views/welcome/auth_params.html.erb

<div>
    <%= @auth_params %>
</div>
```

The `get_authentication_parameters(token = nil, expire = nil)`  takes two optional arguments, `token`, and `expire`. `token` defaults to a random string if not provided. `expire` defaults to infinite.&#x20;

You should see an output like this:

```ruby
{    
    :token=>"6b4d79e5-eb36-4de0-8ef5-534c88e45ebf",
    :expire=>1600957501,
    :signature=>"4c19c4066a140921eddaff7aaa3625df2bf8182a"
}
```

## **What's next**

The possibilities for image manipulation and optimization with ImageKit are endless. Learn more about it here:&#x20;

* [Image Transformations](https://docs.imagekit.io/features/image-transformations)
* [Image optimization](https://docs.imagekit.io/features/image-optimization)
* [Media Library](https://docs.imagekit.io/media-library/overview)
* [Performance monitoring](../../features/performance-monitoring.md)
