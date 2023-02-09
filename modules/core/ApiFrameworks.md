# API Frameworks - A top-level overview

## Introduction

Python has a vast smorgasbord of different API / webservice frameworks, each with their own philosophy as to what a good API/webserver should look like.

What follows is a whistle-stop tour of the three most common frameworks, with signposting to further reading should you want to do more research.

I've attached some commentary as to the considerations that are worth thinking about when choosing which API framework to adopt for a project.

## Django

### What is Django?

Django is probably the most commonly used Python webservices Framework. It is a full-stack framework, providing webserver, static file templating, and ORM functionality out-of-the-box, as well as high-level functionality such as authentication, user & permission management, and an [administrative portal](https://docs.djangoproject.com/en/4.1/ref/contrib/admin/). It has become synonymous with rapid web development, though this rapidity can come with a cost. This is in part due to its very opinionated ethos as to the modular structure of any implementing service, and in part due to the rich ecosystem of plugins which have grown up around Django, providing functionality to assist with creating RESTful APIs ([Django REST Framework](https://www.django-rest-framework.org/)).

### Advantages of Django

#### Lower time-to-value for new functionality

Because it provides a large amount of functionality needed within parent classes, the amount of code needed to write complex functionality can be lower than in equivalent frameworks. This makes Django quick to prototype new functionality. It can be said that Django requires less code to implement functionality, though whether this is an advantage is debatable (see [Disadvantages of Django](#disadvantages-of-django)).

#### Highly Standardised Between Codebases

Because most codebases implementing Django will tend to follow the structure that Django strongly suggests, it reduces the time needed to get to grips with a new codebase. The downside of this is that replaces project-specific domain knowledge with Django-specific domain knowledge.

### Disadvantages of Django

#### Complex

Django has an ethos which favours minimising the amount of code required to meet any specific use case by exposing classes which implement generic use cases via a fine-grained structure of conceptually well-defined methods and relying on the inheriting class to override only the code paths where the current use-case differs from the generic use-case.  Whilst this reduces the amount of code that needs to be written for any given project, there is a high conceptual overhead in tracking the code paths defined by a hierarchy of parent classes, putting a high barrier-to-entry in terms of domain knowledge of the Django ecosystem.

#### Can be Difficult to Re-architect

Django services can be difficult to break out into domain-driven microservices, due to implicit assumptions made in Django, and it's associated plugins, such as the assumption that all data the user wishes to manipulate through the service will be stored in a local persistence store, that make transparent refactoring difficult. Django comes with its own notion of abstraction in the form of [`apps`](https://docs.djangoproject.com/en/4.1/intro/reusable-apps/), which can be used to break code into well-defined units (though it is left to the developer to implement an API layer for inter-app communication) providing some of the value associated with microservices. However, other benefits associated with microservices, such as granular deployments, are difficult to obtain with this `app`-driven notion of abstraction.

#### Difficult to Upgrade to a New Django Version

Upgrading a sizable codebase to a new Django release is rarely a straightforward activity. This is in part due to the complexity of the Django API, changes to the Django API which require changes within the codebase, but can be made all the more difficult by the varying timelines of the various Django plugins with a new Django release. Well-maintained plugins will often pose no problem, but the more esoteric plugins may have a long lead time for compatibility with a new version, or may require the team to fork and update the plugin themselves. Because of the flat dependency model of python, the upgrade itself can only be carried out once the work to ensure compatibility has been completed across the whole codebase.

#### Dependency Resolution With Large Numbers of Django Plugins

Because of the flat dependency model of python, any python package can only be installed once, no matter how many other packages require it as a dependency. As the number of plugins and other dependencies grows, it can be difficult for any one version of a package to meet many different version constraints imposed on it.

### Further Reading About Django

There are several very good Django tutorials out there:
* [DjangoGirls Django Tutorial](https://tutorial.djangogirls.org/en/)
* [Official Tutorial Django](https://docs.djangoproject.com/en/3.2/intro/tutorial01/)

There are also tutorials that are focused on Django REST Framework:
* [learndjango.com Django REST Framework tutorial](https://learndjango.com/tutorials/official-django-rest-framework-tutorial-beginners)

For those working against a sizable pre-existing codebase, the class-oriented documentation is an invaluable reference:
* [Classy class-based views](https://ccbv.co.uk/)
* [Classy class-based forms](https://cdf.9vo.lt/)
* [Classy Django REST Framework](https://www.cdrf.co/)

## FastAPI

### What is FastAPI?

FastAPI is a modern webserver framework, utilizing python type-hints for runtime type-checking, and with asynchronicity as a first-class use case.

### Advantages of FastAPI

#### Type-hints Become Single Source of Truth

By using standard python type hints for runtime Request Response validation, it allows a single source of truth for the structure of the data to be represented in a human-readable way at the function level, making inconsistencies easier to pick up in advance using tools such as mypy/pyright in an IDE, and adding a justification for keeping these type hints up-to-date

<!-- #### Generates OpenAPI documentation out of the box -->

### Disadvantages of FastAPI

#### Asynchronous methods can only easily call other asynchronous methods

This is less of a flaw with FastAPI and more of a necessary constraint when using asynchronous methods in python; these methods (coroutines) cannot call a synchronous method (subroutine), which limits the choice of libraries that can be used (though more asynchronous alternatives are becoming available). The alternative is to register the API response callback itself as a synchronous method, losing the value of asynchronicity

#### Not as Established as Other Frameworks

With its first alpha release in 2018, and still yet to achieve a major (1.0) release, FastAPI is still a relatively new framework. As a result it doesn't have the vibrant ecosystem of plugins and example code that other frameworks may have.

### Further Reading About FastAPI

[Official FastAPI tutorial](https://fastapi.tiangolo.com/tutorial/first-steps/)

## Flask

### What is Flask?

Flask has more or less become the standard for python webserver frameworks where the full-stack out-of-the-box functionality of Django isn't appropriate or desirable. It features a webserver and a templating engine (jinja2) out of the box, but can be complemented

### Advantages of Flask

#### Flexibility

Flask is ultimately very flexible in how it's used. There is a wide ecosystem of plugins which will fulfil common use-cases, but Flask imposes few constraints on how something should be done.

### Disadvantages of Flask

#### Less Standardisation

With the advantage of flexibility comes the idea that it is left to the development team to make sensible choices in implementation.

### Further Reading About Flask

[Official Flask Tutorial](https://flask.palletsprojects.com/en/2.2.x/tutorial/)
