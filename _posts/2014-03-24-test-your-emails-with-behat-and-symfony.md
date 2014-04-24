---
layout: post
category: article
title: Test your emails with Behat and Symfony
comments: true
---

# Test your emails with Behat and Symfony

{% excerpt %}

When writing tests with Behat, one is often confronted with the need to test
email sending. For example, when a user registers for your application, the user
might need to confirme his email address. In that case an email is send to the user
containing a link he has to follow in order to confirm. Actually testing that
this email is send is not that complicated with Behat and Symfony.

{% endexcerpt %}

## Principle

When using SwiftMailer, this is quite easy to check. SwiftMailer has a spooling mecanism
based on filesystem. Which means that every email SwiftMailer will send,
will be written to a file on the disk. All you have to do is check that file
once the email is send to check if it contains the tested email.

## Configuration

First of all, you have to setup SwiftMailer to spool emails and write those emails
to a given file. As you can see in the refence configuration, that settings is
available through the swiftmailer.spool.file parameter.

All you have to do is add the following to your config_test.yml file :

<div class="syntax">
{% highlight yaml %}
swiftmailer:
    spool:
        type: file
{% endhighlight %}
</div>

Now SwiftMailer will write all emails to the `app/cache/test/swiftmailer/default` folder.

## Email setup

Before you can start testing your emails with Behat, you have to add a unique identifier
to each of your emails. That way you can check each email individually. Once again, SwiftMailer
makes this quite easy to do. All you have to do is add a new header to your message.
This can be done like this :

<div class="syntax">
{% highlight php startinline %}
$message = \Swift_Message::newInstance();
$message->getHeaders()->addTextHeader('X-Message-ID', 'myUniqueEmailId');
{% endhighlight %}
</div>

This particular email can be referenced in Behat with `myUniqueEmailId`.

## Behat setup

Now you need a step in Behat to check those files for the right email. I have created a
small bundle which includes that necessary step (among others things) : LilwebExtraBundle.

All you have to do is add it to your `composer.json` file :

<div class="syntax">
{% highlight json %}
"lilweb/extra-bundle": "0.0.1"
{% endhighlight %}
</div>

Then add the Behat Context to your main context :

<div class="syntax">
{% highlight php startinline %}
use Lilweb\ExtraBundle\Features\Context\EmailContext;
...
$this->useContext('emailContext', new EmailContext());
{% endhighlight %}
</div>

This will make a new step available :

<div class="syntax">
{% highlight gherkin %}
And the "myUniqueEmailId" mail should be sent to "sender@adresse.com"
{% endhighlight %}
</div>


That is all you have to do ! However in some cases, for an unknown reason, this will not work every time.
About once every 20 runs, this test will fail. I am still investigating this but it appears it has something to do
with the execution order. SwiftMailer might still be writing the file to disk before
the checks runs. Re-running the tests generally make this turn green.
