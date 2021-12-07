---
description: >-
 File uploading in Ruby on Rails application with carrierwave gem using ImageKit.io.
---

# Ruby on Rails with carrierwave

This is a quick start guide to show you how to integrate ImageKit in the Ruby on rails application with carrierwave gem. The code samples covered here are hosted on Github -  [https://github.com/imagekit-samples/quickstart/tree/master/ruby/sample-app-with-carrierwave](https://github.com/imagekit-samples/quickstart/tree/master/ruby/sample-app-with-carrierwave).

This guide walks you through the following topics:

* [Setting up ImageKit Ruby SDK](ruby_on_rails_with_carrierwave.md#setting-up-imagekit-ruby-on-rails-sdk)
* [Upload images](ruby_on_rails_with_carrierwave.md#uploading-images-in-ruby-on-rails-application)

## Setting up ImageKit Ruby on Rails SDK

For this tutorial, we will create a fresh rails application and work with it. If you already have an existing Rails app, it is also possible to use that, although you would need to modify the terminal commands and configurations in this tutorial as applicable.&#x20;

Let's create a new rails application. Create a new directory and enter the command:

```bash
rails new sample-app-with-carrierwave
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
    config.public_key = 'your_public_key'
    config.private_key = 'your_private_key'
    config.url_endpoint = 'your_url_endpoint' #https://ik.imagekit.io/dgn23df2n
  end
  config.service = :carrierwave
  # config.constants.MISSING_PRIVATE_KEY = 'custom error message'
end

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

There are two different methods of uploading a file to your ImageKit Media Library:

1. Direct image upload**.**
2. Attaching the image to a model using CarrierWave**.**

### **Direct image upload**

First, add the following code to `app/views/welcome/index.html.erb`:

```ruby
<h3>File Upload</h3>
<%= form_with(url: {action: :upload}, multipart: true) do %>
    <%= file_field_tag 'picture' %>
    <%= submit_tag "Upload" %>
<% end %>

<br>

<% flash.each do |name, msg| %>
    <%= content_tag :div, msg, class: "alert alert-info" %>
<% end %>
```

This creates a simple form with a file upload field and a submit button. The form submits the file to the path `/welcome/upload`. We need to add this path to our routes. Go to `config/routes.rb`, and add the following line:

```bash
post 'welcome/upload'
```

Now, we have the route and the HTML markup set up to submit a file for uploading. Let's introduce the controller action 'welcome#upload' to receive the file and upload it to the Media Library. Go to `app/controllers/welcome_controller.rb`, and add the following method:

```ruby
def upload
    uploaded_file = params[:picture]
    @imagekitio = ImageKitIo.client
    output = @imagekitio.upload_file(
        file: uploaded_file,
        file_name: "new_file.jpg",
        response_fields: 'tags',
        tags: ["hello"]
    )
    if output[:error]
        redirect_to "/", notice: "There was some problem uploading your image"
    else
        redirect_to "/", notice: "Your image has been uploaded successfully with the name of #{output[:response]["name"]}"
    end
end
```

Here, we first extract the file from the _:picture_ property of the _params_ object, and then upload the file to our Media Library by the name of _new\_file.jpg,_ using the `upload_file()` API. We also attach an example tag '_hello_' to the image.

Now, refresh the home page, and you should see the upload field.

![Upload field](<../../../.gitbook/assets/image (13).png>)

Select an image/file from your local computer, and click on the _Upload_ button. A success message shall flash below the file field.

_Your image has been uploaded successfully with the name \<NAME\_OF\_FILE>_

Now, after upload, if you go to the [Media Library](https://imagekit.io/dashboard#media-library) section of your ImageKit dashboard, you should see the newly uploaded image.

If you get an authentication error during upload, verify that the API keys are configured correctly.

**Fetching uploaded file**

Fetch the uploaded image and display it in the UI by adding this line to your `app/views/welcome/index.html.erb`.

```markup
<%= image_tag "https://ik.imagekit.io/<YOUR_IMAGEKIT_ID>/<FILE_NAME>" %>
```

The app should render your uploaded image correctly.

### **Attaching the image to a model (using CarrierWave)**
You can attach an image as an attribute of one of your models, making it convenient to write a model-oriented code. This method uses the CarrierWave gem, which is a dependency of the imagekitio gem.

Open `Gemfile` and add carrierwave gem

```ruby
gem 'carrierwave', '~> 2.0'
```

run `bundle install`

Next, we'll create an entity called an uploader.

```bash
rails g uploader <Uploading_attribute_name>
# For example if you want to create uploader for Picture attribute then use
rails g uploader Picture
# Generated uploader's path will be app/uploaders/picture_uploader.rb
```

Now, go to the generated uploader file - `app/uploaders/<name>_uploader.eb`, and enter the following code. The _options_ configuration is optional.

```ruby
include ImageKitIo::CarrierWave

# If you want to add uploading options then create this method inside uploader file as an example

def options
    options = 
    {
        response_fields: 'isPrivateFile, tags',
        tags: ["Rails SDK uploads"],
        use_unique_file_name: false,
        folder: "your_directory/"
    }
end

# If you want to set upload dir then you can use following method or you can also use options method.
# This method shuld return string
def store_dir
    "your_directory/"
end
```

Now, we generate a model and give it a picture attribute, which will be an image. If you already have a model in your project, you can simply add the attribute to the existing model.

```bash
rails generate model Post
```

Now go to the model file - `app/models/post.rb`, and add the following lines:

```bash
class Post < ApplicationRecord
    attr_accessor :picture
    mount_uploader :picture, PictureUploader
end
```

You are free to change the name of the attribute from :picture to something else.

Perform the database migrations. This will add two columns - title, picture to the Posts table of our database.

```bash
rails g migration AddPictureToPosts picture:string
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
            <%= image_tag post.picture.url %>
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
