# Three Num Sum


## Learning Objective
* Apply previous solutions to increasingly complex problems


## Interviewer Prompt
Given an array of distinct integers and an integer representing a target sum, write a function that returns an array of all triplets in the input array that sum to the target sum.


### Examples

```javascript
arrayThreeSum([12, 3, 1, 2, -6, 5, -8, 6], 0)   //should return [[-8, 2, 6], [-8, 3, 5], [-6, 1, 5]]
arrayThreeSum([5, 6 , 1, -9 , 7, 3 , 2], 35)    //should return []
arrayThreeSum([1, 15, -5, 12 , -3, 6 , 2], 10)  //should return [[ -3, 1, 12 ]]
```

## Interviewer Strategy Guide
Let's think about time complexity.  If your interviewee goes down the path of trying all possible combintations of three integers (i.e. a triple for-loop that is doubly nested), that will have a time complexity of O(n^3).  Encourage them to find a more optimal approach.

### Hints
- Feel free to point out that the triplets themselves are arrays since this may not be obvious to the interviewee. This means that the function will return an array of arrays.
- The input array is NOT sorted. Unless specified otherwise, we _can_ sort the input array and then use that sorted array to help us optimize the solution.
- Suggest to the interviewee to think of how they would handle optimizing this problem if they only had to sum two integers. Three num sum can be thought of as Pair Sum inside a for-loop.

## Solutions

**Pointers Solution**: O(n^2) time complexity, O(n) space complexity
- This solution is very similar to one of the solutions from Pair Sum.
- Array.prototype.sort() has a time complexity of O(nlog(n)). The reason this solution has a time complexity of O(n^2) is because O(n^2) dwarfs O(nlog(n)), allowing us to drop the latter from the time complexity analysis.
```javascript
function arrayThreeSum(arr, targetSum){

  //sorts the input arr from least to greatest
  arr.sort((a, b) => a-b)

  const solution = []

  for (let i = 0; i < arr.length-2; i++){
    let element = arr[i]
    let leftIndex = i + 1
    let rightIndex = arr.length - 1

    //for each element in the array check to see if any two other integers in the array add to the target sum
    while(leftIndex < rightIndex){
      let currentSum = element + arr[leftIndex] + arr[rightIndex]

      //if the currentSum is equal to the target sum add an array of those 3 integers to the solution array
      if(currentSum === targetSum){
        solution.push([element, arr[leftIndex], arr[rightIndex]])
        leftIndex ++
        rightIndex --
      } else if(currentSum > targetSum){
        rightIndex --
      } else if (currentSum < targetSum){
        leftIndex ++
      }
    }
  }
  return solution
}
```


**Memoization Solution**: O(n^2) time complexity, O(n) space complexity
- Note that this solution does not return a sorted solution like the one above.

```javascript
function arrayThreeSum(arr, targetSum){
  const solution = []
  for(let i = 0; i< arr.length-1; i++){
    //the sum needed given we already know one element arr[i]
    let currentSum = targetSum - arr[i]
    //create a hash table to store all of the integers from arr[i] we have tried
    let memo ={}
    for (let j = i+1; j < arr.length; j++){
      if(memo[currentSum-arr[j]]){
        solution.push([arr[i], currentSum-arr[j], arr[j]])
      }else{
        memo[arr[j]] = true
      }
    }
  }
  return solution
}
```

## Extra Resources
* [Solution Slides](https://docs.google.com/presentation/d/1PbP_bQ-6Y6FeOEnMlmgfT5_h0ssPq7QGbeAZq4kuAiw/edit?usp=sharing)
* [AlgoExpert Link](https://www.algoexpert.io/questions/Three%20Number%20Sum)
