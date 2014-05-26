---
layout: post
category: article
title: Cleaning up the git history of a Symfony 2 project.
comments: true
---

# Cleaning up the git history of a Symfony 2 project.

{% excerpt %}


If your Git repository contains files that should never have been commited, you are in the right place. Let's say you have [commited a configuration file][github-fails]
containing sensitive data such as SSH access to your servers or database credentials. Or you have commited an external library massively increasing the repository size, there is a quite simple way
to correct that. You night have noticed that removing the file from git in a commit does not decrease the repository size nor remove the file from the file from history.
Indeed, every file that once has been tracked is fully present in the git history since Git is able to fully restore the workspace to a given commit. If the file was
present in a commit, Git must have the file stored somewhere.

{% endexcerpt %}

# Identify the trash

First get the file/folders to remove. For a Symfony project, those files are :

- vendors : all the external librairies, handled by [Composer][composer]
- app/cache and app/logs : quite explicit, no reason those should be tracked.
- app/config/parameters.yml : this file might contain quite sensitive data.
- web/bundles : compiled/copied/symlinked files.


# git filter-branch

Git clearly contains some dark voodoo and looking at the [man page of git-filter-branches][git-filter-branch] confirms that. From what I understood from
the obscure language spoken on that page, git-filter-branches will iterate over all commits and execute a function on each commit.
So let's try that :

<div class="syntax">
{% highlight bash %}
git filter-branch --index-filter "git rm -rf --cached --ignore-unmatch vendor app/logs app/cache app/config/parameters.yml" --prune-empty -- --all
{% endhighlight %}
</div>


# Cleaning up git.

Now that you have removed all references to those files from you git history, you can tell git to update it's file-database and remove all files
that are no longer referenced. That is what we do with the following commands :

<div class="syntax">
{% highlight bash %}
rm -rf .git/refs/original
git reflog expire --expire=now --all
git gc --aggressive --prune=now
{% endhighlight %}
</div>

If all went well, the files will have completely dissappeared from your project's history. The repository size should also have dropped nicely. The project I tried this on,
was about 9.3 MB before and only 2.2 MB after cleanup.

**DISCLAIMER** : This completely screws up your Git history, since it will regenerate new SHA1 hashes for every "modified" commit.
This will have side-effects if your repository is published and shared.  **You should only run this if you understand what you are doing.**


[composer]: https://getcomposer.org/
[github-fails]: https://github.com/search?q=secret+extension%3Ayml+path%3Aapp%2Fconfig%2Fparameters.yml&type=Code&ref=searchresults
[git-filter-branch]: https://www.kernel.org/pub/software/scm/git/docs/git-filter-branch.html
