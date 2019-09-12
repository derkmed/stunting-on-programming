# Time Based Key-Value Store

<b>Question: Create a timebased key-value store class TimeMap, that supports two operations.

1. set(string key, string value, int timestamp)
  * Stores the key and value, along with the given timestamp.
2. get(string key, int timestamp)
  * Returns a value such that set(key, value, timestamp_prev) was called previously, with timestamp_prev <= timestamp.
  * If there are multiple such values, it returns the one with the largest timestamp_prev.
  * If there are no values, it returns the empty string ("").
</b>

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
