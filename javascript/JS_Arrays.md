<!-- TOC -->

- [How do you flatten multi dimensional arrays](#how-do-you-flatten-multi-dimensional-arrays)
- [What is the easiest way to resize an array](#what-is-the-easiest-way-to-resize-an-array)
- [How do you reverse an array without modifying original array?](#how-do-you-reverse-an-array-without-modifying-original-array)
- [What are the differences between arguments object and rest parameter](#what-are-the-differences-between-arguments-object-and-rest-parameter)
- [What are the differences between spread operator and rest parameter](#what-are-the-differences-between-spread-operator-and-rest-parameter)
- [How to verify if a variable is an array?](#how-to-verify-if-a-variable-is-an-array)
- [What is the difference between dense and sparse arrays?](#what-is-the-difference-between-dense-and-sparse-arrays)
  - [What are the different ways to create sparse arrays?](#what-are-the-different-ways-to-create-sparse-arrays)
- [Slice, Splice 👇🏻](#slice-splice-)
  - [What is the purpose of the array slice method](#what-is-the-purpose-of-the-array-slice-method)
  - [What is the purpose of the array splice method](#what-is-the-purpose-of-the-array-splice-method)
  - [What is the difference between slice and splice](#what-is-the-difference-between-slice-and-splice)
- [How do you check whether an array includes a particular value or not](#how-do-you-check-whether-an-array-includes-a-particular-value-or-not)
- [How do you compare scalar arrays](#how-do-you-compare-scalar-arrays)
- [What is collation](#what-is-collation)
- [What is for...of statement](#what-is-forof-statement)
- [What is the output of below spread operator array](#what-is-the-output-of-below-spread-operator-array)
- [How do you combine two or more arrays](#how-do-you-combine-two-or-more-arrays)
- [What is the difference between Shallow and Deep copy](#what-is-the-difference-between-shallow-and-deep-copy)
- [What happens with negating an array](#what-happens-with-negating-an-array)
- [What happens if we add two arrays](#what-happens-if-we-add-two-arrays)
- [How do you remove falsy values from an array](#how-do-you-remove-falsy-values-from-an-array)
- [How do you get unique values of an array](#how-do-you-get-unique-values-of-an-array)
- [How do you map the array values without using map method](#how-do-you-map-the-array-values-without-using-map-method)
- [What is the easiest way to convert an array to an object](#what-is-the-easiest-way-to-convert-an-array-to-an-object)
- [How do you create an array with some data](#how-do-you-create-an-array-with-some-data)

<!-- /TOC -->

# How do you flatten multi dimensional arrays

Flattening bi-dimensional arrays is trivial with Spread operator.

```javascript
const biDimensionalArr = [11, [22, 33], [44, 55], [66, 77], 88, 99];
const flattenArr = [].concat(...biDimensionalArr); // [11, 22, 33, 44, 55, 6677, 88, 99]
```

But you can make it work with multi-dimensional arrays by recursive calls,

```javascript
function flattenMultiArray(arr) {
  const flattened = [].concat(...arr);
  return flattened.some((item) => Array.isArray(item))
    ? flattenMultiArray(flattened)
    : flattened;
}
const multiDimensionalArr = [11, [22, 33], [44, [55, 66, [77, [88]], 99]]];
const flatArr = flattenMultiArray(multiDimensionalArr); // [11, 22, 33, 44, 66, 77, 88, 99]
```
     
Also you can use the `flat` method of Array.
     
```javascript
const arr = [1, [2,3], 4, 5, [6,7]];
const fllattenArr = arr.flat(); // [1, 2, 3, 4, 5, 6, 7]
     
// And for multiDemensional arrays
const multiDimensionalArr = [11, [22, 33], [44, [55, 66, [77, [88]], 99]]];
const oneStepFlat = multiDimensionalArr.flat(1); // [11, 22, 33, 44, [55, 66, [77, [88]], 99]]
const towStep = multiDimensionalArr.flat(2); // [11, 22, 33, 44, 55, 66, [77, [88]], 99]
const fullyFlatArray = multiDimensionalArr.flat(Infinity); // [11, 22, 33, 44, 55, 66, 77, 88, 99]
```

# What is the easiest way to resize an array

The length property of an array is useful to resize or empty an array quickly. Let's apply length property on number array to resize the number of elements from 5 to 2,

```javascript
var array = [1, 2, 3, 4, 5];
console.log(array.length); // 5

array.length = 2;
console.log(array.length); // 2
console.log(array); // [1,2]
```

and the array can be emptied too

```javascript
var array = [1, 2, 3, 4, 5];
array.length = 0;
console.log(array.length); // 0
console.log(array); // []
```

# How do you reverse an array without modifying original array?

The `reverse()` method reverses the order of the elements in an array but it mutates the original array. Let's take a simple example to demonistrate this case,

```javascript
const originalArray = [1, 2, 3, 4, 5];
const newArray = originalArray.reverse();

console.log(newArray); // [ 5, 4, 3, 2, 1]
console.log(originalArray); // [ 5, 4, 3, 2, 1]
```

There are few solutions that won't mutate the original array. Let's take a look.

1. **Using slice and reverse methods:**
In this case, just invoke the `slice()` method on the array to create a shallow copy followed by `reverse()` method call on the copy.

```javascript
const originalArray = [1, 2, 3, 4, 5];
const newArray = originalArray.slice().reverse(); //Slice an array gives a new copy

console.log(originalArray); // [1, 2, 3, 4, 5]
console.log(newArray); // [ 5, 4, 3, 2, 1]
```

2. **Using spread and reverse methods:**
In this case, let's use the spread syntax (...) to create a copy of the array followed by `reverse()` method call on the copy.

```javascript
const originalArray = [1, 2, 3, 4, 5];
const newArray = [...originalArray].reverse();

console.log(originalArray); // [1, 2, 3, 4, 5]
console.log(newArray); // [ 5, 4, 3, 2, 1]
```

# What are the differences between arguments object and rest parameter

There are three main differences between arguments object and rest parameters

1. The arguments object is an array-like but not an array. Whereas the rest parameters are array instances.
2. The arguments object does not support methods such as sort, map, forEach, or pop. Whereas these methods can be used in rest parameters.
3. The rest parameters are only the ones that haven’t been given a separate name, while the arguments object contains all arguments passed to the function

# What are the differences between spread operator and rest parameter

Rest parameter collects all remaining elements into an array. Whereas Spread operator allows iterables( arrays / objects / strings ) to be expanded into single arguments/elements. i.e, Rest parameter is opposite to the spread operator.


# How to verify if a variable is an array?

It is possible to check if a variable is an array instance using 3 different ways,

`Array.isArray()` method:

The `Array.isArray(value)` utility function is used to determine whether value is an array or not. This function returns a true boolean value if the variable is an array and a false value if it is not.

```javascript
const numbers = [1, 2, 3];
const user = { name: 'John' };
Array.isArray(numbers);  // true
Array.isArray(user); //false
```


# What is the difference between dense and sparse arrays?

An array contains items at each index starting from first(0) to last(array.length - 1) is called as Dense array. Whereas if at least one item is missing at any index, the array is called as sparse.

     

```js
const avengers = ["Ironman", "Hulk", "CaptainAmerica"];
console.log(avengers[0]); // 'Ironman'
console.log(avengers[1]); // 'Hulk'
console.log(avengers[2]); // 'CaptainAmerica'
console.log(avengers.length); // 3

const justiceLeague = ["Superman", "Aquaman", , "Batman"];
console.log(justiceLeague[0]); // 'Superman'
console.log(justiceLeague[1]); // 'Aquaman'
console.log(justiceLeague[2]); // undefined
console.log(justiceLeague[3]); // 'Batman'
console.log(justiceLeague.length); // 4
```

    

## What are the different ways to create sparse arrays?


1. **Array literal:** Omit a value when using the array literal
   ```js
   const justiceLeague = ["Superman", "Aquaman", , "Batman"];
   console.log(justiceLeague); // ['Superman', 'Aquaman', empty ,'Batman']
   ```
2. **Array() constructor:** Invoking Array(length) or new Array(length)
   ```js
   const array = Array(3);
   console.log(array); // [empty, empty ,empty]
   ```
3. **Delete operator:** Using delete array[index] operator on the array
   ```js
   const justiceLeague = ["Superman", "Aquaman", "Batman"];
   delete justiceLeague[1];
   console.log(justiceLeague); // ['Superman', empty, ,'Batman']
   ```
4. **Increase length property:** Increasing length property of an array
   ```js
   const justiceLeague = ['Superman', 'Aquaman', 'Batman'];
   justiceLeague.length = 5;
   console.log(justiceLeague); // ['Superman', 'Aquaman', 'Batman', empty, empty]
   ```
	

# Slice, Splice 👇🏻

## What is the purpose of the array slice method

The **slice()** method returns the selected elements in an array as a new array object. It selects the elements starting at the given start argument, and ends at the given optional end argument without including the last element. If you omit the second argument then it selects till the end.

Some of the examples of this method are,

```javascript
   let arrayIntegers = [1, 2, 3, 4, 5];
   let arrayIntegers1 = arrayIntegers.slice(0, 2); // returns [1,2]
   let arrayIntegers2 = arrayIntegers.slice(2, 3); // returns [3]
   let arrayIntegers3 = arrayIntegers.slice(4); //returns [5]
```

**Note:** Slice method won't mutate the original array but it returns the subset as a new array.

  

## What is the purpose of the array splice method

The **splice()** method is used either adds/removes items to/from an array, and then returns the removed item. The first argument specifies the array position for insertion or deletion whereas the optional second argument indicates the number of elements to be deleted. Each additional argument is added to the array.

   Some of the examples of this method are,

   ```javascript
   let arrayIntegersOriginal1 = [1, 2, 3, 4, 5];
   let arrayIntegersOriginal2 = [1, 2, 3, 4, 5];
   let arrayIntegersOriginal3 = [1, 2, 3, 4, 5];

   let arrayIntegers1 = arrayIntegersOriginal1.splice(0, 2); // returns [1, 2]; original array: [3, 4, 5]
   let arrayIntegers2 = arrayIntegersOriginal2.splice(3); // returns [4, 5]; original array: [1, 2, 3]
   let arrayIntegers3 = arrayIntegersOriginal3.splice(3, 1, "a", "b", "c"); //returns [4]; original array: [1, 2, 3, "a", "b", "c", 5]
   ```

   **Note:** Splice method modifies the original array too and returns the deleted array.

  

## What is the difference between slice and splice

Some of the major difference in a tabular form

| Slice                                        | Splice                                          |
| -------------------------------------------- | ----------------------------------------------- |
| Doesn't modify the original array(immutable) | Modifies the original array(mutable)            |
| Returns the subset of original array         | Returns the deleted elements as array           |
| Used to pick the elements from array         | Used to insert or delete elements to/from array |


# How do you check whether an array includes a particular value or not

The `includes()` method is used to determine whether an array includes particular value among its entries by returning either true or false.

```javascript
var numericArray = [1, 2, 3, 4];
console.log(numericArray.includes(3)); // true
var stringArray = ["green", "yellow", "blue"];
console.log(stringArray.includes("blue")); //true
```

    

# How do you compare scalar arrays

You can use length and every method of arrays to compare two scalar(compared directly using ===) arrays. The combination of these expressions can give the expected result,

```javascript
const arrayFirst = [1, 2, 3, 4, 5];
const arraySecond = [1, 2, 3, 4, 5];
console.log(
  arrayFirst.length === arraySecond.length &&
    arrayFirst.every((value, index) => value === arraySecond[index])
); // true
```

If you would like to compare arrays irrespective of order then you should sort them before,

```javascript
const arrayFirst = [2, 3, 1, 4, 5];
const arraySecond = [1, 2, 3, 4, 5];
console.log(
  arrayFirst.length === arraySecond.length &&
    arrayFirst.sort().every((value, index) => value === arraySecond[index])
); //true
```

# What is collation

Collation is used for sorting a set of strings and searching within a set of strings. It is parameterized by locale and aware of Unicode. Let's take comparison and sorting features,

1. **Comparison:**

 ```javascript
 var list = ["ä", "a", "z"]; // In German,  "ä" sorts with "a" Whereas Swedish, "ä" sorts after "z"
 var l10nDE = new Intl.Collator("de");
 var l10nSV = new Intl.Collator("sv");
 console.log(l10nDE.compare("ä", "z") === -1); // true
 console.log(l10nSV.compare("ä", "z") === +1); // true
 ```

2. **Sorting:**

```javascript
var list = ["ä", "a", "z"]; // In German,  "ä" sorts with "a" Swedish, "ä" sorts after "z"
var l10nDE = new Intl.Collator("de");
var l10nSV = new Intl.Collator("sv");
console.log(list.sort(l10nDE.compare)); // [ "a", "ä", "z" ]
console.log(list.sort(l10nSV.compare)); // [ "a", "z", "ä" ]
```

# What is for...of statement

The for...of statement creates a loop iterating over iterable objects or elements such as built-in String, Array, Array-like objects (like arguments or NodeList), TypedArray, Map, Set, and user-defined iterables. The basic usage of for...of statement on arrays would be as below,

```javascript
let arrayIterable = [10, 20, 30, 40, 50];

for (let value of arrayIterable) {
  value++;
  console.log(value); // 11 21 31 41 51
}
```

# What is the output of below spread operator array

```javascript
[..."John Resig"];
```

The output of the array is ['J', 'o', 'h', 'n', '', 'R', 'e', 's', 'i', 'g']
**Explanation:** The string is an iterable type and the spread operator within an array maps every character of an iterable to one element. Hence, each character of a string becomes an element within an Array.


# How do you combine two or more arrays

The concat() method is used to join two or more arrays by returning a new array containing all the elements. The syntax would be as below,

```javascript
array1.concat(array2, array3, ..., arrayX)
```

Let's take an example of array's concatenation with veggies and fruits arrays,

```javascript
var veggies = ["Tomato", "Carrot", "Cabbage"];
var fruits = ["Apple", "Orange", "Pears"];
var veggiesAndFruits = veggies.concat(fruits);
console.log(veggiesAndFruits); // Tomato, Carrot, Cabbage, Apple, Orange, Pears
```

# What is the difference between Shallow and Deep copy

There are two ways to copy an object,

**Shallow Copy:**
Shallow copy is a bitwise copy of an object. A new object is created that an exact copy of the values in the original object. If any of the fields of object are references to other objects, just the reference addresses are i.e., only the memory address is copied.

    

```javascript
var empDetails = {
  name: "John",
  age: 25,
  expertise: "Software Developer",
};
```

to create a duplicate

```javascript
var empDetailsShallowCopy = empDetails; //Shallow copying!
```

if we change some property value in the duplicate one like this:

```javascript
empDetailsShallowCopy.name = "Johnson";
```

The above statement will also change the name of `empDetails`, since we have a shallow copy. That means we're losing the original data as well.

**Deep copy:**
A deep copy copies all fields, and makes copies of dynamically allocated memory pointed to by the fields. A deep copy occurs when an object is copied along with the objects to which it refers.

    

```javascript
 var empDetails = {
   name: "John",
   age: 25,
   expertise: "Software Developer",
 };
```

Create a deep copy by using the properties from the original object into new variable

```javascript
var empDetailsDeepCopy = {
  name: empDetails.name,
  age: empDetails.age,
  expertise: empDetails.expertise,
};
```

Now if you change `empDetailsDeepCopy.name`, it will only affect `empDetailsDeepCopy` & not `empDetails`

# What happens with negating an array

Negating an array with `!` character will coerce the array into a boolean. Since Arrays are considered to be truthy So negating it will return `false`.

```javascript
console.log(![]); // false
```

# What happens if we add two arrays

If you add two arrays together, it will convert them both to strings and concatenate them. For example, the result of adding arrays would be as below,

```javascript
console.log(["a"] + ["b"]); // "ab"
console.log([] + []); // ""
console.log(![] + []); // "false", because ![] returns false.
```



# How do you remove falsy values from an array

You can apply the filter method on the array by passing Boolean as a parameter. This way it removes all falsy values(0, undefined, null, false and "") from the array.

```javascript
const myArray = [false, null, 1, 5, undefined];
myArray.filter(Boolean); // [1, 5] // is same as myArray.filter(x => x);
```

# How do you get unique values of an array

You can get unique values of an array with the combination of `Set` and rest expression/spread(...) syntax.

```javascript
console.log([...new Set([1, 2, 4, 4, 3])]); // [1, 2, 4, 3]
```

# How do you map the array values without using map method

You can map the array values without using the `map` method by just using the `from` method of Array. Let's map city names from Countries array,

```javascript
const countries = [
  { name: "India", capital: "Delhi" },
  { name: "US", capital: "Washington" },
  { name: "Russia", capital: "Moscow" },
  { name: "Singapore", capital: "Singapore" },
  { name: "China", capital: "Beijing" },
  { name: "France", capital: "Paris" },
];

const cityNames = Array.from(countries, ({ capital }) => capital);
console.log(cityNames); // ['Delhi, 'Washington', 'Moscow', 'Singapore'Beijing', 'Paris']

```

# What is the easiest way to convert an array to an object

You can convert an array to an object with the same data using spread(...) operator.

```javascript
var fruits = ["banana", "apple", "orange", "watermelon"];
var fruitsObject = { ...fruits };
console.log(fruitsObject); // {0: "banana", 1: "apple", 2: "orange", 3"watermelon"}
```

# How do you create an array with some data

You can create an array with some data or an array with the same values using `fill` method.

```javascript
var newArray = new Array(5).fill("0");
console.log(newArray); // ["0", "0", "0", "0", "0"]
```