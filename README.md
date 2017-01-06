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
### How to make local variable thread-safe? (tricky :smile:)
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
### How to make sure JMS message is received

### Number of pairs in Array
write java function which calculates number of pairs in array. Pair is when two nubers added together, result is zero.

### Traverse tree
Write java fuction to find some element in binary search tree

### Reverse List
Write java function to reverse elements in one directional linked list

### Merge sorted arrays
Having two separate sorted arrays, write java function merging these arrays in single sorted array without sorting it 

### Find duplicate number in array
An array contains numbers. Exactly one number is duplicated in the array. Write java function to find this duplicate

### Find number in matrix
In a 2D matrix, every row ins increasingly sorted from left to rifht. Last number in each row is not greater then first number in next row. Write java function to check if there is a number in the matrix

### Find number of occurances in array
Write java function to find number of occurances of some number in given array of numbers



## Unsorted questions
## CS basics, Algorithms & Data structures
### Junior
- Data structures e.g. Stack vs queue
- Describe heap
- Boolean operations, short circuits in Boolean operations
- Complexity of algorithms, big O notation (can be discussed during coding task)
- Trees, graphs and ways to traverse graph

### Regular
- Sorting algorithms
- Recursion, tail recursion, mutual recursion
- Binary tree / Self balanced trees (Red-black tree, AVL, Splay)
- Search in binary tree
- Linear-time sorting (count sort)
- Sorting of linked list

### Senior
- Prefix and suffix trees
- NP-complete algorithms
- Map/reduce and divide and conquer approach in solving tasks
- Dynamic programming
- How would you fix a cyclic dependency
- What is the difference between static and dynamic typing? Duck typing?

## OOP
### Regular
- Inheritance vs Composition
- Coupling & Cohesion
- Name OOD principles that you know
- Do you know SOLID principles (describe one of them)

## Design patterns
### Regular
- Give an insight into such patterns as Façade/Proxy/Decorator/Strategy/Observer (selectively)
- Which patterns do you use in a daily basis. Explain their principles

### Senior
- What major patterns do the Java APIs utilize? Where exactly?
- Can you please explain “bridge”, "singleton" design pattern?
- Can you please explain "FlyWeight" design pattern?
- Describe difference between abstract factory and builder
- Describe difference between adapter and decorator
- Can you please explain "Builder" design pattern?

## Clean Code
### Regular
- How do you identify good code? (Based on book "Clean code" by R. C. Martin)
- Average lines per method & class
- Is it a problem to have comments in code

### Senior
- how package structure can be packed into java project? (package per feature, package per layer?)

## Testing
### Regular
- Black box vs white box testing
- Functional vs nonfunctional testing
- Regression vs retesting
- Smoke test vs sanity test
- Outside-in testing
- How to test legacy code
- Do you have experience in TDD. What are TDD steps (test, code, REFACTOR)
- What libraries that help to write unit tests have you used
- What is mocking? Have you used any mocking frameworks?
- What does quality code mean to you?

### Senior
- What features of Junit do you know? @DataPoint @Theory
- Do you know BDD, how it differs from TDD

## Java basics
### Junior
- Length in bytes for primitive types
- Meaning of keywords: static, final, transient
- this' and 'super' keywords
- Contract between equals() and hashCode()
- Access modifiers in Java
- Explain strictfp, volatile, transient
- What different between StringBuffer and StringBuilder
- Purpose, types and creation of nested classes

### Regular
- What does it mean that an object or a class is mutable or immutable
- How to make class immutable
- Besides “string” do you know any other immutable classes
- Rules to create equals method 
- Difference between overriding and overloading
- How can we replace multi-inheritance pattern in Java
- Can interface inherit from different interface
- What is a marker interface
- Regular vs. static initialization blocks

## Memory & GC
### Regular
- How is virtual heap space divided in Java
- What difference between float and BigDecimal? How they store the data?
- Java object references
- What is deep copy of a Java object
- Disadvantages of setting heap size too high
- What are utilities for JVM monitoring? What is Jconsole?
- How to force GC be executed

### Senior
- Garbage collection principles
- What are memory leaks
- Are memory leaks a problem in Java
- What is variable shadowing
- How would you monitor JVM
- How would you monitor how GC behaves during program execution
- Name few GC implementations (Serial, Parallel, ParallelOld, ConcarentMarkAndSweep) describe major differences

### Expert
- Memory model in JVM (happens before, atomicity of memory reads and writes)
- Perm Space vs Metadata space in Java 8
- What are G1 of C4 garbage collectors


## Exceptions
### Regular
- Checked vs. unchecked exceptions. Why would one use former or later?
- Difference in Error and UncheckedException
- Could we have only try and finally without catch
- Cases when the finally block isn't executed

### Senior
- What is exception handling mechanism
- Java Exceptions API
- How to avoid catch block
- What is Multi-Catch     catch (ClassCastException | IOException e) what is compile type of e
- What is try-with-resources 

### Expert
- What are Suppressed exceptions
- NoClassDefFoundError vs ClassNotFoundException


## Collections
### Regular
- General collection interfaces (Collection, Set, Map, List, Queue, SortedSet, SortedMap)
- Interfaces extending Collection. Is Map part of Collection interface
- Difference between ArrayList and LinkedList
- Difference between Stack and Queue
- TreeSet vs TreeMap

### Senior
- Internal structure of HashMap/Hashtable
- Requirements for implementation of hashCode to achieve best performance
- Definition and ways of resolving collisions in hash tables
- Differences between Hashtable and ConcurrentHashMap
- Special versions of collections. EnumSet, EnumMap, WeakHaskMap, IdentityHashMap
- Iterator and modification of a List. ConcurentModificationException. Collections with safe iterators (CopyOnWriteArrayList/CopyOnWriteArraySet)

