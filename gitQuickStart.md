1. do some work
2. `git add name_of_file_you_worked_on`
3. do more work, add more files if you want
4. `git commit -m message_saying_what_you_did`

That's it.

If you want different versions of your work to switch between:

1. `git checkout -b name_of_branch`
2. `git checkout main`
3. `git checkout name_of_branch`

That's it. You can have as many branches as you want.

If you want to save your work elsewhere e.g. github, private server

1. `git remote add name_of_remote_repository http://address_of_remote.com/name_of_remote_repository`
2. after doing work in above 2 sections:
  `git push name_of_remote_repository`
  + git will send work from the branch you are on, to the remote repository
3. if the remote repo has been updated:
  `git pull name_of_remote`
  or
  `git fetch name_of_remote`
  then when ready: `git pull name_of_remote`

That's it. That's git. If you don't love using the command line, try one of these:

Sublime Merge
Git Kraken
Tortoise Git
GitG
GitK

Or your IDE likely has a version control feature.

They all run those commands, but with a visual interface, which can be nice.

Other alternatives to git include Mercury (hg), bazaar, and older systems like subversion (SVN) 
