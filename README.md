# Ace the Programming Interview

## Java EE components
+ Web components
    * Java Servlet
    * JavaServer Faces (JSF)
    * JavaServer Pages (JSP)

+ Business componenets
    * Enterprise JavaBeans (EJB)

## Database replication and sharding

## SOLID

## Spring and proxies
### Spring transactions
### AspectJ vs CGLib

## Java internals
### Java invoke dynamic
### BigDecimal and BigInteger internals
### What's new in Java 8
### What's new in Java 9
### Difference between Errors and Exceptions

## Concurrecncy and parallelism
### How to make local variable thread-safe?
### What is _access barrier_?
### How to make code thread-safe (without `synchronized` keyword)

## Testing
## What kind of _test doubles_ do you know?
- _Dummy objects_ are passed around but never actually used. Usually they are just used to fill parameter lists.
- _Fake objects_ actually have working implementations, but usually take some shortcut which makes them not suitable for production (an in memory database is a good example).
- _Stubs_ provide canned answers to calls made during the test, usually not responding at all to anything outside what's programmed in for the test. Stubs may also record information about calls, such as an email gateway stub that remembers the messages it 'sent', or maybe only how many messages it 'sent'.
- _Mocks_ are what we are talking about here: objects pre-programmed with expectations which form a specification of the calls they are expected to receive.

## Branch vs line code coverage
*Line coverage* measures how many statements you took (a statement is usually a line of code, not including comments, conditionals, etc). *Branch coverage* checks if you took the true and false branch for each conditional (if, while, for). You'll have twice as many branches as conditionals.

Consider the example:

```java
    public int getNameLength(boolean isCoolUser) {
        User user = null;
        if (isCoolUser) {
            user = new John(); 
        }
        return user.getName().length(); 
    }
```
If you call this method with `isCoolUser` set to `true`, you get 100% statement coverage. Sounds good? NOPE, there's going to be a null pointer if you call with `false`. However, you have 50% branch coverage in the first case, so you can see there is something missing in your testing (and often, in your code).

## Mockito author Szczepan Faber


## JMS
### JMS Publish-Subscriber using REST services

## Git merge vs rebase
- Merge takes all the changes in one branch and merges them into another branch in one commit.
- Rebase says I want the point at which I branched to move to a new starting point

So when do you use either one?

### Merge
- Let's say you have created a branch for the purpose of developing a single feature.  When you want to bring those changes back to master, you probably want **merge** (you don't care about maintaining all of the interim commits).  
- This creates a new “merge commit” in the feature branch that ties together the histories of both branches. Merging is nice because it’s a non-destructive operation. The existing branches are not changed in any way. On the other hand, this also means that the feature branch will have an extraneous merge commit every time you need to incorporate upstream changes. If master is very active, this can pollute your feature branch’s history quite a bit.

### Rebase
- A second scenario would be if you started doing some development and then another developer made an unrelated change.  You probably want to pull and then **rebase** to base your changes from the current version from the repo.
- This moves the entire feature branch to begin on the tip of the master branch, effectively incorporating all of the new commits in master. But, instead of using a merge commit, rebasing re-writes the project history by creating brand new commits for each commit in the original branch.


## Business
### OTC transactions


## Problems to solve
### Find missing number
### 9-balls 
