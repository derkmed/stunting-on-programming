# Text Justification

<b>Question: Given an array of strings words and a width maxWidth, format the text such that each line has exactly maxWidth characters and is fully (left and right) justified.</b>

<b>You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces ' ' when necessary so that each line has exactly maxWidth characters.</b>

<b>Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line does not divide evenly between words, the empty slots on the left will be assigned more spaces than the slots on the right.</b>

<b>For the last line of text, it should be left-justified and no extra space is inserted between words.</b>


<b>Note:</b>
* A word is defined as a character sequence consisting of non-space characters only.
* Each word's length is guaranteed to be greater than 0 and not exceed maxWidth.
* The input array `words` contains at least one word.



Thought Process:
* Let's keep a buffer for the current line
* Whenever adding a new word will breach the maxWidth, it's time to append the justified version of the current buffer
* Append spaces to each word in the current line buffer, round-robin style left-to-right
* Simple left justification of the final line at the end


```
class Solution:
    def fullJustify(self, words: List[str], maxWidth: int) -> List[str]:
        def _just(currLine: [], nLetters: int):
            # round-robin allocation of spaces
            for i in range(maxWidth - nLetters):
                if len(currLine) == 1:
                    currLine[0] += ' '
                else:
                    currLine[i % (len(currLine) - 1)] += ' '
            return ''.join(currLine)
            
        
        result = []
        currLine, nLetters = [], 0
        for w in words:
            if nLetters + len(w) + len(currLine) > maxWidth:
                # adding this word will lead  to too many letters, so append curr line (justified)
                justified_line = _just(currLine, nLetters)
                result.append(justified_line)
                currLine, nLetters = [], 0
            currLine += [w]
            nLetters += len(w)
            
        # left justify the last line
        result.append(' '.join(currLine) + ' ' * (maxWidth - nLetters - len(currLine) + 1))
        return result
            
```

T = O(N * maxWidth)
S = O(N * maxWidth)
