# CompSci202_Lab8
Data Structures and Algorithms | Colgate University | Lab 8 

LAB 8 WRITEUP:

Jackson Carter and Ethan Rackleff

SPECIFICATION: At the core, we are trying to solve the solution for the final line. We need to check, while length of that supposed line is less than the maximum length what combinations will minimize slack. We know we need to return the final word in the array. The problem itself is based on a relationship between all the lines and attempting to minimize slack as a whole. If one word changes several other words will need to change. 

BASE CASE:

if L = 0 || n.length = 0 

return null;

//n represents the input array, L represents the max number of characters per line

//also, if some word has length greater than max width of a line, return null.

RECURSION

```java
Let S(x) be the slack function (excess spaces -> slack, e.g. x^2....)
Let f(k....n) = L - (w[k] + w[k+1] + ... + w[n] + n - k) for some k <= n
So f gives num of spaces left in some line

								-->if can add to cur line: Slack(n - 1) - S(w[n] + 1)
Slack(n) = min: -->Slack(n - 1) + S(f(n))
							  -->Slack(n - 2) + S(f(n-1...n)) 
							  -->....
							  -->Slack(k - 1) + S(f(k... n))
							for k<=n and f(k.....n) >= 0
For each min Slack solution, store the last word indices in an array. 
Then adjust this array depending on what min is (add last word to soln from
slack or if on same line replace last entry in soln with new last word in line
```

So in the above, a min slack for some number of words is the minimum out of the options of moving the last word plus some number of words before it (until the new line exceeds the max width) + the slack of the solution from the word before the first word in this new line. The recursion calls the same subproblem multiple times, so we want to create a dynamic programming approach to solving this:

Store Slack(0), Slack(1), Slack(2) … Slack(n - 1) in an array

- By building up to Slack(n) by solving all of the previous problems and storing the results, we only have to solve each possible subproblem once.
- Store the min slack calculated for Slack(i), i < n in an array, and also store a respective aux array as satellite data (either in another array or use nodes) in order to track the last indices on each line for each solution.
- Once the min for Slack(i) is found, we just take the satellite data from the previous subproblem (Slack(i-k)) and either add the latest word to the end (if we go to a new line in optimal solution) or replace the last word (if we stay on same line).

Justification: The recurrence creates an optimal solution by using the idea that adding a new word is at most going to add a single line, so a previous optimal solution, plus the slack whatever words we pull down to this new line, will be a optimal solution. By memoizing the problem, we can then greatly reduce the running time (logic remains the same, but we can write the algorithm iteratively instead of recursively).

Time Complexity: O(n^2) → We have to build the length n array of subproblem solutions, of which each subproblem takes O(n) time (has to access some k < n entries in array [O(1)] and building the slack of the new line is O(1) in each subproblem since we can just track it and add to it each iteration).

Space Complexity: O(n^2) → We store the indices of last words for each subproblem in aux arrays. If each subproblem has every word on its own line, then the length of each aux array will be 1, 2, 3, …, n = O(n^2).
