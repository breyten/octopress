---
layout: post
title: "Tracking Django projects in Recents"
date: 2014-04-09 12:30:15 +0200
comments: true
published: true
categories: [recents, django]
---
[Recents](http://recents.io/) is a nice little project that shows the programming projects you most recently worked on. An awesome idea, but it currently ships with only limited support for different kinds of development projects. More specifically, it has no support for Django projects yet. Fortunately, you can add it yourself.

<!-- more -->

Adding a Django project is pretty easy. You just click the wheel when you click on the icon in the menu bar and then it will start a text editor with the settings, which is a JSON structure. This structure is [explained](https://github.com/teammixture/recents-settings) on the Github page. Of course, the key file for a Django project is obvious (`manage.py`). The only difficult thing is to figure out the name of the project. Recents has support to search the name of a project in a settings file. For a Django project, this settings file is usually `manage.py` as well. Most of the time, it will contain a line like this:

``` python
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "app_name.settings")
```

From this line we can extract the project name using a regular expression. In the end, I ended up with the following:

``` json
{
   "projectType":"django",
   "nameRegex":"\\s*os\\.environ\\.setdefault\\(\"DJANGO_SETTINGS_MODULE\", \"(.*)\\.settings\"\\)",
   "settingsFile":"manage.py",
   "image":"django.png",
   "keyFile":"manage.py",
   "projectRootRelativeToKeyFile":null,
   "filesExistInRoot":[
      "manage.py"
   ],
   "ignoreIfFilesInRoot":null,
   "ignoreIfPathContains":null,
   "button1":{
      "name":"Editor",
      "app":[
         "Sublime Text",
         "Sublime Text 2",
         "Coda 2"
      ],
      "arguments":""
   },
   "button2":{
      "name":"Terminal",
      "app":[
         "iTerm",
         "Terminal"
      ],
      "arguments":""
   },
   "button3":{
      "name":"Finder",
      "app":[
         "Finder"
      ],
      "arguments":""
   },
   "button4":null
}
```

If you want a nice little icon for your Django project, just pick an image and copy it to the `/Applications/Recents.app/Contents/Resources/` directory and you're done.