**[< Home](README.md)**
# Interview 

## Question 

Ex 
### Why hibernate on your project ? 

Hibernate is the most common implementation of JPA, and also came with more function. 

Because the project is in Java so we use JPA. 

Because everyone on the team know how to use this tech.

### Which percentage of coverage is ideal and why ?

The standard is 80%

I think 75% is a good number 

The number isn't allmighty spoke about integration test and unit test

### How did you monitor your app ?

We want to always monitor our app.

There are a firefighter on the project in intern 

I already use on project 
- Argo
- Signoz 
- Sentry
- Dependency track

- Grafana
- Prometheus

Big on market : datadog (~ sentry)

### If the API send a 500, what did you do ?

Maybe rollback the prod app if possible

WE use monitoring servicies to check the log / error

WE hot fix it 

### What is the most difficult bug you have to solve on the last 6 mouth and which method did you use to solve it ?

Some data are created on migration script and so how can we solve that when his data are used on one instance and not the other ?


 
