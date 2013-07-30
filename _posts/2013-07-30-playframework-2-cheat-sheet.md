---
layout: post
category: article 
title: Play2, cheat-sheet
comments: false
---

# PlayFramework 2, cheat-sheet.

{% excerpt %}

And for the first time on this blog a post in English. Just a bunch of tips/tricks for Play I've came across on the web and decided to note somewhere. And where better to do that than on this blog. Enjoy!

{% endexcerpt %}

Here is the complete list : 

- [Log SQL queries](#log-sql-queries)


## <a id="log-sql-queries">Log SQL queries</a>

For debugging purpose, one might want to log the SQL queries issued by Play. For example when working with an "ORM" such as Slick where the SQL queries are generated. The first thing, I tried was log4jdbc but that is for the moment incompatible with Slick. (I am working on a PR) The other solution is to use the BoneCP logging capablities which are not as fine graned as log4jdbc but can be enough for debugging purpose.

- You have to add a <span class="syntax">application-logger.xml</span> file in your project as stated on the [official website][logback-configuration]. 
- Add the following to that file : 

<div class="syntax">
{% highlight xml %}

<logger name="com.jolbox.bonecp" level="DEBUG" />

{% endhighlight %}
</div>

- And last add the following to the <span class="syntax">application.conf</span> file : 

<div class="syntax">
{% highlight properties %}

db.default.logStatements=true

{% endhighlight %}
</div>

[logback-configuration]: http://www.playframework.com/documentation/2.1.x/SettingsLogger


*more to come...*
