# CS2210-Exam-2
Problem 2: Palindrome Substrings

Step 1. Define recursively
	Base case:
		Strings of length 1 are all palindromes

	Recursive case:
		For even length strings:
			The second half of the strings must be a mirror of the first half

		For odd length strings:
			The characters after the middle character must be a mirror of the characters before the middle character

		For all strings:
			Remove one of the characters in the string and recursively call the remaining substring, repeat for all characters in the string. (e.g. string ‘abc’, 	recursively call ‘bc’, then ‘ac’, then ‘ab’)

Step 2. Data Structures used

	For this problem a simple list/array data structure can be used. If a substring is found to be a palindrome, then it gets added. If the program encounters that substring again, then it and all substrings of that substring are skipped.

Step 3. IDEAL/Duke 7
	
	IDEAL:
		I: Identify the problem
			To identify, store, and output all substring of a given string which are palindromes, using dynamic programming 

		D: Define your goals
			Identify if the string is a palindrome
			Recursively call all substrings of that substring
			Return only the substrings which are palindrome
			Integrate dynamic programming through the use of data structures to store previously identified substrings
		E: Explore possible solutions
			The overall code was fairly easy to come up for this problem, but choosing what data structure to integrate took a bit longer. I originally used a list but that was becoming too tedious so I switched instead to a dictionary, which turned out to be easier to use
		A: Anticipate and Act
			Making sure to account for empty strings and strings where palindromes overlap is absolutely crucial. The first one is an easy fix, the second one though might take a bit of unusual coding.
		L: Look and Learn
Working with strings seems incredibly more complicated than working with other structures. So working with them may be necessary to improve my skills there
		
	Duke 7:
		1: Work small instance by hand
			Given input ‘aba’, ‘aba’ is a palindrome, ‘ab’ and ‘ba’ are not palindromes, and ‘a’, ‘b’, and ‘a’ are palindromes, so there are 4 palindromes in ‘aba’.
		2:  Write process
			Determine if string is a palindrome, then partition into smaller substrings, then repeat
		3: Find patterns
			If it is a palindrome, add to palindrome count. Recursively call each substring of the string.
		4: Check by hand
			String ‘aba’ returns 4, and string ‘abc’ return 3
		5: Write code
			Written
		6: Run test cases
			Run using ‘abc’, ‘ ’, and ‘aaa’. First two work, last one is having issues due to overlapping
		7: Debug
			Debugging in progress

Step 4: Code
def palindrome_substrings(S, D, shift=0):
    if len(S) < 1:
        return 0
    
    count = 0
    if D.search(S):
        info = D.read(S)
        if [info[0], info[1]] == [0+shift, (len(S)-1)+shift]:
            return 0
    
    if len(S) == 1:
        D.insert(S, [(0+shift), (len(S)-1)+shift, 1])
        return 1
    
    stack = []
    if len(S)%2 == 1:
        for j in range(len(S)//2):
            stack.append(S[j])
        
        for j in S[(len(S)//2)+1:]:
            if stack[-1] != j:
                break
            stack.pop(-1)
    else:
        for j in range(len(S)//2):
            stack.append(S[j])
        
        for j in S[(len(S)//2):]:
            if stack[-1] != j:
                break
            stack.pop(-1)
    
    count += palindrome_substrings(S[:-1], D, shift)
    count += palindrome_substrings(S[1:], D, shift+1)
    
    if len(stack) == 0:
        D.insert(S, [(0+shift), (len(S)-1)+shift])
        count += 1
    else:
        D.insert(S, [(0+shift), (len(S)-1)+shift])
        
    return count
    
In the Hash file:
  class Dictionary:
    def __init__(self,size):
        self.bucket = [[] for i in range(size)]
        
    def insert(self, n, data = None):
        if type(n) == str:
            key = 0
            for s in n:
                key = (key*511 + ord(s))%len(self.bucket)
        else:
            key = n%len(self.bucket)
        
        if data != None:
            if [n, data] not in self.bucket[key]:
                self.bucket[key].append([n, data])
                return 1
            else:
                return -1
            
        if n not in self.bucket[key]:
            self.bucket[key].append(n)
            return 1
        else:
            return -1
        
    def search(self, n):
        if type(n) == str:
            key = 0
            for s in n:
                key = (key*511 + ord(s))%len(self.bucket)
        else:
            key = n%len(self.bucket)
            
        for i in self.bucket[key]:
            if type(i) == int:
                if i == n:
                    return True
            else:
                if i[0] == n:
                    return True
        return False
    
    def read(self, n):
        if type(n) == str:
            key = 0
            for s in n:
                key = (key*511 + ord(s))%len(self.bucket)
        else:
            key = n%len(self.bucket)
            
        for i in self.bucket[key]:
            if i[0] == n:
                return i[1]
        return None
    
