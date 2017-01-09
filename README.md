# Ace the Programming Interview

## Java EE
+ Implementations
    * Oracle Glassfish reference implementation
    * RedHat
    * IBM

+ Components
    * Web components
        - Java Servlet
        - JavaServer Pages (JSP)
        - JavaServer Faces (JSF)
        
    * Business components
        - Enterprise JavaBeans (EJB)

## Database replication and sharding

## Scale-up vs scale-out (database example)
- *Scale-up* - when you buy more RAM or table space
- *Scale-out* - when you buy more instances and replicate them

## Coupling and cohesion
- Coupling
    + We can define it as the degree to which a class, method or any other software entity, is directly linked to another. This degree of coupling can also be seen as a degree of dependence.
    + *Example:* when we want to use a class that is tightly bound (has a high coupling) to one or more classes, we will end up using or modifying parts of these classes for which we are dependent.
    
- Cohesion
    + Cohesion is the measure in which two or more parts of a system work together to obtain better results than each part individually.
    + *Example:* Han Solo and Chewbacca aboard the Millennium Falcon.

## SOLID
- First defined by Rober C. Martin in early 2000s
- Gathered and named (SOLID) by Michael Feathers
- Based on two basic concepts: *coupling* and *cohesion*
- Principles
    + Single Responsibility Principle (SRP)
        * A class should have only one reason to change
        * This principle means that a class must have only one responsibility and do only the task for which it has been designed.
        * Otherwise, if our class assumes more than one responsibility we will have a high coupling causing our code to be fragile with any changes.
            
    + Open Closed Principle (OCP)
        * Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification
        * According to this principle, a software entity must be easily extensible with new features without having to modify its existing code in use.
            - open for extension: new behaviour can be added to satisfy the new requirements.
            - close for modification: to extending the new behaviour are not required modify the existing code.
        * If we apply this principle we will get extensible systems that will be less prone to errors whenever the requirements are changed. We can use the abstraction and polymorphism to help us apply this principle.
        
    + Liskov Substitution Principle 
    + Interface Segregation Principle 
    + Dependency Inversion Principle (DI)

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
### What kind of _test doubles_ do you know?
- _Dummy objects_ are passed around but never actually used. Usually they are just used to fill parameter lists.
- _Fake objects_ actually have working implementations, but usually take some shortcut which makes them not suitable for production (an in memory database is a good example).
- _Stubs_ provide canned answers to calls made during the test, usually not responding at all to anything outside what's programmed in for the test. Stubs may also record information about calls, such as an email gateway stub that remembers the messages it 'sent', or maybe only how many messages it 'sent'.
- _Mocks_ are what we are talking about here: objects pre-programmed with expectations which form a specification of the calls they are expected to receive.

### Branch vs line code coverage
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

### Mockito author Szczepan Faber


## JMS
### JMS Publish-Subscriber using REST services

