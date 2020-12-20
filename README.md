# CI/CD Deep dive

This project is a way for me to document what I learn about CI/CD concepts and ideas. 
Keep in mind that I'm obviously biased by my own perspective, a software engineer with a
background focused in mobile development and that I may also get a lot of stuff wrong.
This should probably be a private project, but  ¯\_(ツ)_/¯

The idea is to write a bunch of articles. Each article should target one or more of the following topics:
# Learn by reading
**Books/Articles resources (reading status):**
- [x] [The DevOps handbook](https://www.amazon.com/DevOps-Handbook-World-Class-Reliability-Organizations/dp/1942788002)
- [x] [The Twelve Factor App](https://12factor.net/)
- [x] [Feature Toggles](https://martinfowler.com/articles/feature-toggles.html)
- [x] [Feature Flags](https://jeroenmols.com/blog/2019/08/13/featureflags/)
- Experimentation
	- [x] [Controlled Experiments: Crash Course Statistics](https://www.youtube.com/watch?v=kkBDa-ICvyY)
	- [ ] [Trustworthy Online Experiments](https://www.amazon.com.br/Trustworthy-Online-Controlled-Experiments-Practical/dp/1108724264) - [Notes](https://github.com/iurysza/mobile-devops/tree/master/deployment-release-decoupling/trustworthy-online-controlled-experiments-notes.md)


# Learn by writing (article ideas)
## [CI/CD at conceptual level](https://github.com/iurysza/mobile-devops/tree/master/foundation)
Book reviews
- The DevOps handbook
- Hands-on continuous integration and delivery
- The Twelve-Factor App

## [Deployment/Release decoupling](https://github.com/iurysza/mobile-devops/tree/master/deployment-release-decoupling)
- multiple strategies
- Feature Toggles
- Experimentation: A/B Tests

## My experience
- small companies
- startups
- super-apps with millions of users 
- Private library projects 
- OSS libraries

## Start simple, basic automation:
Use kotlin-scaffold project as the template 
for the articles

**Static Analysis**:
- concept
- tools
- approaches
[Article](https://dev.to/iurysza/continuous-kotlin-static-analysis-2l2o)

**Testing**:
- Making sense of coverage reports
- Best practices, flakiness, speed

**Publishing**:
- Java/kotlin 
	- Private nexus
	- Obfuscation
	- OSS publishing
- Android lib publishing


# Learn by doing
### [WIP] - [android-team-coding-test](https://github.com/iurysza/android-team-coding-test)
The idea here is to build a good-enough solution for real-world android project with multiple contributors.
The pipeline here should provide clear feedback for whoever tries to open PRs on the master brach.

- Used it to implement some `jenkins` pipelines and to learn about the tool
    - [x] Create a jenkins image to run my pipelines locally
    - [x] Setup jenkins server

- Implemented CD practices:
    - [x] auto-installing pre-commit hook
    - [x] static analysis with `ktlint` and `detekt`
    - [x] generates check-style reports that are parsed with a custom `danger-checkstyle` plugin
	- [x] uses `ngrok` for github-webhook integration to trigger local jenkins server builds
    - [x] jenkins creates inline comments (based on the checkstyle reports) on PRs opened on the master branch
    - [ ] code coverage reports are used as a quality-gate
	- [ ] setup jenkin's image so that it can build and run tests/instrumented tests for android


### [WIP] - [kotlin-scaffold](https://github.com/iurysza/kotlin-scaffold)
This is a kotlin project setup using some gradle plugins to enable fast and reliable library development. It is still missing an integration with a tool that can automate these processes.
This was more intended to learn to publish Open source software.

- Strict code styling applied to every build
- IDE formatter integration
- git hook auto installer
- Runs ktlint and detekt over files that changed on your commit.
- Provides a simple way to upload the library to `mavenCentral`
- Create tasks to generate version tags

### [flutter-scaffold](https://github.com/iurysza/flutter-scaffold)
- Simple github-actions setup to handle CI/CD

