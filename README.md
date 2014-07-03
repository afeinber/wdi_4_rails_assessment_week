# Week 4 Comprehensive Assessment

Fork this repository, update this file to include your answers, and submit a pull request. You may refer to any notes, past projects, or external resources you want. The questions do not have to be completed in order.

1. My app has Students who are organized into Houses. What lines should I add to `config/routes.rb` to create a complete set of nested RESTful routes for these resources?

resources :students


2. Write a migration to create a `students` table with columns for name, age, and something to link the student to the `House` they belong to.

class CreateStudents < ActiveRecord::Migration
  def change
    create_table :students do |t|
      t.integer :age
      t.string :name
      t.references :house
    end
  end
end



3. Assuming a Student can only be in a single House, what kind of associations should I define on these models? Do I need to introduce any new models, and if so, what associations should I define on those?

class House
  has_many :students
end

class Student
  belongs_to :house
end


4. Now I want Students to be able to enroll in Courses. What kind of associations should I define on these models? Do I need to introduce any new models, and if so, what associations should I define on those?

class Student
  has_many :student_courses
  has_many :students, through: :student_courses
end

class StudentCourse
  belongs_to :students
  belongs_to :courses
end

class Course
  has_many :student_courses
  has_many :students, through: :student_courses
end


5. I want Students to be able to leave Comments on both Courses and Professors. What kind of associations should I define on these models? Do I need to introduce any new models, and if so, what associations should I define on those?

class Student
  has_many :comments
end

class Course
  has_many :comments, as: :commentable
end

class Professor
  has_many :comments, as: :commentable
end

class Comment
  belongs_to :commentable, polymorphic: true
end


6. When creating a new Student in my students controller, assuming I have a `student_params` method and the house the student should belong to is stored in `@house`, what code should I write to link the student to the house?
@house.students.create(student_params)

7. If I'm using Devise with a User model, what code should I write in a controller or view to find out whether a user is signed in? And how can I get a reference to the currently-signed-in user?

a) user_signed_in?

b) current_user


8. If I'm using Devise with a User model, what code should I write in my students controller so that only signed-in users can access the `new` and `create` actions?

before_action :authenticate_user!, only: [:new, :create]


9. When I deploy a new version of my app to Heroku that contains a new migration, how can I run that migration on my production database?

heroku run rake db:migrate


10. If accessing my app on Heroku gives me an uninformative "something went wrong" error message, how can I find out what the real error is?

heroku logs
