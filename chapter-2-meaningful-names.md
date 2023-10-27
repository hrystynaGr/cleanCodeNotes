## Chapter 2: Meaningful Names

This chapter teaches us to choose names wisely, considering future developers who may work with our code. The author provides a few rules on how to select appropriate names.

**Use Intention-Revealing Names**
From the name of a function or variable, you should have a general idea of what is happening. For example, this piece of code is hard to understand:

```
function getThem(obj) {
    return obj.filter((e)=> e.status === 4)
}
```
You can see that we are filtering an object and retaining only items with the value 4. But what are we filtering? Why does an item need to have a value of 4? What is happening? Let's change the code to answer these questions:

```
function isFlagged(song) {
    return song.status === 4;
}

function getFlaggedSongs(songs) {
    return songs.filter((e)=> isFlagged(e))
}
```
Now we know that "4" means the status for flagged songs, and we are retrieving all flagged songs from the list of songs.

**Avoid Disinformation**
Do not use the name `retrieveFromArray` if the function retrieves from any object. Do not use the name `cardsSet" unless you specifically mean the actual "Set" data type. Never use "O" or "l" for single-character names, as they look like zero and one and could lead to significant confusion.

**Avoid Single Character Names**
You can use them for cycles or for simple variables in a callback function, but do not use them for anything else.

**Do Not Use Different But Similar Names**
1. Word Noise: Those three names mean exactly the same thing—`getActiveAccount(); getActiveAccountData(); getActiveAccountInfo()`. So, it is impossible to understand which one does what. Better namings would be `getActiveAccountId(); getActiveAccountData(); getActiveAccountCreationDate();`.
2. Long Names with Small Differences: How long does it take to find the difference between these names? `getAccountFromManagersListAndPutItToXYZClass()`, `getAccountFromManagersListAndPutItToXYZKClass()`.
3. Misspelled Names: Sometimes you can find yourself in a situation where you end up with two variables with the same name, so to solve this situation, you will misspell one of them. For example, "accounts" and "acounts." Then another developer sees "acounts" and wants to change it to the correct spelling. What will happen? Right, the code will not interpret anymore because your coworker created one variable from two different variables. And we couldn't blame him less.

**Use Pronounceable Names**
Imagine you want to ask your colleague a question about this function:

```
funtion dmhAcctsOnFz(acc) {
    return acc.dmhAcctsOnFzDt;
}
```
So your question will sound like, "Hey Mike, do you know what 'dy-em-ejch a-k-k-t-s on fi-ez' returns?" There is no chance he will understand what you're talking about without looking at your screen. The situation would be better if the function were named as follows:

```
funtion getTimeOnFreezeLeft(account) {
    return account.timeOnFreeze;
}
```

**Class Names**
Classes and objects should have noun or noun phrase names like Customer, WikiPage, Account, and AddressParser. A class name should not be a verb.

**Method Names**
A method name should be a verb.

**Don't Be Cute**
Avoid the temptation to name your variable after a movie character or any slang word that seems funny to you. Remember, the developer working on your code next could be from a different cultural background or may not understand your sense of humor. Your cute naming could lead to massive confusion.

**Pick One Word per Concept**
If inside the `Colleague` class you use the `add()` method to add a new colleague, it is wrong to use `set()` or `oneMore()` to add a new tree to the `Garden` class.
