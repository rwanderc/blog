---
layout:      post
title:       "Simple, Short and Concise"
subtitle:    "the first thing you must consider when writing code"
description: "Writing code is not an easy job. Writing code the others can read is an art and is counterintuitive how hard is to keep code simple. Here are 5 big rules that help me write cleaner code every day and might also help you."
date:        2019-09-21 22:00:00
author:      "Wander Costa"
header-img:  "/img/post-bg-clean-landscape.jpg"
thumb-img:   "/img/post-thumb-clean-landscape.jpg"
img-credits: "kues"
img-credits-url: https://www.freepik.com/free-photos-vectors/background
img-credits-real-url: https://www.freepik.com/free-photo/tree-beautiful-day_928890.htm#page=9&query=simplicity&position=8
tags:
- java
- clean code
- software engineering
- software development
- development
---

### Coding for computers is easy. The challenge is to code for people.

In this series of posts, I present 5 high level rules and aspects that I
 collected over the years that help me write better code daily and might
 also help you. Read from beginning: [The Journey to 
 Clean Code][journey].

# Write simple, short and concise
Reading code is like connecting pieces. Having easier pieces to connect
Simple and short do not always walk together. You you can write less
 lines, which does not mean less complexity.

**To be simple**, **to be short** and **to be concise** mean so many
different things, that I must enumerate them into more tangible aspects:

* **Single Responsibility Principle**: [**SRP**][SRP] is possibly the
  most important aspect, and has to do with the **keep it short**
  and **keep it simple** advices.
  
  Short is not only in terms of code, but also in responsibility.
  Methods should do ONE thing! Classes should have ONE responsibility.
  Not many, only ONE. If methods do many things, it's harder to test
  and maintain them.
  
  I highly recommend reading further about [**SRP**][SRP] as well as
  the whole [**SOLID**][SOLID] principles. It's valuable!

* **Avoid nesting**: nested conditions are hard to read and to
  understand, since they require more mental mapping. How many times
  did you face a code that you had to read 4 or 6 times to understand,
  like this:

  ``` java
  if (condition0) {
    if (condition1) {
      if (condition2) {
        if (condition3) {
            ...
        }
      } else {
          ...
      }
    }
  }
  ```

  This is simply hard to read, due to the big decision tree in it.
  Also, it's definitely hard to test, and most probably the next
  developer will have a hard time to avoid introducing a bug in the
  expected behavior.
  Nested conditions are tough! I avoid them by refactoring, by possibly
  delegating part of the responsibility to other classes, or at last
  by introducing other methods so as to split the logic into more
  digestable pieces.

* **Fishbone logic**: think of the logic flow as a fishbone, where
  the main or most important scenario (a.k.a. happy path) is the one
  that will walk throught the whole execution, from the head to the
  tail. And any side scenario or exceptional condition might interrupt
  the flow.

  ![fishbone.png](/img/fishbone.png)
  <sup>Image by [Flaticon][FlaticonAuthor]</sup>

  The following code exemplifies with an authentication logic how the
  opposite can lead to confusion:
  ``` java
  User authenticate(String username, String password) {
    if (!system.isUnderMaintenance()) {
      User user = dao.find(username, password);
      if (!user.isActive() || user.isAccountBlocked()) {
        throw new UserNotAllowedException(); // Exceptional path
      } else {
        return user; // Happy path
      }
    } else {
      throw new SystemUnderMaintenanceException(); // Exceptional path
    }
  }
  ```

  This code could be written in a more readable way, where the happy
  path is not nested inside conditionals:
  ``` java
  User authenticate(String username, String password) {
    if (system.isUnderMaintenance()) {
      throw new SystemUnderMaintenanceException(); // Exceptional path
    }
    User user = dao.find(username, password);
    if (!user.isActive() || user.isAccountBlocked()) {
      throw new UserNotAllowedException(); // Exceptional path
    }
    return user; // Happy path
  }
  ```

  However, it does not mean that every code can be written this way.
  Of course, there are cases where this is not viable or does not apply.

* **Positive statements**: for me, it's much more intuitive testing
  `if (condition) {...}` than testing `if (!condition) {...}`. This
  does not seem like a big thing, but it does really make a difference
  when reading code.

  Pick the latest good authentication example... it's testing 
  `!user.isActive()`. Negative conditionals alone do not cause much
  problems to read. However, when other conditionals are put
  together or nested inside, and you have to understand the connected
  logic, the negative statement are somehow harmful. The code could
  be written this way:

  ``` java
  User authenticate(String username, String password) {
    if (system.isUnderMaintenance()) {
      throw new SystemUnderMaintenanceException(); // Exceptional path
    }
    User user = dao.find(username, password);
    if (user.isDisabled() || user.isAccountBlocked()) {
      throw new UserNotAllowedException(); // Exceptional path
    }
    return user; // Happy path
  }
  ```

  it's not possible, given the restrictions in the model. But whenever
  possible, make use of the simpler  For exemple, if the User entity
  has a field `active`, 

