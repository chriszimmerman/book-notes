# Chapter 2 - Writing Tightly Coupled Code

Implementing domain logic in either the UI or data access layers will lead to pain and suffering. Do yourself a favor and create a domain layer from the beginning. 

As discussed in chapter 1, the Single Responsibility Principle (SRP) states that each class should only have a single responsibility, or, better put, a class should have only one reason to change.

More often than not, however, it can be more challenging to see whether a class has multiple reasons to change. What often helps is to look at the SRP from the perspective of code cohesion. Cohesion is defined as the functional relatedness of the elements of a class or module. The lower the relatedness, the lower the cohesion, and the higher the risk a class violates the SRP.

Being able to detect SRP violations is one thing, but determining whether a violation should be fixed is yet another. It isn’t wise to apply the SRP if there are no symptoms. Needlessly splitting up classes that cause no maintainability problems can add extra complexity. The trick in software design is to manage complexity.

One of the many reasons that it’s so easy to write tightly coupled code is that both the language features and our tools already pull us in that direction. If you need a new instance of an object, you can use the new keyword. If you don’t have a reference to the required assembly, Visual Studio makes it easy to add. But every time you use the new keyword, you introduce a tight coupling. As discussed in chapter 1, not all tight coupling is bad, but you should strive to prevent tight coupling to Volatile Dependencies.