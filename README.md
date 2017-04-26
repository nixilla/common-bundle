# Common Symfony classes
 
Common Symfony classes used throughout the projects

[![Build Status](https://travis-ci.org/FlyingColours/common-bundle.svg?branch=develop)](https://travis-ci.org/FlyingColours/common-bundle)
[![Coverage Status](https://coveralls.io/repos/github/FlyingColours/common-bundle/badge.svg?branch=develop)](https://coveralls.io/github/FlyingColours/common-bundle?branch=develop)

## Components

### Content Negotiation and Template Resolver Listener

Symfony Event Listener which works out right response content type based on "Accept" header.

```yml
# app/config/services.yml

parameters:

    priorities: [ 'application/json', 'text/html' ]

services:

    content.negotiator:
        class: Negotiation\Negotiator
        
    listener.template.resolver:
        class: FlyingColours\CommonBundle\Listener\TemplateResolverListener
        arguments: [ "@sensio_framework_extra.view.guesser" ]
        tags:
            - { name: kernel.event_listener, event: kernel.controller, method: onKernelController }
    
    listener.content.negotiation:
        class: FlyingColours\CommonBundle\Listener\ContentNegotiationListener
        arguments: [ "%priorities%", "@content.negotiator", "@serializer", "@templating" ]
        tags:
            - { name: kernel.event_listener, event: kernel.view, method: onKernelView }

```
