---

---
# Performing set operations UNION, INTERSECT, MINUS with Looker BI tool

## Stating the problem

I would like to list and count all the users who have used product A but never used product B (in a specific period/area/country).

Being more specific, imagine this data structure: journey, passenger, service group.

[![](https://habrastorage.org/webt/g7/y7/_m/g7y7_mr7mhuagangfiztlxfepsm.png)](https://habrastorage.org/webt/g7/y7/_m/g7y7_mr7mhuagangfiztlxfepsm.png)

You cannot achieve the desired result by simply slicing this table. One needs a more advanced technique.

## Plain SQL solution

Of course this task could be easily solved by writing some SQL code.

Several approaches are valid:

* You may leverage MINUS operation on top of two sets of users: those who have used product A MINUS those who have used product B.
* You can write a subquery with IN or EXISTS keywords.
* Joining a fact table on itself with proper filters (in this case anti-join) also works.

## Can I do this with BI tool?

In case I don’t have enough SQL skills? Or don’t even have the direct database access.

Also I might want to still retain pre-defined dimensions, measures and filters for which we all love BI tools.

Looker is a very powerful instrument. Let us try to accomplish this task by performing MERGE operation.

First describe primary query to get the base segment:

  
[![](https://habrastorage.org/webt/hn/1k/_b/hn1k_bkhuprtsqgjjujb0zxjp-y.png)](https://habrastorage.org/webt/hn/1k/_b/hn1k_bkhuprtsqgjjujb0zxjp-y.png)

Then find passengers who we want to exclude from the first set. This would be just a different service group:

[![](https://habrastorage.org/webt/ei/uh/yk/eiuhykjwibwzbclwltgnf4_jqk4.png)](https://habrastorage.org/webt/ei/uh/yk/eiuhykjwibwzbclwltgnf4_jqk4.png)

Now define merge rules – namely a condition on which we join two sets:

  
[![](https://habrastorage.org/webt/ve/o6/zt/veo6zt0ef1ku-hv4q39ttvpqtue.png)](https://habrastorage.org/webt/ve/o6/zt/veo6zt0ef1ku-hv4q39ttvpqtue.png)

Now the magic happens with Table Calculations:

{% highlight sql %}
{% raw %}

-- Any Journey in Business Service Group: 
NOT is_null(${q1_f_passengers_journeys.service_group})

-- # Passengers: 
sum(if(${any_journey_in_business_service_group}, 1, 0))

{% endhighlight %}
{% endraw %}

[![](https://habrastorage.org/webt/6q/de/5e/6qde5eetmovvdmerwths8-53gpk.png)](https://habrastorage.org/webt/6q/de/5e/6qde5eetmovvdmerwths8-53gpk.png)

And here is the result, each passenger is marked whether he has used service B or not, and total segment capacity is calculated:

[![](https://habrastorage.org/webt/wu/o4/ws/wuo4ws9_l4mpgkukrzdv_1zhpxg.png)](https://habrastorage.org/webt/wu/o4/ws/wuo4ws9_l4mpgkukrzdv_1zhpxg.png)

More in Looker docs on merged results: [Merging results from different Explores](https://docs.looker.com/exploring-data/exploring-data/merged-results).