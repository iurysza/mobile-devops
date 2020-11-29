# CI/CD Deep dive

This project is a way for me to document what I learn about CI/CD concepts and ideas. 
Keep in mind that I'm obviously biased by my own perspective, a software engineer with a
background focused in mobile development and that I may also get a lot of stuff wrong.
This should probably be a private project, but  ¯\_(ツ)_/¯

The idea is to write a bunch of articles. Each article should target one or more of the following topics:
# Study outline
## CI/CD at conceptual level 
**Books/Articles resources:**
- [The DevOps handbook](https://www.amazon.com/DevOps-Handbook-World-Class-Reliability-Organizations/dp/1942788002)
- [The Twelve Factor App](https://12factor.net/)
- [Feature Toggles](https://martinfowler.com/articles/feature-toggles.html)
- [Feature Flags](https://jeroenmols.com/blog/2019/08/13/featureflags/)

**Deployment/Release decoupling**
- multiple strategies
- Feature Toggles
- Experimentation: A/B Tests

## My experience:
- small companies
- startups
- super-apps with millions of users 
- Private library projects 
- OSS libraries.

### Basics
Use kotlin-scaffold project as the template 
for the articles

**Static Analysis**:
- concept
- tools
- approaches

**Publishing**:
- Java/kotlin 
	- Private nexus
	- Obfuscation
	- OSS publishing
- Android lib publishing
