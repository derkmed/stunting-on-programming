# Time Based Key-Value Store

<b>Question: Create a timebased key-value store class TimeMap, that supports two operations.

1. set(string key, string value, int timestamp)
  * Stores the key and value, along with the given timestamp.
2. get(string key, int timestamp)
  * Returns a value such that set(key, value, timestamp_prev) was called previously, with timestamp_prev <= timestamp.
  * If there are multiple such values, it returns the one with the largest timestamp_prev.
  * If there are no values, it returns the empty string ("").

Note:

All key/value strings are lowercase.
All key/value strings have length in the range [1, 100]
The timestamps for all TimeMap.set operations are strictly increasing.
1 <= timestamp <= 10^7
TimeMap.set and TimeMap.get functions will be called a total of 120000 times (combined) per test case.

</b>

Thought Process:
* We definitely need some sort of mapping data structure. How about a hash map?
* Now for each key, we can run binary search on the guaranteed sorted lists of timestamps to find the relevant value for that key

```python

class TimeMap:
    
    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.time_map = collections.defaultdict(list)
        
    def set(self, key: str, value: str, timestamp: int) -> None:
        self.time_map[key].append((timestamp, value))

    def get(self, key: str, timestamp: int) -> str:
        timestamps_values = self.time_map.get(key, None)
        if timestamps_values is None: return ""
        
        timestamps, values = zip(*timestamps_values)
        i = bisect.bisect(timestamps, timestamp)
        # the [1] is because this is a tuple!
        return timestamps_values[i-1][1] if i else ""

```

T = O(1) for set  
T = O(logn) for get  
S = O(N)  
