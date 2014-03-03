---
layout: post
title: "Multiple evironments for deployment using Fabric"
date: 2014-03-03 12:44:50 +0100
comments: true
categories: [fabric, deployment]
---
I use [Fabric](http://www.fabfile.org/) to deploy my Python projects. While tt offers a lot of flexibility, it lacks a concept of different environments (Also referred to as stages) so you need to figure out your own solution.

Such a solution boils down to modifying the env variable on the fly. While [this post](http://stackoverflow.com/questions/2326797/how-to-set-target-hosts-in-fabric-file) at StackOverflow points to the right direction, it's a bit more laborious than I like since you explicitly have to set the attributes on the env variable with each option. The solution I came up with looks like this:

``` python
from fabric.api import run, local, env, settings, cd, task, put, execute
from fabric.contrib.files import exists
from fabric.operations import _prefix_commands, _prefix_env_vars, require
#from fabric.decorators import runs_once
#from fabric.context_managers import cd, lcd, settings, hide

STAGES = {
    'test': {
        'hosts': ['breyten@test.example.org'],
        'code_dir': '/var/www/test.example.org',
        'code_branch': 'master',
        # ...
    },
    'production': {
        'hosts': ['breyten@example.org'],
        'code_dir': '/var/www/example.org',
        'code_branch': 'production',
        # ...
    },
}

def stage_set(stage_name='test'):
    env.stage = stage_name
    for option, value in STAGES[env.stage].items():
        setattr(env, option, value)

@task
def production():
    stage_set('production')

@task
def test():
    stage_set('test')

@task
def deploy():
    """
    Deploy the project.
    """
    
    require('stage', provided_by=(test,production,))
    # ...
```

This allows you to deploy easily: run `fab test deploy` to the deploy to the test environment, while `fab production deploy` deploys to the production environment. Running the deploy task directly results in an error so I always have to be aware of the environment I deploy to.