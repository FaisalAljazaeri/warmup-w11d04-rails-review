# Rails Review

For each of the following topics, please answer each of the questions. You can use links and images to support your answer. Once done please make a pull request.

## Has many Through

- [Rails Active Record Intro](https://github.com/sei-entropy/lesson-w11d02-rails-active-record#active-record-associations)
-[Hospital App](https://github.com/sei-entropy/hw-w11d02-rails-hospital)

### Questions

1. What is the role of a join table in a many-to-many relationship?
  Creating a table to hold references of both of the tables, which created the 
  many-to-many relation between the two models.


2. What two columns must be present in a join table?
  a column to hold a foreign key that refrences the primary key of the first table,
  and a column to hold a foreign key that references the primary key of the second table.


3. Given the example below, edit the code to define a has many :through relationship.

    ```ruby
    class Customer < ActiveRecord::Base
      has_many :purchases
      has_many :products, through: :purchases
    end

    class Product < ActiveRecord::Base
      has_many :purchases
      has_many :customers, through: :purchases
    end

    class Purchase < ActiveRecord::Base
      belongs_to: product
      belongs_to: customer
    end
    ```


4. Based on #3, give an example of associating two instances via the join model.
  ```ruby
  class CreatePurchases < ActiveRecord::Migration
    def change
      create_table :purchases do |t|
        t.references :product, index: true, foreign_key: true
        t.references :customer, index: true, foreign_key: true

        t.timestamps null: false
      end
    end
  end
  ```


## Devise

- [Devise Lesson](https://github.com/sei-entropy/lesson-w11d03-rails-devise)

### Questions

1. What does the `current_user` method that the Devise gem provides?
  It's a helper to provide you with the object of the current signed-in user
  so you can reterive information about them, like for example their email address


2. What does the `authenticate_user!` method that the Devise gem provides?
  Used with controller's action methods to enuser thet selected actions require authentication
  Example: 
  ```ruby
    before_action :authenticate_user, only: [:delete, :create]
  ```


3. Write a signout link using the `link_to` rails helper and a devise path.
  ```ruby
    <%= link_to "Sign Out", destroy_user_session_path, method: :delete %>
  ```


4. How do I generate a devise model in the terminal?
  rails generate devise model_name


5. What are the trade offs for using a gem for authentication over a handrolled solution? (no real right answer)
  Most likely the gem that provides the auth features has been used and tested with many projects
  thus making it more reliable and better that a typical handrolled solution.



## Validations

- [Validations Lesson](https://github.com/sei-entropy/lesson-w11d03-rails-model-validations)

### Questions

1. Which component, of Rails MVC, is responsible for the business logic?
  The Model


2. Assume the user's age is an optional field.  Write the validation to verify that a User's age is between 13 and 125 (inclusive).
  ```ruby
    validates :age, numericality: {greater_than_or_equal_to: 13,  only_integer: true }
    validates :age, numericality: {less_than_or_equal_to: 125,  only_integer: true }
  ```


3. What would `user.errors.messages` return (for the above User), if you assigned `user.age = 12`?
  "age field must be greater than or equal to 13"


4. Assume you visit "/customers/new" and enter some invalid information.  Given this controller code, what url would your browser be on after pressing "Create Customer"?
  "/customers" is the url but the 'new' form will be rendered there

``` ruby
class CustomersController < ApplicationController
  def new
    @customer = Customer.new
  end

  def create
    @customer = Customer.new(customer_params)
    if @customer.save
      redirect_to customer_path(@customer)
    else
      render :new
    end
  end
  ...
  private
  def customer_params
    params.require(:customer).permit(:first_name, :last_name, :age)
  end
end
```


5. Give one reason why we might have the similar validations in the browser, model, and database layer of our application.
  Because you can't gurantee any input that you might get from a client and you need to validate in
  multiple places to avoid any problems.

