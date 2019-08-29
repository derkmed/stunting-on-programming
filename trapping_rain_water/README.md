# Trapping Rain Water 1





Thought Process:
* Each index is bounded by `min(max_on_right, max_on_left)`
* If we do a shrinking window pointer comparison between left and right pointers in which left < right, we observe the following two conditions:
     * case 1: `left < h` and `h < right` or `h > right`
     * case 2: `left > h`
# Trapping Rain Water 2