* **Good formatting & indentation**: that's quite simple. Which one is
  easier to read?
  This one:

  ``` java
    User authenticate(String username, String password) {
        if (system.isUnderMaintenance())
    { throw new SystemUnderMaintenanceException(); // Exceptional path
    }
    User user = dao.find(username, password);
    if (user.isDisabled() || user.isAccountBlocked()) {
    throw new UserNotAllowedException(); // Exceptional path
    }
        return user; // Happy path
  }
  ```

  Or this one:

  ``` java
  User authenticate(String username, String password) {
    if (system.isUnderMaintenance()) {
      throw new SystemUnderMaintenanceException(); // Exceptional path
    }
    User user = dao.find(username, password);
    if (user.isDisabled() || user.isAccountBlocked()) {
      throw new UserNotAllowedException(); // Exceptional path
    }
    return user; // Happy path
  }
  ```

  Take good care of the way you write code.

## Use meaningful naming
Back in college I studied with a guy (let's call him John) that was used
 to naming variables as `john1`, `john2`, `john3`... That was really 
 funny back then. Today, I still see variables which names do not mean
 any useful thing, but now it's not that funny any more.
 
Names of variables, parameters, methods and classes are for humans! If
 you name a variable `numberOfClients` of `nc`, the computer will always
 understand. But not people! You have the chance of making it easier for
 people to understand what you write just by providing better naming.

Below, a list of the main drives I usually take to create useful names:
* **Must describe the purpose**: the above mentioned `numberOfClients` is
  a good example of a purpose. If instead, it was just called `clients`,
  the reader would need more insights to figure out if it's not a counter
  of clients, a list of clients or a boolean flag to advise it has clients.
  No matter if you code in a strongly typed language like Java (where you
  rely on the type to help you understand the purpose of the variable) or
  weakly typed language like Javascript (where you cannot rely on the type),
  names must bring a full of insights and the purpose of its existence.

* **Don't change the purpose of a variable**: or if you need to change it,
  make it another variable. If you had the `listOfClients` and during the
  flow, this list is filtered to get only the active clients, you can
  easily create another variable now called `listOfActiveClients`.
  Even though it's argueable tha the purpose has changed, in the context
  of a code, this is an important insight a read cannot lose. Remember,
  names are for humans!

* **Use names you can pronunciate**: for me, the reading of code is like
  the reading of a text or article. I need to make sense of the pieces
  I'm digesting. Whenever a variable without a quick pronunciable name
  comes into my mind, there is a mental effort to connect that variable
  with the context it belongs to. Natural languages already provided
  people a good amount of  sets of conventions and

* **Use searcheable names**: 

* **Avoid adding something only you know**: using `i` or `j` for loops is
  something every programmer is accostummed to. 
  
### Classes, methods and variables
 
 programing languages offer
 the possibility of naming them differently so as other humans can make
 the same understanding.
Nomes pronunciaveis
nomes que revelam proposito
nomes passiveis de busca
evite mapeamento mental
 NO BIAS
 

## Avoid writing comments
First of all... documentation is not comment, and vice-versa!
 Documentation is definitely important and necessary for people who use
 the code to deeply understand what it does, what kinds of errors might
 happen, what return values to expect, etc.

However, commenting a line explaining what that line is doing is only
 done to clarify the implicit logic, that is not clear enough. Maybe
 due to naming conventions, 

## Get ride of the bias
That's the hardest, I'd say. We have our own identities, which are the
 product of the way we think. And that's absolutely important to have.
 However, writing code is like writing an essay. If each paragraph
 is written by a different person with different dialects, the reader
 the essay will possibly have problems to connect the ideas.

I see writing code as an analog situation, where we always share the
 writing of the essay. You might have your code style, more

My 
People express themselves the way they understand the world. That's
 absolutely important, because it 

## Refactor during tests
When I write code which test is not deadly simple, I have a clear
 indication that my code might be very coupled, or holding too many
 responsibilities.


# Why clean code matters


It that's yet not clear why clean code is not 
If there is a word to better describe it, for me, it's **productivity**.
 Of course in the past, but mainly today, when there are MVPs, quick
 iterations and feedback, when the market responds to what you create
 in a matter of hours or days. The whole computer engineering moved out
 from heavy and fat processes into a more lean approach, in order to 
 be able to move faster. Software development also did the same.

And being able to read and understand the code we write is a premise
 when 
 requirement 


# Time to Context
But the world is not boolean and I'm not pretending to say that today I
 absolutely write clean and readable code. Whenever a programmer writes
 code, he writes based on his own experiences, his own biased decisions.
 And I still have mine

  t really caused a developing my experiences and the learnings I had when working with other
 developers, Another very big factor

 book
 https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882
https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship-ebook/dp/B001GSTOAM/ref=sr_1_1?dchild=1&keywords=clean+code&qid=1589865571&sr=8-1
This shortly resumes my experience with  et exposes one of the main concerns
 every developer will have throughout their work life:
 **How to write code that people actually understand?**


# Conclusion
After years of experience, I understand that the way we code is the way we see the world, at least at a given moment. And that's the trick! The internal process that makes sense for you right now might not make any sense tomorrow, and therefore you reask the question: what does this code do?

As a chronicle author writing a book that drives the thoughts of the reader towards the understanding of the characters and scenario, a developer must write good code and that can lead the next developer to the understanding of what h wanted.should drive the Clean code is not a final state, 




[SRP]:https://en.wikipedia.org/wiki/Single-responsibility_principle
[SOLID]:https://en.wikipedia.org/wiki/Single-responsibility_principle
[FlaticonAuthor]:https://www.flaticon.com/free-icon/fish-bone-silhouette_2610
[cleancode]:https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882
[journey]:../clean-code-0-the-journey