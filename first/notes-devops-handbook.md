# DevOps: How I Learned to Stop Worrying and Love ~~the Bomb~~ Deploys

The DevOps Handbook: How to Create World-Class Agility, Reliability,
& Security in Technology Organizations is an incredible source of knowledge, written by some 
very smart folks from the DevOps community. You should really check that out if have the chance.
The book offers, as the name implies, practical information and real life experiences in moving an organization 
to a DevOps value stream.

If you're new to this, you're probably wondering what a value stream is the first place.
Quoting from the book: 
> In the DevOps, we typically define a value stream as the process required to **convert
> a business hypothesis** into a **technology-enabled service** that delivers **value** to the **customer**.

Right off the bat, the authors split DevOps practices into three ways:
- **The First Way** enables fast left-to-right flow of work from Development to Operations to the customer. In order to maximize flow, we need to make work visible, reduce our batch sizes and intervals of work, build in quality by preventing defects from being passed to downstream work centers, and constantly optimize for the global goals.
- **The Second Way** enables the fast and constant flow of feedback from right to left at all stages of our value stream. It requires that we amplify feedback to prevent problems from happening again, or enable faster detection and recovery.
- **The Third Way** enables the creation of a generative, high-trust culture that supports a dynamic, disciplined, and scientific approach to experimentation and risk-taking, facilitating the creation of organizational learning, both from our successes and failures. Furthermore, by continually shortening and amplifying our feedback loops, we create ever-safer systems of work and are better able to take risks and perform experiments that help us learn faster than our competition and win in the marketplace.

The book is divided into three parts, one per way. Each part documents the relevant technical 
practices with supporting case studies and experiences sourced from industry behemoths like Netflix and Amazon.
But this is not a book review, I'm not a DevOps expert and truth be told, this is the first book I read on the subject.

So, what is this anyway?
I found that a lot of the content in this book resonates incredibly well with my work experience and wanted
to share some insights, particularly the ones related to the **First way**.


## The First Way - The Technical Practices of Flow
#### Create the foundation of our Pipelines!

Quoting from the book:
> In order to create fast and reliable flow from Dev to Ops, we must ensure that we always use 
> production-like environments at every stage of the value stream. Furthermore, these environments 
> must be created in an automated manner, ideally on demand from scripts and configuration information stored
> in version control, and entirely self-serviced, without any manual work required from Operations. 
> Our goal is to ensure that we can re-create the entire production environment based on what’s in version control.

The authors go on to say that we should enable on demand creation of dev, test, and production environments so 
developers can run production-like environments anywhere, even on their own computers, created on demand and self-serviced.

This way, developers can run and test their code in a production-like environment as part of their daily work, 
providing early and constant feedback on code quality.

#### Create our single repository of truth for the entire system
To ensure that we can restore production service repeatedly and predictably (and, ideally, quickly)
even when catastrophic events occur, we must check in all the assets needed to recreate the system state.
That includes:
- All application code and dependencies (e.g., libraries, static content, etc.)
- Any script used to create database schemas, application reference data, etc.
- All the environment creation tools and artifacts
- Any file used to create containers (e.g., Docker or Rocket definition or composition files)
- All supporting automated tests and any manual test scripts

Ideally this should happen using immutable infrastructure where no manual changes occur - make it easier to destroy and rebuild than to configure.

#### Enable fast and reliable testing

Key principles / requirements:

- Design, architect and build for testability. Consider TDD.
- Create a proper test pyramid (unit tests on the bottom)
- Tests must be fast
	- How fast? Faster than that - Automate!
	- Run in parallel if needed
- Tests must cover all code: application, infrastructure
- Some manual testing will always be needed e.g. exploratory, but we should minimize it
- Pull the andon cord (stop the line) when the pipeline breaks



#### Continuous Integration and Trunk Based Development

- Small batches!
- Keep trunk in a deployable state

#### Automate Deployments

 Enablers:

- Automate deployments to all environments
- Make and keep all environments as similar as is practical (they must be and remain consistent - eliminate, minimize drift)
- Provide self-service capabilities with appropriate controls
- Includes all steps for example
	- Packaging code in ways suitable for deployment 
	- Creating pre-configured virtual machine images or containers 
	- Automating the deployment and configuration of middleware 
	- Copying packages or files onto production servers 
	- Restarting servers, applications, or services 
	- Generating configuration files from templates 
	- Running automated smoke tests to make sure the system is working and correctly configured 
	- Running testing procedures 
	- Scripting and automating database migrations

#### Decouple Deployment from Release
Deploy is pushing your code to some part of your infrastructure and release is exposing code to execution.
Deploy code regularly to stay safe, release features as needed to deliver value.

Common strategies:
- Environment based:
	- Blue Green - bring up the old and new version on separate clusters/servers, route traffic to new, experience problems, switch back 
- Canary 
	- serve new version to a subset of the customer base prior to broad release to validate all is well
- Cluster immune system builds on the canary pattern such that deployments/releases are tied to business and environmental telemetry that will automatically roll or switch back to previous version if problems are experienced e.g. average number of orders per minute drops dramatically post release
- Application based:
	- Feature flags - enable features to be toggled on and off to Roll back easily
		- A/B test
		- Gracefully degrade deployment
		- Increase resiliency via SOA e.g. toggle a service based on its availability 
	- Dark launch 
		- make a feature available in production for testing evaluation prior to making it available to customers (see below) 
	- Graceful degradation under stress 
		- if performance begins to falter a lesser version of the feature is toggled on to maintain at least the basic capability
