# Feature Toggles
## Pete Hodgson on Martin Fowler's blog

- Feature Toggles are a powerful technique
- Allow teams to modify system behavior without changing code. 
- They fall into various usage categories, 
- That is important to take into account when implementing and managing toggles.
- Toggles introduce complexity
- We can keep them in check by using some best practices and appropriate tools
- We should aim to reduce the number of toggles in a system



- Initially you have a simple static toggle implementation
- You want to do toggle it only for a small number of users
- To do that you can:
	- Use a **Toggle Configuration** that is environment specific
	- Allow **Toggle Configuration** to be modified in runtime via some admin panel
	- Change the implementation to make dynamic, per-request decisions. Usually taking into
	account a **Toggle Context** as a proxy for users.

## Common scenarios:
**Canary releasing**
- Only a small percentage of the total userbase will see that the new path - a canary cohort.
- The concept of **cohort** is implemented on the Toggle Router.
	- A group of users is consistently assigned a specific value for the feature-flag.
	- The canary cohort was take by a random sampling of 1% of the user base.
	- This cohort receives the new feature.
	- The rest doesn't
	- The KPIS are measured on both groups and if no issues are found, they enable
	the same feature for both groups.

**A/B testing**
- The same idea as canary
- User cohorts are way bigger
- Used to resolve contentious products debates based on data, rather than HiPPOs


## Toggle Categories

### Release Toggles
Release Toggles allow incomplete and un-tested codepaths 
to be shipped to production as latent code which may never be turned on. 

- Enable trunk-based development for CI
- The most common way to enable CD principle of deploy/release decoupling.
- WIPs can be checked into the trunk while keeping it in a deployable state
- Product managers may want to delay the release of a feature for whatever reason
(marketing campaign coordination)

- **Longevity:** brief, not longer than a couple of weeks.
- **Dynamism:** static, every toggling decision is the same for a given release. 
Creating new release version to change a toggle is acceptable.


### Experiment Toggles
- used to perform a/b testing
- each user is placed into a cohort 
- each of them receives a different value
- the performance of KPIS is measured to verify the effect.
- Used for data driven optimizations

- **Longevity:** brief, needs to be in place long enough to generate valid KPI data and can't stay 
too long because of the impact of other tests may invalidate the test.
- **Dynamism:** very dynamic, each request is likely from a different user 
and thus may be routed differently from the last.


### Ops Toggles
- Control operation aspect of system behavior
- Maybe added along with a new feature release which has unclear performance implications
- Ops team can disable a the feature quickly in production
- Usually short lived, but some can stay for years.
- Kill switches that allow ops teams to gracefully degrade non-vital system 
functionality during high stress scenarios
- Each one can be seen as a manually-managed Circuit Breaker

- **Longevity:** long term, some stay indefinitely.
- **Dynamism:** dynamic, can be changed from a admin panel, ops teams need to be able to change the flow in realtime

### Permission Toggles
- change the flow for certain user groups: 
	- premium users 
	- company employees
	- alpha, beta users
- similar to canary release, but here the cohort is not random.

- **Longevity:** long term, some stay indefinitely.
- **Dynamism:** very dynamic, each request is likely from a different user 
and thus may be routed differently from the last.


- Runtime toggles are more complex
	- Ab testing toggles:
- Experiment routers make dynamic decisions per user
- use some cohorting algorithm
- router needs a context for that cohort, specifying sizes for cohort and control groups

- This can get very complex
- this is why we need techniques to mitigate the impact of This
- they are either based on the longevity or dynamism

# Implementation Techniques
## De-coupling decision points from decision logic

- This is the initial step
- Create an abstraction that you can use to define the value for a decisions
- Use it where you want to branch the code
- This makes the code that uses the router unaware of the logic behind the decision

## Inversion of Decision
Use DI to create the router and only inject it where you need it

## Avoiding conditionals
Use the strategy pattern to allow creating a custom decision maker that will instantiate
the component you need, the logic is hidden there.

# Toggle Configuration
## Dynamic routing vs dynamic configuration
Inside the dynamic toggles group there is another division

There are two ways that a flag decision can change at runtime:
- Ops Toggles: dynamically reconfigured from ON to OFF in response to a system outage 
- Permissions and Experiment Toggles: dynamic routing based on request 
context (which user is making the request)
	- Dynamic decisions, static configuration. Probably only changed on redeploy.
	- Experiments can't / shouldn't be modified at runtime, doing so would make them statistically invalid

# Approaches for managing toggle configuration
## Hardcoded Toggle Configuration
- The most basic, similar to commenting/uncommenting code

## Parametrized Toggle Configuration
- reconfigure via command-line arguments or environment variables
- Allows reconfiguration via redeploy or process restart
- Hard to manage after a while

## Toggle Configuration File
- All configuration stays in a config file
- Reconfigure via file change
- Oldest one around


## Toggle Configuration in App DB
- Working with files is cumbersome when configurations grow too much
- Configurations stay in centralized store
- At this point, people usually build an admin panel to view and modify FFs


## Distributed Toggle Configuration
- Leverages special-purpose hierarchical key-value stores, think Zookeeper
- Distributed cluster that provides a shared source of environmental configuration
- All nodes in the cluster are automatically informed of any change
- built in functionalities: simple admin panel and feature flags

## Overriding Configuration
- In the real world there are usually multiple solutions in place
- It's common to have overriding layers of toggle configuration 
- Beware - Overriding configurations goes against CDs ideal

## Per-request overrides
- Gives more confidence that the override will always be applied
- Better CD practice 
- Security issue. Requests need to be signed


# Working with feature-flagged systems
- Helpful technique, but brings complexity

## Expose current feature toggle configuration
- any system using feature flags should expose some way for an operator to discover the current
state of the toggle configuration.

## Take advantage of structured Toggle Configuration files
- typical structured, human readable (think YAML) are helpful
- Human-readable description for each toggle is surprisingly Helpful

## Manage different toggles differently
- There various categories of feature toggles with different characteristics
- We should embrace the differences
- Manage differently! Even if they derive from the same infrastructure
#### When decision logic is decoupled from Toggle Points then:

- Changing a toggle category should impact: 
	- How the toggle is configured
	- Where that configuration lives
	- who manages the configuration 
- Changing a toggle category should NOT impact: 
	- The toggle Point


## Feature Toggles introduce validation complexity
- CD becomes more complex, specially for testing
- Multiple toggles create a combinatoric explosion of possible toggle states
- You need to support runtime configuration overrides.
- Consider exposing an endpoint which allows for dynamic in0memory re-configuration of a feature flag.
Specially if using things like Experiment Toggles.




