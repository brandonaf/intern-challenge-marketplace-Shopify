# Table of Contents



[Introduction](https://github.com/brandonaf/intern-challenge-marketplace-Shopify/blob/master/README.md#introduction)

[Local Installation Instructions](https://github.com/brandonaf/intern-challenge-marketplace-Shopify#installation-instructions)

[Usage Guide](https://github.com/brandonaf/intern-challenge-marketplace-Shopify#usage-guide)

[Thought Process](https://github.com/brandonaf/intern-challenge-marketplace-Shopify#thought-process)

# Introduction
Hello, I'm Brandon! Thank you for reviewing my coding challenge submission. 

- The purpose of this application is to simulate the backend of an online shopping experience. Using this API one can view a list of inventory, create a cart, and purchase products in the cart.

- There is error handling throughout the entirety of the application to ensure that the user does not change the database in an unexpected manner. Additionally, there are appropriate error messages that are returned, so that the user understands if they have done something wrong.  

- The application has been deployed to a Google Cloud instance, thus eliminating the need for local installation. However, if you would like to install it locally for testing purposes, detailed  [Installation Instructions](https://github.com/brandonaf/intern-challenge-marketplace-Shopify#installation-instructions) have been included in this README.

- For a deeper look into my thought process, and for explanations behind the decissions I made, please see the [Thought Process](https://github.com/brandonaf/intern-challenge-marketplace-Shopify#thought-process) section. 



# Local Installation Instructions

1. Ensure your Rails version is >= 5.2.2 and your Ruby version is >= 2.5.1
2. Navigate to where you would like to install the project on your computer and use `mkdir intern-challenge-marketplace-Shopify` to make a folder
3. Navigate to the newly created folder using `cd intern-challenge-marketplace-Shopify`
4. Clone this repository into the folder using `git clone [cloning link]`
5. PostgreSQL is used as the primary database for this application. Please ensure PostgreSQL is installed on your machine. If it is not please install it using the following tutorials:

MacOS: `https://medium.com/@Umesh_Kafle/postgresql-and-postgis-installation-in-mac-os-87fa98a6814d`

Ubuntu: `https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-18-04`

6. In the application directory run `rake db:create` to create the database

7. Now run `rake db:migrate` to create the necessary tables. 

8. In order to seed the database with default mock data (used for testing the application) run `rake db:seed`

9. Now we are ready to start the rails server. Run `rails s` to start the server on the default port (which is usually port 3000). If you would like to specify the port use `rails s -p [port number here]`

10. Now that our API is running, we need a method to query this API. This can be done in many ways (including cURL requests, using a browser, or a dedicated application). If you are using MacOS or Ubuntu I suggest you download `Postman` as it provides a nice GUI for API requests.

11. That completes the setup! To learn how to use this application, please see the "Usage Guide" section of this README

# Usage Guide



Keep in mind that the following queries can be used for either the deployed version of the application, or a local version. If testing locally, ensure that the URL for the requests is changed to reference the local host IP address which is `127.0.0.1`. Also, ensure that the "3000" in the URL is replaced with the port number you specified when running the local Rails server. 



**Viewing inventory:**

1. To view a JSON list of inventory (including items with zero stock) use the following GET request `http://34.73.21.104:3000/api/v1/show`

2. To view a JSON list of inventory (excluding items with zero stock) use the following GET request `http://34.73.21.104:3000/api/v1/show?instock=true`

3. To view a single product's information use the following GET request `http://34.73.21.104:3000/api/v1/show?name=Apple` If an invalid name is specified, the appropriate error message will be returned.  


**Creating a cart and purchasing products:**

1. To create a new cart use the following GET request `http://34.73.21.104:3000/api/v1/newcart`

2. To add an item to your cart use the following GET request: `http://34.73.21.104:3000/api/v1/add?name=Kitkat+bar&quantity=5`
     
     - In the URL enter the name of the product you wish to add to your cart in the `name` argument (if the name has a space ensure you           use a "+" between the words)
     - In the URL enter the quantity of the product you wish to add to your cart in the `quantity` argument
     
     - If an invalid name or quantity is requested the API will not allow you to add the item to the cart, and will return the appropriate        error message
     
3. To view your current cart use the following GET request: `http://34.73.21.104:3000/api/v1/viewcart`. Subtotals are shown for each product, and the grand total of the cart is also returned. 

4. To checkout the items in your cart use the following GET request: `http://34.73.21.104:3000/api/v1/checkout`. "Checking out" your cart will reduce the inventory count by the specified quantity


# Thought Process

- To develop this API I decided to use the Ruby on Rails framework as I have plenty of experience developing in it, and it is used at     Shopify. 

- Since no front-end was required for this project, when creating a new Rails application, I configured it with the sole purpose of being  an API

- If at a later date, a front-end was required, I would create a separate React application and have it query the Rails API to            retrieve/post back-end data

- When creating the Rails application, I decided to use PostgreSQL as the primary database client. This is because PostgreSQL works well with deployment services such as Heroku. Additionally, PostgreSQL is much better for a larger-scale application in comparison to the default Rails database client, sqlite3

- To make things simple for users testing the application locally, I configured a seeds.rb file. This file populates the database with default values so that the user doesn't have to populate the database manually with mock data

- A minor change I made to the application specification is that I allow users to add a specified quantity of an item to their cart, rather than just one. This change better reflects a real-world e-commerce experience

- To make the API more secure, I configured the server's firewall in such a way to prevent unauthorized requests from reaching the backend. Additionally, when creating the `routes.rb` file, I only configured routes that I expect the user to use. This prevents someone from altering the database in an unexpected manner.  

- Since a member from the Shopify team will be reviewing my code, I focused more on readability rather than performance. Additionally, since this project was fairly small-scale, small differences in runtime efficiency are negligible. However, in a real-world, large-scale application, I understand how important it is to optimize runtime. Even a small efficiency change on a large-scale system can make a massive difference in overall runtime.
