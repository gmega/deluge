# Import Plugin

*N.B. This is just a collection of information.*

## Transmission
Transmission stores its state in bencoded files with the following format:

```python
{
 'activity-date': 1287828153,
 'added-date': 1242304732,
 'bandwidth-priority': 0,
 'corrupt': 0,
 'destination': '/mnt/disk1/transmission/',
 'dnd': [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
 'done-date': 1242335699,
 'downloaded': 2469147918,
 'max-peers': 75,
 'paused': 0,
 'peers2': '[binarydata]',
 'peers2-6': '[binarydata]',
 'priority': [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
 'progress': {
  'bitfield': '[binarydata]'
  'have': 'all',
  'mtimes': [
   1242335303,
   1242305325,
   1242335699,
   1242305892,
   1242335674,
   1242305529,
   1242334056,
   1242310169,
   1242335397,
   1242305771,
   1242334539,
   1242307206,
   1242333740,
   1242309147,
   1242333592,
   1242305446
  ]
 },
 'ratio-limit': {
  'ratio-limit': '2.000000',
  'ratio-mode': 0
 },
 'speed-limit-down': {
  'speed': 100,
  'use-global-speed-limit': 1,
  'use-speed-limit': 0
 },
 'speed-limit-up': {
  'speed': 500,
  'use-global-speed-limit': 1,
  'use-speed-limit': 0
 },
 'uploaded': 2010821546335
}
```