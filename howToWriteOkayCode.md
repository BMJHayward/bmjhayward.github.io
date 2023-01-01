1. rule of 10s
 + if it's more than 10 lines, put them in their own function
 + if there are more than 10 functions, put them in a class
 + if there are more than 10 classes, put them together in a module (or a namespace)

2. DRY - don't repeat yourself
  This one isn't a secret, but for many people who work "adjacent" to software engineering, it might be new.
  Basically, if you have to write it more than once, put it in a function and import a function.
  Or, write it once, and refer to it every where you were repeating it.
  This principal alone will make anyone's code easy to maintain and easy for others to contribute to.
  Sometimes, a bit of repetition is good, e.g. when something is almost identical, but there are still enough differences
  that boiling it down into a single object/function/class/module would actually be more complex anyway. You have to use your judgement a little bit.

3. Learn to use your version control.
  This will almost always be a tool called "git". So just use that. If the command line isn't your preference, there are
  programs like Sublime Merge [LINK] and Atlassian Sourcetree [LINK] which give you a visual interface.
  Basically, the 3 things you need to do are:
  When you are done changing something, "add" the file to git
    `git add <filename>`
  When you've finished that round of changes, you then "lock" the change in:
    `git commit -m 'write a message in quotes so what the change does'`
  You can make a different version of your code by doing the above 2 steps on a different "branch":
    `git branch -b 'name your branch something descriptive here'`
  And then you just switch between branches, rather than keeping many copies of your code:
    `git checkout masterbranch`
    `git checkout alternate_timeline`
    `git checkout prototype_feature`
  ..and so on..

4. Keep learning all about your language until you know the whole thing, not just the bits you need to get the job done.
  + granted, C++ is a monster and some other languages are huge enough that you probably won't ever know _all_ of it
5. Keep learning about your editor/IDE, same as (4)

6. Try reading it out loud sometimes
  This may sound silly, but it's a bit like rubber-ducky programming. A lot of "good" programming is what makes it clear to other programmers, rather than the computer.
  If you read it out loud, or explain it out loud to someone, different parts of your brain are engaged and you'll likely think of something you missed when you were writing
  "in the zone".

7. Don't be afraid to chuck it out and start again.
  The real asset is your own knowledge, not the code. Rewriting something, even just a small part can really unlock a lot of benefits.
  It's amazing what becomes obvious when you come back to old code 6 months down the track.


There are some great books out there, like "The pragmatic programmer", and "Code complete". Sure, the code
examples might be outdated, but the principles can still be very, very useful. Another option if you like books and are
interested in the Python langauge, testing or web apps is "Test-Driven Development with Python".


