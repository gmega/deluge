# Priorities

This plugin is only compatible with the **0.5.x** and **1.1.x** releases of deluge. 

This plugin allows you to assign a tracker to on of 5 upload-slots.
These slots can have a different share of the available upload-bandwidth.

The shares stand in relation to each other. That means, if you have one tracker assigned with the value 99 and one with 98 and both have active uploading torrents, these tracker will both have nearly 50% of the bandwidth for their torrents.
So you can assign smaller numbers as 1 for tracker a, and 3 for tracker b. This will result in 75% of the bandwidth for tracker b and 25% for tracker a.

The available bandwidth for each tracker will also be split by the number of active torrents.

I hope you enjoy it. Feel free to contact me about bugs or imporvements (It's my first python project)
