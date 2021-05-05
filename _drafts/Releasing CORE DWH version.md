---

---
# Releasing CORE DWH version

Now I would like to pin a specific version of code to a tag and release.

{% highlight bash %}

git tag -a 0.2.0 head # tag
git push origin 0.2.0 # push to origin

{% endhighlight %}

[![](https://habrastorage.org/webt/iq/oe/c0/iqoec0kjply_cvccpeuxyxeqpey.png)](https://habrastorage.org/webt/iq/oe/c0/iqoec0kjply_cvccpeuxyxeqpey.png)

Importing this module becomes as simple as this:

`packages.yml`:

{% highlight yaml %}
{% raw %}

packages:
 - git: "https://github.com/kzzzr/mybi-dbt-core.git"
   revision: 0.2.0

{% endraw %}
{% endhighlight %}