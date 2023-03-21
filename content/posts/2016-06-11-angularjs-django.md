---
layout: post
title: AngularJS running on Django
description: This is a description about creating web app by angularjs, running based on django.
permalink: angularjs-django
tags:
- angularjs
- django
date: 2016-06-10
---

This is a description about creating web app by angularjs, running based on django. It does not explain complicate logic, and only get focus on how to show simple page using these two framework.

Define that Django has been already installed, run commands below to create basic django project.

{% highlight groovy %}
{% raw %}
django-admin startproject djangoServer
cd djangoServer
python manage.py startapp index
{% endraw %}
{% endhighlight %}

Now there are django project with app name index. This will be used to run our angular web application. There are some places to fix.
First look on settings.py in django.

{% highlight python %}
{% raw %}
...
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': ['/Users/kwangin/WebstormProjects/angular-django/app'],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

STATICFILES_DIRS = [
    '/Users/kwangin/WebstormProjects/angular-django/app',os.path.join(BASE_DIR, "static"),
]
...
{% endraw %}
{% endhighlight %}

You need to tell where your front-end project will be placed. In my case '/Users/kwangin/WebstormProjects/angular-django/app' is the location. In django, files for web project such as CSS, JS, HTML files are being managed as 'static' files, and we need to tell where these files are. Put this address on TEMPLATES, and STATICFILES_DIRS to define the front-end dir.

Front-end part is more simple than this. Here is my sample project.

![Screenshot](/assets/post_img/angular_django/angulardefault.png)

This is based on AngularJS seed project. You could find it on https://github.com/angular/angular-seed. It only shows 2 link, and comment "Angular seed app" on single page.
There are no codes to point out backend location, but has bit bothering work to do. This part will be cons for using django with angularjs because you don't need to do this on nodeJS(or maybe such other back-end frameworks).
Django project defines to put on front-end project files as static file, we need to define the location of all front-end file as below of path 'static'. As you see in image above, file path all has been changed to be located below '/static/'

{% highlight javascript %}
{% raw %}
...
<script src="bower_components/angular/angular.js"></script>
<script src="bower_components/angular-route/angular-route.js"></script>
<script src="app.js"></script>
...
{% endraw %}
{% endhighlight %}

will be

{% highlight javascript %}
{% raw %}
...
<script src="/static/bower_components/angular/angular.js"></script>
<script src="/static/bower_components/angular-route/angular-route.js"></script>
<script src="/static/app.js"></script>
...
{% endraw %}
{% endhighlight %}

All of path includes '/static/' at front of it. This path is defined at settings.py as STATIC_URL parameter.

{% highlight python %}
{% raw %}
...
STATIC_URL = '/static/'
...
{% endraw %}
{% endhighlight %}

One more, if you need to input values on controller to template page like below, you need to wrap double-brace with 'verbatim'. The first page(usually index.html) is being called by django, and it cannot recognize double-brace form usually used on AngularJs. This issue pretty bothered me when I first created angularjs-django project.

{% highlight javascript %}
{% raw %}
...
<div ng-controller="YourCtrl">{% verbatim %}{{appName}}{% endverbatim %} seed app: v<span app-version></span></div>
...
{% endraw %}
{% endhighlight %}

This is it. Let's run django and open on local browser.

![Screenshot](/assets/post_img/angular_django/browsersample.png)