## Garbage collector
### What is the difference between `real`, `user` and `sys` times in garbage collection log file
[Origin](https://blog.gceasy.io/2016/04/06/gc-logging-user-sys-real-which-time-to-use/)

**Real** is wall clock time – time from start to finish of the call. This is all elapsed time including time slices used by other processes and time the process spends blocked (for example if it is waiting for I/O to complete).

**User** is the amount of CPU time spent in user-mode code (outside the kernel) within the process. This is only actual CPU time used in executing the process. Other processes and time the process spends blocked do not count towards this figure.

**Sys** is the amount of CPU time spent in the kernel within the process. This means executing CPU time spent in system calls within the kernel, as opposed to library code, which is still running in user-space. Like ‘user’, this is only CPU time used by the process.

**User**+**Sys** will tell you how much actual CPU time your process used. Note that this is across all CPUs, so if the process has multiple threads, it could potentially exceed the wall clock time reported by Real.

**Example 1:**
```
[Times: user=11.53 sys=1.38, real=1.03 secs]
```
In this example: `user` + `sys` is much greater than `real` time. That’s because this log time is collected from the JVM, where multiple GC threads are configured on a multi-core/multi-processors server. As multiple threads execute GC in parallel, the workload is shared amongst these threads. Thus actual clock time (`real`) is much less than total CPU time (`user` + `sys`).

**Example 2:**
```
[Times: user=0.09 sys=0.00, real=0.09 secs]
```
Above is an example of GC Times collected from a Serial Garbage Collector. As Serial Garbage collector always uses a single thread only, real time is equal to the sum of user and system times.

### What causes long GC pauses
##### Too high object creation reate
Optimizing the application for create fewer object is good strategy. Use tools like `JProfiler`, `YourKit`, `JVisualVM`

##### Young generation too small 
[Origin.](https://dzone.com/articles/how-to-reduce-long-gc-pause)

When the young generation is undersized, objects will be prematurely promoted to the old generation. Collecting garbage from the old generation takes more time than collecting it from the young generation. Thus, increasing the young generation size has the potential to reduce long GC pauses. The young generation's size can be increased by setting one of the two JVM arguments
- `-Xmn` specifies the size of the young generation.
- `-XX:NewRatio` Specifies the size of the young generation relative to the old generation. For example, setting `-XX:NewRatio=2` means that the ratio between the old and young generation is `1:2`. The young generation will be half the size of the overall heap. So, if the heap size is 2 GB, then the young generation size would be 1 GB.

##### Change GC alogrithm
GC algorithm has a major influence on the GC pause time. Tune GC settings to obtain optimal GC pause time. If you don’t have a lot of GC expertise, then I would recommend using the G1 GC algorithm because of its auto-tuning capability. In G1 GC, you can set the GC pause time goal using the system property `-XX:MaxGCPauseMillis.` 

**Example:** `-XX:MaxGCPauseMillis=200`, the maximum GC pause time is set to 200 milliseconds. This is a soft goal, which JVM will try it’s best to meet it

##### Process Swapping
Sometimes due to a lack of memory (RAM), the operating system could be swapping your application from memory. Swapping is very expensive, as it requires disk accesses, which is much slower compared to physical memory access. In my humble opinion, no serious application in a production environment should be swapping. When the process swaps, GC will take a long time to complete.

[Get all processes that are being swapped](http://stackoverflow.com/questions/479953/how-to-find-out-which-processes-are-swapping-in-linux)

If you find your processes are swapping, then do one of the following:
- Allocate more RAM to the service
- Reduce number of processes running on the server to free up memory
- Reduce the heap size of your application


##### Tune number of GC threads
For every GC event reported in the GC log, user, sys, and real times are printed. 

**Example:** `[Times: user=25.56 sys=0.35, real=20.48 secs]`

To know the difference between each of these times, please [read this article](https://blog.gceasy.io/2016/04/06/gc-logging-user-sys-real-which-time-to-use/)If, in the GC events, you consistently notice that ‘real’ time isn’t significantly less than the ‘user’ time, then it might be indicating that there aren’t enough GC threads. Consider increasing the GC thread count. Suppose `user` time is 25 seconds and you have configured the GC thread count to be 5, then the real time should be close to 5 seconds (because `25 seconds/5 threads = 5 seconds`).

**WARNING:** Adding too many GC threads will consume a lot of CPU and take away resources from your application. Thus you need to conduct thorough testing before increasing the GC thread count.

##### Background IO Traffic
If there is heavy file system I/O activity (i.e. lot of reads and writes are happening), it can also cause long GC pauses. This heavy file system I/O activity may not be caused by your application. Maybe it is caused by another process that is running on the same server. Still it can cause your application to suffer from long GC pauses. Here is a brilliant [article from LinkedIn Engineers](https://engineering.linkedin.com/blog/2016/02/eliminating-large-jvm-gc-pauses-caused-by-background-io-traffic) that walks through this problem in detail.

When there is heavy I/O activity, you will notice the `real` time to be significantly higher than `user` time. 

**Example:** `[Times: user=0.20 sys=0.01, real=18.45 secs]` When this happens, here are some potential solutions to solve it:
- If high I/O activity is caused by your application, then optimize it.
- Eliminate the processes that are causing high I/O activity on the server.
- Move your application to a different server where there is less I/O activity.

##### Direct `System.gc()` calls
When `System.gc()` or `Runtime.getRuntime().gc()` method calls are invoked, it will cause stop-the-world full GCs. During stop-the-world full GCs, the entire JVM is frozen (i.e. No user activity will be performed during this period). `System.gc()` calls are made from one of the following sources:
- Your own developers might be explicitly calling the `System.gc()` method.
- It could be third-party libraries, frameworks, or sometimes even application servers that you use. Any of those could be invoking the `System.gc()` method.
- It could be triggered from external tools (like VisualVM) through the use of JMX.
- If your application is using RMI, then RMI invokes `System.gc()` on a periodic interval. This interval can be configured using the following system properties: 
    + `-Dsun.rmi.dgc.server.gcInterval=n`
    + `-Dsun.rmi.dgc.client.gcInterval=n`

Evaluate whether it’s absolutely necessary to explicitly invoke `System.gc()`. If there is no need for it, then please remove it. On the other hand, you can forcefully disable the `System.gc()` calls by passing the JVM argument: `-XX:+DisableExplicitGC`. For complete details on `System.gc()` problems and solution [refer to this article](https://blog.gceasy.io/2016/11/22/system-gc/)






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




## Coding tasks
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



# Unsorted questions
## CS basics, Algorithms & Data structures
### Junior
- Data structures e.g. Stack vs queue
- Describe heap
- Boolean operations, short circuits in Boolean operations
- Complexity of algorithms, big O notation (can be discussed during coding task)
- Trees, graphs and ways to traverse graph

###### Regular
- Sorting algorithms
- Recursion, tail recursion, mutual recursion
- Binary tree / Self balanced trees (Red-black tree, AVL, Splay)
- Search in binary tree
- Linear-time sorting (count sort)
- Sorting of linked list

###### Senior
- Prefix and suffix trees
- NP-complete algorithms
- Map/reduce and divide and conquer approach in solving tasks
- Dynamic programming
- How would you fix a cyclic dependency
- What is the difference between static and dynamic typing? Duck typing?

## OOP
###### Regular
- Inheritance vs Composition
- Coupling & Cohesion
- Name OOD principles that you know
- Do you know SOLID principles (describe one of them)

## Design patterns
###### Regular
- Give an insight into such patterns as Façade/Proxy/Decorator/Strategy/Observer (selectively)
- Which patterns do you use in a daily basis. Explain their principles

###### Senior
- What major patterns do the Java APIs utilize? Where exactly?
- Can you please explain “bridge”, "singleton" design pattern?
- Can you please explain "FlyWeight" design pattern?
- Describe difference between abstract factory and builder
- Describe difference between adapter and decorator
- Can you please explain "Builder" design pattern?

## Clean Code
###### Regular
- How do you identify good code? (Based on book "Clean code" by R. C. Martin)
- Average lines per method & class
- Is it a problem to have comments in code

###### Senior
- how package structure can be packed into java project? (package per feature, package per layer?)

## Testing
###### Regular
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

###### Senior
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

###### Regular
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
###### Regular
- How is virtual heap space divided in Java
- What difference between float and BigDecimal? How they store the data?
- Java object references
- What is deep copy of a Java object
- Disadvantages of setting heap size too high
- What are utilities for JVM monitoring? What is Jconsole?
- How to force GC be executed

###### Senior
- Garbage collection principles
- What are memory leaks
- Are memory leaks a problem in Java
- What is variable shadowing
- How would you monitor JVM
- How would you monitor how GC behaves during program execution
- Name few GC implementations (Serial, Parallel, ParallelOld, ConcarentMarkAndSweep) describe major differences

###### Expert
- Memory model in JVM (happens before, atomicity of memory reads and writes)
- Perm Space vs Metadata space in Java 8
- What are G1 of C4 garbage collectors


## Exceptions
###### Regular
- Checked vs. unchecked exceptions. Why would one use former or later?
- Difference in Error and UncheckedException
- Could we have only try and finally without catch
- Cases when the finally block isn't executed

###### Senior
- What is exception handling mechanism
- Java Exceptions API
- How to avoid catch block
- What is Multi-Catch     catch (ClassCastException | IOException e) what is compile type of e
- What is try-with-resources 

###### Expert
- What are Suppressed exceptions
- NoClassDefFoundError vs ClassNotFoundException


## Collections
###### Regular
- General collection interfaces (Collection, Set, Map, List, Queue, SortedSet, SortedMap)
- Interfaces extending Collection. Is Map part of Collection interface
- Difference between ArrayList and LinkedList
- Difference between Stack and Queue
- TreeSet vs TreeMap

###### Senior
- Internal structure of HashMap/Hashtable
- Requirements for implementation of hashCode to achieve best performance
- Definition and ways of resolving collisions in hash tables
- Differences between Hashtable and ConcurrentHashMap
- Special versions of collections. EnumSet, EnumMap, WeakHaskMap, IdentityHashMap
- Iterator and modification of a List. ConcurentModificationException. Collections with safe iterators (CopyOnWriteArrayList/CopyOnWriteArraySet)

## Java 5+ Specifics
###### Regular
- What is a parameterized or generic type
- Can we use parameterized types in exception handling
- What is Autoboxing and what are its advantages/pitfalls

###### Senior
- What is a wildcard parameterized type
- Problems Enum type solves (comparing to "public static int" enum pattern)
- Can we add something to List<?> (PECS principle?)
- What is method reference (Java 8)
- What is default metod (Java 8)
- What is stream API (java 8)
- What is Lambda expression (Java 8)
- What are Annotations and which predefined by the language specification does one know (@Deprecated, @Override, @SuppressWarnings)

###### Expert
- What happens with lambda after compile time?
- How to create custom annotation? What is retention type?


## Multithreading
###### Regular
- Thread vs Runnable, run() vs start()
- Synchronization of java blocks and methods
- Explain usage of the couple wait()/notify()
- Difference between sleep and wait
- Atomic operations
- Deadlock/Livelock definition plus example
- Executors framework
- What does it mean Volatile keyword
- java.util.concurrent.*, what utils do you know

###### Senior
- Thread local, what for are they needed
- Does child thread see the value of parent thread local
- Starvation/Race condition definition plus example
- Execution order
- Atomicity of long and double assignment operations
- ExecutorService submit vs execute
- How to publish object
- RecursiveTask vs RecursiveAction
- Lock-free operations, how to create lock-free implementation of field reassignment

###### Expert
- What is Fork-Join
- What is Phaser (Java 7)

## XML
###### Regular
- DTD vs. XMLSchema
- What is XPath
- What is XSLT
- SAX vs. DOM
- What is XML Namespace

###### Senior
- SAX vs StAX


## Java NIO
###### Senior
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

###### Regular
- What are the types of Dependency Injection Spring supports
- Autowiring. Types of autowiring.
- What are inner beans.

###### Senior
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

###### Regular
- Why many indexes are not good for performance
- What transaction isolation levels do you know?
- Which of SELECT, UPDATE, DELETE, ADD’s performance is mostly affected by performance of indexes?
- Types of tables (regular, temporary, index-organized etc)
- What does it mean database de-normalization?
- What are normal forms?
- What are ways to increase performance of database

###### Senior
- What is difference between SQL and NoSQL?
- Does every relational database implements SQL standard in the same way?
- What is a nested subquery?
- Types of indices in Oracle
- What is a hierarchical query? How to create it?
- What is a bitmap index
- Partitioning and methods of partitioning

## Unix and networking
###### Regular
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

###### Regular 
- What do you mean by Named – SQL query and how to invoke it? (hibernate/jpa)
- What is the difference between sorted and ordered collection in hibernate? (hibernate/jpa?)
- Difference between get() and load() (hibernate)
- find() vs getReference()  - (jpa)

###### Senior
- What are the entity states in Hibernate (hibernate - 3 states/jpa - 4 states (additional removed))
- Hierarchy-to-tables mapping strategies (hibernate/jpa)
- What are the Collection types in Hibernate ?
- Caching levels (hibernate/jpa)
- How to create own user type in hibernate? (hibernate)
- How is laziness One-To-Many  implemented in hibernate (hibernate)

###### Expert
- new features of jpa 2.1 - (jpa)
- Describe converters in Jpa  2.1 - (jpa)
- Statements order during flushing the session (hibernate)

## JMS
###### Senior
- What is the purpose of JMS
- Difference between topic and queue
- What is the basic difference between Publish Subscribe model and P2P model?
- Durability
- What are the different types of messages available in the JMS API?

## RESTFul
###### Regular
- What is REST
- What is Resource
- Which kind of requests are mapped to  which methods

## Typical web applications vulnerabilities and prevention
###### Senior
- SQL Injection
- Cross-site request forgery
- Reflected Cross-site scripting
- Click jacking
- Session token in URL
- Insecure direct object references



# Questions from candidate (+ what kind of answer are we looking for)
###### [Source](https://blog.elpassion.com/questions-a-knowledge-seeking-developer-should-ask-at-a-software-house-job-interview-4d09939e9cb2#.xgng295rk)

###### How do your developers learn?
- Code reviews
- Refactoring sprints
- Pair programming
- Dojos
- Retreats
- Code roasts
- Knowledge sharing sessions
- Internal talks
- Internal / External Hackathons
- Safari books / Coursera sponsored trainings

###### Do you allow developers to experiment? 
- Free choice of technology
- Encouraged to try new things
- Time for exploring unchartered territories

###### What does it take for one to get promoted to a higher position?
- Carrer path for developers
- 1-on-1s
- Feedback
- Peer review
- Evaluation at least once in 6 months
- Know what sills are expected at different carrer levels

###### Tell me about a time your team did something totally different / innovative
- We pay in Bitcoins
- We had bot wars Hackathon
- We had no-mouse week
- We tried X and failed

###### Will I be mentoring others and will I be mentored by someone?
- Both seniors and juniors on team
- Team has tech-mentor
- Pair programming
- Code reviews

###### Are there any further steps available after one becomes senior?
- Plan for Seniors'growth
- We are happy to shape your future
- Continuous learning
- Mentorship

###### Do you offer training allowance & time off for training / attending conferences?
- Sponsored conference outings
- Buy learning materials
- We talk at conferences a lot
- Training allowance is offered

###### What do you do when project starts to go wrong?
- You fail and learn
- Look at what went wrong and do it right next time
- We do retro for both success and failures
