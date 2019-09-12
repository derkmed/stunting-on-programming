# Time Based Key-Value Store

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
