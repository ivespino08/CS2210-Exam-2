Problem 3: Arithmetic Slices

Step 1: Define recursively
	Base case:
		If the length of the slice is less than 3, return the list of arithmetic slices

	Recursive case:
		If the difference between the first two element in the slice is also the distance between all other consecutive elements in the slice, add it to the list of arithmetic slices

		Remove one of the elements of the slice and recursively call what remains, repeat for all elements in the slice (i.e. if slice is [1,2,3,4], call [2,3,4], [1,3,4],[1,2,4], and [1,2,3]
Step 2: Data Structure used
	For this problem a regular list will work fine. One we’ve determined if a slice is arithmetic or not, the slice is placed in the list, regardless of the answer. If we ever encounter that slice again we instantly return. If we’ve encountered that slice before, we have also encountered all its sub-slices.

Step 3: IDEAL/Duke 7
	IDEAL:
		I: Identify the problem
			To count the number of arithmetic slice in a list, and be able to identify repeat slice using dynamic programming
		D: Define your goals
			Identify if the list has an arithmetic difference
			Recursively call all slices contained in the list
			Return the number of arithmetic slices in the list
			Integrate dynamic programming through the use of data structures to store previously identified slices
		E: Explore possible solutions
			Overall, this was a really straightforward problem to solve but it did take me a bit to settle on using a list for dynamic programming. I wanted to use something more fancy like a Dictionary or matrix but a list proved to be the simplest solution
		A: Anticipate and Act
			Making sure to account for empty or too small slices, both are easily fixable
		L: Look and Learn
			Since the slices were list this problem wasn’t too hard to solve but remembering that sometimes simple doesn’t mean bad (as in the case of the list for dynamic programming)		
	Duke 7:
		1: Work small instance by hand
			Given input [1,2,3,4], [1,2,3,4] is an arithmetic slice, [1,2,4] and [1,3,4] are not arithmetic slices, and [1,2,3] and [2,3,4] are arithmetic slices, so there are 3 arithmetic slices in [1,2,3,4].
		2:  Write process
			Determine if slice has arithmetic difference, then partition into smaller slices, then repeat
		3: Find patterns
			If it is an arithmetic slice, add to the arithmetic slice count. Recursively call each sub-slice of the slice.
		4: Check by hand
			[1,2,3,4] returns 3, and [1,2,3] returns 1
		5: Write code
			Written
		6: Run test cases
			Run using [], [1,2,3,4,5], and [2,-1,-5,-7]. First two work, last one is having issues due to overlapping
		7: Debug
			Debugging complete
        
Step 4: Code
def arithemetic_slices(N, L):
    if len(N) < 3:
        return 0
    
    if N in L:
        return 0
    
    count = 0
    
    dif = N[1] - N[0]
    arith = True
    for i in range(len(N)-1):
        if N[i+1] - N[i] != dif:
            arith = False
    
    if arith:
        L.append(N)
        count += 1
    else:
        L.append(N)
    
    for i in range(len(N)):
        tmp = N.copy()
        tmp.pop(i)
        count += arithemetic_slices(tmp, L)
        
    return count
   
