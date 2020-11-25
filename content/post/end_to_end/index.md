---
authors:
- admin
categories: []
date: "2020-11-21T00:00:00Z"
draft: false
featured: false
image:
  caption: ""
  focal_point: ""
lastMod: "2020-11-25T00:00:00Z"
projects: []
subtitle: Resources for projects and portfolio building
summary: A collection of inspiration for my own projects
tags: ["Data Science", "Machine Learning"]
title: End-to-End Projects
---


## 2021 Goals

One of my goals for 2021 is to build up a portfolio of end-to-end machine learning projects. In this post, I'll keep a running list of resources for inspiration:

[Data Science Portfolio Projects: A Step-by-Step Guide](https://www.kdnuggets.com/2020/10/guide-authentic-data-science-portfolio-project.html) (by [Felix Vemmer](https://www.linkedin.com/in/felix-vemmer/))

This is a clear step-by-step guide. I like the emphasis on web scraping which is where I need to focus my skills on next.

[Full Stack Deep Learning (at Berkeley)](bit.ly/berkeleyfsdl)

This looks to be a promising course that covers: "a promising experiment to a shipped product: project structure, useful tooling, data management, best practices for deployment, social responsibility, and finding a job or starting a venture". The course is **entirely online**. See this [tweet thread](https://twitter.com/full_stack_dl/status/1329477077733609480)

[Applied ML in Production](https://madewithml.com/courses/applied-ml-in-production/) by [Goku Mohandas](https://twitter.com/GokuMohandas)

This aims to be a "guide and code-driven case study on MLOps for software engineers, data scientists and product managers...developing an end-to-end ML feature, from product --> ML --> production, with open source tools". Sounds very promising. 

[End-to-End Machine Learning Course Catalog](https://end-to-end-machine-learning.teachable.com/p/complete-course-library-full-end-to-end-machine-learning-catalog) by [Brandon Rohrer](https://twitter.com/_brohrer_)


[First 30 days of Machine Learning](https://twitter.com/PrasoonPratham/status/1330372876134912000)

This tweet thread by [Pratham Prasoon](https://twitter.com/PrasoonPratham), as the title suggests, is for newcomers to ML, but I think by the end of the sequence (doesn't have to be 30 days) there's a Kaggle project to complete. *note*: this is not ML-in-production like some of the other resources, but Kaggle projects are great for learning. 

He has another thread [worth checking out](https://twitter.com/PrasoonPratham/status/1325331515090219008).


[Suggested Project from Jan Giacomelli](https://twitter.com/jangiacomelli/status/1331170945738760192) 

This is a pretty 🔥 thread. He suggests:

1. Build an expense tracker CLI app:

Each expensee should have the following: title (string), amount(float), created_at(date), tags(list of strings)

2 Add Database

Instead of storing/reading in/from TXT file, start using SQLite. Write script to copy all of the existing expenses from TXT file to database. Don't use ORM at this point.

3. Start using Classes

Represent expense with class Expense having attributes: title(string), amount(float), created_at(date), tags(list of strings).

Represent Database with class ExpenseRepository with methods: save, get_by_id, list, delete

4. Re-write App to use Commands and Queries

Each command/query is a class with method execute.
At initialization you need to provide all required data for execution.

Commands: AddExpense, EditExpense
Queries: GetById, ListAll

See this post on [Modern Test-Driven Development in Python](https://testdriven.io/blog/modern-tdd/)

5. Add Tests

Add tests for commands and queries

Example:
GIVEN Valid data
WHEN execute method is called on AddExpense command
THEN record is created in database with same attributes as provided

See this post on [Modern Test-Driven Development in Python](https://testdriven.io/blog/modern-tdd/)

6. Flask

Use Flask to build the web application for your expense tracker.
Reuse commands and queries inside views
Use Jinja2 for HTML templating
Add integration tests for endpoints

7. PostgreSQL

Start using PostgreSQL instead of SQLite.
You should only edit ExpenseRepository.
Create script to copy all existing data from SQLite to Postgres

8. Authentication

Add sign up and login to your Flask application
Protect endpoints for expenses to allow only logged in users to use them
Allow user to only see own expenses.

9. Dockerize and Deploy

Dockerize your Flask application
Deploy to Heroku (don't use DB in docker, use it on Heroku)

See this post on [Dockerizing Flask with Postgres, Gunicorn and Nginx](https://testdriven.io/blog/dockerizing-flask-with-postgres-gunicorn-and-nginx/)

10. Start using your application for real

Start tracking your expenses
Even the most little ones
Don't forget to add them daily

11. Data Analysis

Use Pandas and Matplotlib to analyze your expenses
Check frequency, check biggest amount, smallest amount, average amount, most frequent amount and most used tags...

Draw plots: Number of expenses per day, amount spent per day

12. ML

Build model which will predict tags based on the title of expense
Use your existing records
Although your data set is small, try to build model as precise as possible

13. Congratulate yourself

Don't forget to write a blog post for each of these steps.
Don't forget to share your code in a public git repository (GitHub)
Don't forget to tweet it out
Don't forget to add all the skills to LinkedIn



For more content on data science, machine learning, R, Python, SQL and more, [find me on Twitter](https://twitter.com/paulapivat).