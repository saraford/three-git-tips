# Tip 1 - How to visually learn and visually teach git 

Members of the Electron team at GitHub wrote an outstanding git visualization tool located at https://git-school.github.io/visualizing-git/ Note: there is no working directory or staging area for this visualization tool. This tool is for visualizing git commands on commits. 

To setup a demo using Git via the command line, try running a bash script! This way you can easily get back to a baseline by removing the `.git` folder for that repository.

:warning: Never delete a .git folder in a real-world scenario. You can always get data back from git with the right git commands. But once the .git folder is deleted, that data is gone. 

## Fast-forward merge vs Merge Commit

These steps show you how to compare and contrast a Fast-forward merge and a Merge commit using the visualization tool. 

#### Bash script to setup a FF merge

This script creates a new git repository, makes an initial commit, then follows git best practices by creating a branch called feature and adds three feature work items to the feature branch. 

First, make sure you are in the directory you want to create the repository. (e.g. mkdir mydemo; cd mydemo;)

Second, cut and paste the following into your shell. (These scripts were verified using a bash shell.)

```
git init;
touch app.txt;
git add app.txt;
git commit -m "init commit";
git checkout -b feature;
for d in {1..3};
 do touch featurework$d.txt;
 git add featurework$d.txt;
 git commit -m "adding work item $d";
 done

```

:writing_hand: the extra whitespace is for the return key on the last command. 

Third, run `git log` Notice how feature has work that master doesn't have. But... feature has all the commits that master has (the init commit). 

Fourth, run `git checkout master` then `git merge feature` Notice how the output says "Fast-Foward merge"

#### Try a Fast-Forward merge using the Visualization Tool

Let's next visualize the FF merge using the visualization tool. 

Notice the commands on the left-hand side. Follow those commands. 

![image](https://user-images.githubusercontent.com/11529908/88503559-f74ff080-cf86-11ea-804c-e8d903dd360f.png)

Why do you think it is called a FF merge, after watching the visualization? 

#### Undo your FF merge

From the command line on branch master, run `git reset --hard <init commit id>` If you're new to git resets, don't worry. We'll cover the command a little further on.

On the visualization tool, run `undo` - Note: there is no git undo command. You'll learn why in the second git tip. This undo is only for the visualization tool, that comes in really handy later on. 

#### Try a merge commit in the visualization tool 

Let's say there's been a bug fix in production BEFORE you did the merge. Let's visualize this scenario first. From the visualization tool, continue to type in the commands listed on the left-hand side. 

![image](https://user-images.githubusercontent.com/11529908/88503988-1b600180-cf88-11ea-83a6-7024a36a6ff8.png)

Now in the visualization tool, run `git merge feature` Why do you think a new commit was made? You can assume there is NO merge conflict. If you were to run `git log` from branch master, how many commit IDs would be listed? 

Run `git log` in the visualization tool. Did the number of dots light up as you had expected?

Lastly, let's rinse and repeat from the command line. Repeat the same commands from the visualization tool in the command line. What's different this time? Why do you think your `git config --global core.editor` appeared? Why do you think git used a "recursive strategy"? 

## Rebase vs Merge

Rebase is out of scope for this talk, but let's just take a quick peek in the visualization tool. Type `undo` to roll back the merge commit. Now type `git checkout feature` then `git rebase master` - I always say out loud "I want to rebase ONTO master" to help me with the command. What happened?! Can you spot all the differences from the rebase? 

![image](https://user-images.githubusercontent.com/11529908/88504419-94138d80-cf89-11ea-9ad5-a9bab26b2ea1.png)

If you are teaching git, you'll want to type `undo` then repeate the rebase a few times and compare it to a merge commit to give people a chance to see the differences play out in real time.  

## Answers to above questions for tip 1

1. Why do you think it is called a FF merge, after watching the visualization? My answer: Git is a directed acyclic graph and all branches are just pointers to nodes in a graph (including head which is a pointer that tells git to perform its operations here.) In the case of a FF merge, the "pointer" master just moves down the graph to feature. There's nothing to "merge"

2. Why do you think a new commit was made? My answer: Because master has "code" that feature didn't have and feature has "code" that master doesn't have, we need to combine these somehow. Note: this is NOT about merge conflicts. Even if there are no conflicts, you'd still have to do a merge. Sometimes I use an analogy of yellow + blue = green (a new color). Another way (and more accurate way) to think about it is the new commit has two parents now. This is what git is doing under the hood to write its graph history. 

3. Why do you think your `git config --global core.editor` appeared? My answer: Notice how in the visualization tool there's a "merge" message on the merge commit. The visualization tool automatically gave it the message "merge". All commits must have a message. There's no way around that. 

4. Why do you think git used a "recursive strategy"? My answer: From the command line, you'll see "recursive strategy" instead of Fast forward. There are many different types of merge strategy, including octopus merge! 

5. Can you spot all the differences from the rebase? My answer: I really want the audience to notice the new commit IDs. Rebase changes history! Understand what this means (tl;dr - only rebase your own commit IDs that have never seen the light of day via a push). Next, I demo checking out master and merging in feature and noticing a fast-forward merge instead of a merge commit. It doesn't matter which you use when you have updates to push to master. It's really about how your team wants to work and how you want your graphs to look. 





