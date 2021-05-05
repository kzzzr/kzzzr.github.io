---

---
# Releasing CORE DWH version

Now I would like to pin a specific version of code to a tag and release.

{% highlight bash %}

git tag -a 0.2.0 head # tag
git push origin 0.2.0 # push to origin

{% endhighlight %}

![](https://lh4.googleusercontent.com/zoqF1cfe78bBb3PL2J2jiwyzKAhUOUX9po2Hk1GiS2FEL2qVyaOvgt75Siksf52-yuCu5OcQg-weE583chp8NzwssTYkSE_0B4muGpimdRcPHONZF6NxHaRMH5zOjUZidG1l5nkH =624x225)

Importing this module becomes as simple as this:

`packages.yml`:

{% highlight yaml %}
{% raw %}

packages:
 - git: "https://github.com/kzzzr/mybi-dbt-core.git"
   revision: 0.2.0

{% endraw %}
{% endhighlight %}