## Java 5+ Specifics
### Regular
- What is a parameterized or generic type
- Can we use parameterized types in exception handling
- What is Autoboxing and what are its advantages/pitfalls

### Senior
- What is a wildcard parameterized type
- Problems Enum type solves (comparing to "public static int" enum pattern)
- Can we add something to List<?> (PECS principle?)
- What is method reference (Java 8)
- What is default metod (Java 8)
- What is stream API (java 8)
- What is Lambda expression (Java 8)
- What are Annotations and which predefined by the language specification does one know (@Deprecated, @Override, @SuppressWarnings)

### Expert
- What happens with lambda after compile time?
- How to create custom annotation? What is retention type?


## Multithreading
### Regular
- Thread vs Runnable, run() vs start()
- Synchronization of java blocks and methods
- Explain usage of the couple wait()/notify()
- Difference between sleep and wait
- Atomic operations
- Deadlock/Livelock definition plus example
- Executors framework
- What does it mean Volatile keyword
- java.util.concurrent.*, what utils do you know

### Senior
- Thread local, what for are they needed
- Does child thread see the value of parent thread local
- Starvation/Race condition definition plus example
- Execution order
- Atomicity of long and double assignment operations
- ExecutorService submit vs execute
- How to publish object
- RecursiveTask vs RecursiveAction
- Lock-free operations, how to create lock-free implementation of field reassignment

### Expert
- What is Fork-Join
- What is Phaser (Java 7)

## XML
### Regular
- DTD vs. XMLSchema
- What is XPath
- What is XSLT
- SAX vs. DOM
- What is XML Namespace

### Senior
- SAX vs StAX


## Java NIO
### Senior
- What is NIO
- What is Channel
- What is Buffer
- What is Charset
- What is Selector
- How to lock file?
- What is NIO2 ?

## Spring
### Junior
- Basic idea of IoC pattern. Benefits.
- What is Spring configuration file? How does it look like?
- Out of the box bean scopes (singleton, prototype, request, session, global session)
- What are the main bean scopes in web container

### Regular
- What are the types of Dependency Injection Spring supports
- Autowiring. Types of autowiring.
- What are inner beans.

### Senior
- What modules does Spring Framework have
- Describe Test support in Spring (AbstractTransactionalSpringTests)
- Describe AOP integration in Spring
- How to integrate Spring and Hibernate using HibernateDaoSupport
- Beans lifecycle

## SQL
### Junior
- What are DML and DDL
- Aggregate functions with examples
- Primary key vs unique key. Differences.
- What are transactions?
- What is ACID?
- Views, why they are needed?
- What is a view/materialized view.
- What types of constraints does one know?
- Are database Indexes useful? What is the role of them?
- What kind of joins do you know?
- What is the difference between inner join and outer join?

### Regular
- Why many indexes are not good for performance
- What transaction isolation levels do you know?
- Which of SELECT, UPDATE, DELETE, ADD’s performance is mostly affected by performance of indexes?
- Types of tables (regular, temporary, index-organized etc)
- What does it mean database de-normalization?
- What are normal forms?
- What are ways to increase performance of database

### Senior
- What is difference between SQL and NoSQL?
- Does every relational database implements SQL standard in the same way?
- What is a nested subquery?
- Types of indices in Oracle
- What is a hierarchical query? How to create it?
- What is a bitmap index
- Partitioning and methods of partitioning

## Unix and networking
### Regular
- How to get list of running processes?
- Hot to get thread dump of running java process (kill -3)
- Describe grep / tail / less / more
- Describe difference between TCP and UDP
- How to find out free space on the disk? (df)
- How to find out a size of a directory? (du)

## Hibernate / JPA
### Junior
- Benefits and risks of using Hibernate (hibernate)
- What is a SessionFactory? Is it a thread-safe object?  (hibernate)
- SessionFactory vs Session  (hibernate)
- EntityManager vs EntityManagerFactory  (JPA)
- What is the difference between merge and update (hibernate)
- What is difference between merge and refresh (jpa)
- What is Hibernate Query Language/ Java Persistence Query Language (HQL/JPQL)? (hibernate/jpa)
- Lazy loading (hibernate/jpa)
- Describe concept of JPA (ex: EntityManager, EntityManagerFactory, PersistenceContext, PersistenceUnit) - (jpa)
- Describe annotation: @Temporal/@Transient/@Table/@Version?  - (jpa)

### Regular 
- What do you mean by Named – SQL query and how to invoke it? (hibernate/jpa)
- What is the difference between sorted and ordered collection in hibernate? (hibernate/jpa?)
- Difference between get() and load() (hibernate)
- find() vs getReference()  - (jpa)

### Senior
- What are the entity states in Hibernate (hibernate - 3 states/jpa - 4 states (additional removed))
- Hierarchy-to-tables mapping strategies (hibernate/jpa)
- What are the Collection types in Hibernate ?
- Caching levels (hibernate/jpa)
- How to create own user type in hibernate? (hibernate)
- How is laziness One-To-Many  implemented in hibernate (hibernate)

### Expert
- new features of jpa 2.1 - (jpa)
- Describe converters in Jpa  2.1 - (jpa)
- Statements order during flushing the session (hibernate)

## JMS
### Senior
- What is the purpose of JMS
- Difference between topic and queue
- What is the basic difference between Publish Subscribe model and P2P model?
- Durability
- What are the different types of messages available in the JMS API?

## RESTFul
### Regular
- What is REST
- What is Resource
- Which kind of requests are mapped to  which methods

## Typical web applications vulnerabilities and prevention
### Senior
- SQL Injection
- Cross-site request forgery
- Reflected Cross-site scripting
- Click jacking
- Session token in URL
- Insecure direct object references
