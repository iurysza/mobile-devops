# The Twelve-Factor App

## Codebase
### One codebase tracked in revision control, many deploys
The code should always be tracked in a version control system, such as git.
There must be a one-to-one correlation between the codebase and the app:
- Each component in a distributed system is an app.
- Multiple apps sharing the same code is a violation of twelve-factor. 
They should be refactored into libraries which can be included through a dependency manager.

There must be a single Codebase per app, but you can have multiple deploys (variants) of the app.
A deploy, is a running instance of the app. You could have a production, staging and developer instances.
All stemming from the same codebase but with different configurations per deploy.

## Dependencies
### Explicitly declare and isolate Dependencies

Never rely on implicit existence of system-wide packages. Al dependencies must be declared completely and exactly, 
via a dependency declaration manifest.
The full and explicitly dependency specification is applied uniformly to both production and development.

## Config

Store config in the environment

An app's is everything that may vary between deploys (staging, prod, developer environments). This includes:
- Database handling resources
- Credentials to external services, (Amazon S3, Google services)
- Per-deploy values such endpoints, app name, etc.

A litmus test for whether an app has all config correctly factored out of the code is whether the codebase 
could be made open source at any moment, without compromising any credentials.

A common approach to config is the use of config files which are not checked into revision control.
This is a huge improvement over using constants that are checked into the code repo, but it still has a
weakness: i'ts easy to mistakenly check in a config file to the repo and there's a tendency for config files to be scattered.

The Twelve-Factor app stores config in environment variables (env vars). They offer some advantages:
- Can't be checked into the code repo
- Language and OS agnostic

**Avoid config combinatorial explosion**: Don't group environment variables based on environment. 

Storing config in the environment allows the environment variables to serve as “granular controls”, 
each one independent of the others. 
These controls allow the last mile of CI/CD pipelines to more granularly 
control application configurations at deployment and runtime. Container runtimes and orchestration 
layers provide mechanisms for injecting these environment variables when containers are instantiated. 
The overall effect is decentralizing and decoupling configuration.

## Dev/prod parity
### Keep development, staging, and production as similar as possible

Historically, there have been substantial gaps between development (a developer making live edits to a local deploy of the app) and production (a running deploy of the app accessed by end users). These gaps manifest in three areas:

- The time gap: A developer may work on code that takes days, weeks, or even months to go into production.
- The personnel gap: Developers write code, ops engineers deploy it.
- The tools gap: Developers may be using a stack like Nginx, SQLite, and OS X, while the production deploy uses Apache, MySQL, and Linux.

The twelve-factor app is designed for continuous deployment by keeping the gap between development and production small. Looking at the three gaps described above:

- Make the time gap small: a developer may write code and have it deployed hours or even just minutes later.
- Make the personnel gap small: developers who wrote code are closely involved in deploying it and watching its behavior in production.
- Make the tools gap small: keep development and production as similar as possible.
