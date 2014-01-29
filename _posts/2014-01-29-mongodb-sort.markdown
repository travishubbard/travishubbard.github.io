---
layout: post
title:  "MongoDB/Meteor sorting a result set "
date:   2014-01-29 08:30:00
categories: MongoDB sort in Meteor
---

Searching through the MongoDB docs regarding sorting yeilded the following:
{% highlight javascript %}
db.collection.find().sort( { age: -1 } );
{% endhighlight %}

which kept blowing up my Meteor Code. So I wrote a sort method on the Array:

{% highlight javascript %}
var sorted = customers.sort(function(a, b) {
    a = a.submitted;
    if(a==undefined){a = 0;}
    b = b.submitted;
    if(b==undefined){b = 0;}

    if(a > b)
        return -1;
    if(a < b)
        return 1;
    return 0;
});
return sorted;
{% endhighlight %}

Which left me scratching my head -- why can I not natively sort results? So after a run and some food, discovered the folliwng appears to be the correct way to sort:

{% highlight javascript %}
var sortedResultsArray = customers.find({appointments: {$gt : 0}}, 
{sort: {submitted: -1}}).fetch();
{% endhighlight %}

