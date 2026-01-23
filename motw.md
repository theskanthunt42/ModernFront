---
layout: page
title: MotW
permalink: /motw/
---
# Music (I looped) this Week.
Updates every Friday.(Hopefully)  
Mostly on Apple Music or YouTube.  
You could follow me on [Apple Music][am-profile] to see if theres something I didn't mentioned here but fits your tastes, and TOLD ME AND I WILL HAPPILY CHAT WITH YOU ABOUT IT.  

{% for post in site.posts %}
    {% if post.collections == "motw" %}
Last updated: {{ post.date | date_to_string: "ordinal", "US" }}
    {% endif %}
{% endfor %}


<ul>
  {% for post in site.posts %}
    {% if post.collections == "motw" %}
        <li>
            <a href="{{ post.url }}">Chart of Week {{ post.date | date: "%V" }} - {{ post.date | date_to_string: "ordinal", "US" }}</a>
        </li>
    {% endif %}
  {% endfor %}
</ul>

[am-profile]: https://music.apple.com/profile/the42game
