# The Ultimate Java Refresher (for interviews)

![alt text](https://hips.hearstapps.com/cos.h-cdn.co/assets/16/22/1464842047-screen-shot-2016-06-01-at-93024-pm.png?fill=320:232&resize=320:*)

Hi, and welcome. I heard you would like to learn how to Java. I am here to teach
you how to Java.

The first step to learning Java, is not to use it. Goodbye!

If only it was that easy... Well, if I had any good guess, you're prepping for
that big interview, lacking hours of sleep, hopped up on blackberries, and just
need to know enough Java for the interviewer not to hate you. You're in the
right place! Maybe...idk if I'm even in the right place.

## Chapter 1: Methods

In Java, every "block" of code you write to serve some purpose must be written
inside of a _method_. If, during your interview, you just need to write an
algorithm and aren't designing a data structure, you can assume your method(s)
will be both `public` and `static`.

Example:
```java
public static int sumArray(int[] nums) {
  int sum = 0;
  for (int x : nums) {
    sum += x;
  }
  return x;
}
```

Recall that in Java, every method we define must be defined within a class, so
the above code wouldn't actually compile. But, for the purpose of the interview,
with a question like this, we can omit the class.

The other situation we might define methods, is when designing a data structure
or the blueprint for some kind of object. In this case, we have to pay more
attention to what the interviewer asks for.

For example, say the interviewer wants you to design a data structure called
`BouncyBinarySearchTree` which supports the following operations: `insert()`,
`bounce()`, and `search()`. Clearly, when designing our class, we would want
to expose these as public methods to the users of our class, so we should make
them `public`. We are also free to define any other methods we might find
useful, but we need to make sure to keep them `private`.

Example:
```java
/* A very, very bouncy binary search tree */
public class BouncyBinarySearchTree {
  public void insert(int x) {
    // TODO: insert x into the tree.
  }

  public void bounce() {
    // TODO: bounce the tree (whatever that means).
  }

  public boolean search(int x) {
    // TODO: determine if x exists in the tree.
  }

  private boolean searchHelper(int x, int l, int u) {
    // TODO: write this helper method for search().
  }
}
```

## Chapter 2: Classes

There's really not much to say here. I would suggest following these style
guidelines for writing classes:

```java
public class ClassName {
  // Public member variables.
  // Private member variables.

  // ClassName constructor.

  // Public methods.
  // Private methods.
}
```

As far as access modifiers for member variables go, for small objects such as a
`Node` in a linked list, it's fine to leave member variables as `public` so you
can access them easily like `node.val` and `node.next`, but for more intricate designs like our `BouncyBinarySearchTree` you should make all member variables
`private` when you can and write methods to access them.

## Chapter 3: Arrays

First of all it's important to know how to create an array:
```java
type[] array = new type[length];
```
where `type` is the type of the elements which can be stored in your array such as
`int` or `boolean` or `Monkey`, and `length` is the number of elements which
which will appear in your array.

In most cases, you'll probably prefer to use an `ArrayList` over an array, but
many problems will require you to operate on an array.

The only property of an array that you'll need to know is `length`.

With arrays, come a bunch of utility methods defined in the built-in `Arrays`
class. These methods are `static`, so you can call them freely without needing
an instance of the class itself.

Here are some of the most useful ones to remember:
```java
Arrays.asList(myArray) // Converts your array into a List.
Arrays.sort(myArray); // Super useful for problems that require you to sort.
Arrays.copyOfRange(myArray, i, j); // Makes a copy of all elements from i to j.
```

## Chapter 4: Strings

In many problems, Java strings really suck to work with, so it's often useful
to convert a string to some other type of object such as an array or a
`StringBuilder`.

The general guideline is this:

If you find that you need to modify your string or move characters around, you
should convert it into an array. For example, say you want to sort a string:
```java
public static String sortString(String str) {
  char[] characters = str.toCharArray();
  Arrays.sort(characters);
  return new String(characters);
}
```

If you find yourself, _building_ a string by using the built-in `+` operator
repeatedly, you should be using a `StringBuilder` instead. Consider the
following method which replaces all spaces in a string with the characters
`%20` (something that actually happens when you type a URL with spaces into your
internet browser), a common approach might be:

```java
public static String URLify(String url) {
  String correctURL = "";
  for (char c : url.toCharArray()) {
    if (c == ' ') {
      correctURL += "%20";
    } else {
      correctURL += c;
    }
  }
  return correctURL;
}
```

Doesn't look too bad, however each time we append to our string `correctURL`
using the `+` operator, we are making an entirely new copy of the string. This
is pretty costly and slows down our seemingly `O(n)` algorithm to `O(n^2)`.

Instead, we should use a `StringBuilder` which supports constant time append:

```java
public static String URLify(String url) {
  // This is exactly like creating an empty String.
  StringBuilder correctURLBuilder = new StringBuilder();
  for (char c : url.toCharArray()) {
    if (c == ' ') {
      correctURL.append("%20");
    } else {
      correctURL.append(c);
    }
  }
  // Remember to convert back into a String.
  return correctURL.toSting();
}
```

This is not to say the built-in `String` class doesn't provide some useful
tools. It does:

```java
myString.substring(1,3) // Returns the substring from index 1 to 3.
myString.replaceAll(" ", ","); // Returns a string with all spaces replaced with commas.
myString.indexOf('.'); // Returns the index of the first '.' character.
myString.split(" "); // Returns an array of strings that were separated by a space in the original string.
// Ex. "hello world of java".split(" ") => ["hello", "world", "of", "java"]
```

## Chapter 5: ArrayList

`ArrayList` gives you the benefit of accessing elements like an array
and dynamically adding elements like a list.

There are three ways to instantiate an `ArrayList`, but I'm only going to
mention two of them:
```java
// Create an empty ArrayList.
ArrayList<T> al1 = new ArrayList<T>();

// Create an ArrayList using the elements from some other collection.
ArrayList<T> al2 = new ArrayList<T>(someOtherCollection);
```
where `T` is the type of the objects which will be stored in the `ArrayList`,
such as `Integer`, `Spaceship`, `Pair`, or even other instances of `ArrayList`
(yup! an ArrayList of ArrayLists!).

`Collection` is an interface that a lot of built-in Java classes implement such
as `ArrayList`, `HashSet`, `HashMap`, `LinkedList`, `LinkedHashSet`, etc.

So we can create an `ArrayList` out of virtually any other "collection" of data.

A common use case might be to create an `ArrayList` using the data from some
fixed-size array:

```java
int[] arr = {1, 3, 5, 6};
ArrayList<Integer> arrList = new ArrayList<Integer>(Arrays.asList(arr));
```

Or maybe we would like to add all of the keys from our `HashSet` to an
`ArrayList`:
```java
HashSet<Integer> setOfInts = new HashSet<Integer>();
setOfInts.insert(3);
setOfInts.insert(5);
setOfInts.insert(6);
ArrayList<Integer> nums = new ArrayList<Integer>(setOfInts);
```

There are a few operations which you should remember for `ArrayList`
objects:

```java
myArrayList.add(element); // Adds the element to the end of the list.
myArrayList.addAll(elements); // Adds a collection of elements to the end of the list.
myArrayList.get(3); // Get's the element at index 3.
myArrayList.remove(element); // Remove's the first occurrence of element from the list.
myArrayList.set(3, element); // Places the element at index 3.
myArrayList.clear(); // Removes all elements from the list.
myArrayList.indexOf(element); // Returns index of first occurrence of element.
```

## Chapter 6: HashSet and HashMap

There are two main hashing data structures that you need to know about in Java,
`HashMap` and `HashSet`. Both of these implement the `Collection` interface.

A `HashSet` can be used to store only keys (or elements). For example, imagine
you're implementing the game Hangman. You might want to keep a `HashSet` of
letters that the user has already used so that you can easily check if a letter
has been used when a user chooses it. The method for choosing a letter might
look like this:

```java
/**
 * Returns a list of indices from the hidden word which match the provided letter.
 * @param letter The letter chosen by the user.
 * @param hiddenWord The word the user is trying to guess.
 * @param usedLetters A set of letters that the user has already used.
 * @return An ArrayList of indices or null if the letter has already been used.
 */
public ArrayList<Integer> guessLetter(char letter, String hiddenWord, HashSet<Character> usedLetters) {
  // Check if the letter has already been chosen.
  if (usedLetters.contains(letter)) {
    return null;
  }
  usedLetters.insert(letter);
  ArrayList<Integer> matchingIndices = new ArrayList<Integer>();
  for (int i = 0; i < hiddenWord.length(); i++) {
    if (hiddenWord[i] == letter) {
      matchingIndices.add(i);
    }
  }
  return matchingIndices;
}
```

`HashSet` doesn't have that many important methods to remember:
```java
myHashSet.insert(x); // Inserts x into the set.
myHashSet.contains(x); // Checks if x exists in the set.
myHashSet.remove(x); // Removes x from the set.
myHashSet.clear(); // Removes all elements from the set.
myHashSet.isEmpty(); // Checks if the set is empty.
```

A `HashMap` is very similar to a `HashSet` in that we are storing keys, and can
access those keys in constant time, but we are also able to store a value with
each key. We call these (key, value) pairs.

For example, we may want to create a list for each unique character in a string
storing the indices that the character appears at in the string. Our method
might look like this:

```java
public ArrayList<ArrayList<Integer>> getIndexLists(String str) {
  // Create a HashMap that maps character's to their corresponding index list.
  HashMap<Character, ArrayList<Integer>> indexListMap;

  for (int i = 0; i < str.length(); i++) {
    char c = str[i];
    // Create a new index list if the character does not exist in the hash map.
    if (!indexListMap.containsKey(c)) {
      // Map c to an empty list
      indexListMap.put(c, new ArrayList<Integer>());
    }
    // Add the index to c's index list.
    indexListMap.get(c).add(i);
  }

  // Convert the HashMap to an ArrayList
  return new ArrayList(indexListMap);
}
```

Again, there are not that many methods to remember for `HashMap` either:
```java
myHashMap.put(k, v); // Inserts the key k with the value v.
myHashMap.get(k); // Return the value associated with the key k.
myHashMap.containsKey(k); // Checks the key k exists in the hash map.
myHashMap.remove(k); // The key k (and its value) from the hash map.
myHashMap.clear(); // Removes all elements from the hash map.
myHashMap.isEmpty(); // Checks if the hash map is empty.
```

## Chapter 7: Collections

Like the `Arrays` class, the `Collections` class provides utility methods
for operating on collections of data (that aren't arrays):


```java
Collections.min(myCollection); // Returns the minimum element.
Collections.max(myCollection); // Returns the maximum element.
Collections.sort(myCollection); // Sorts the collections.
Collections.emptyList(); // Returns an empty list (can be used as an ArrayList).
Collections.reverse(myCollection); // Reverses a collection (only for lists).
Collections.shuffle(myCollection); // Randomly orders a collection (only a list).
```
