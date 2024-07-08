A node to discuss changing the Labels plugin by moving to a concept of torrent groups and tags.

## Groups

* a new level of control for torrents
* sits between global and per-torrent settings
* defines bandwidth settings
* should it also define move completed locations?
* can a torrent belong to multiple groups?

## Tags

* tags torrents based on pattern matching
  * can match against tracker or filenames
* a single torrent can have multiple tags
  * this is going to require a bit of thinking
* by itself tags just allow easier filtering

## Organiser Plugin

* tag-based organiser
  * rules are based on tags
* allows for moving torrents on completion, possibly renaming / restructuring
* should it handle linking tags <-> groups or should this be done in the core?
  * organiser could simply link a tag to a group to achieve custom move completed paths