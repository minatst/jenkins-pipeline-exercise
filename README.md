## Coding Assignment

While the purpose is to learn Jenkins, this will be a coding assignment. It is made just to give you some tangible code to work with:
Remember, this is not a programming exercise, but a Jenkins one; code is only there so you have something to build :)

This repository comes with a java project from the start, but any language can be used. If you want to, just replace the java code with one of the other languages from (this repository)[https://github.com/emilybache/GildedRose-Refactoring-Kata].

The description of the application can be read [here](gildedrose.md), but is not necessary yet.

## 0. Fork the repo and clone it down on your machine
	And with your machine, we mean your VM.

## 1. Create a Jenkins server
	Cd into the repo and the setup folder.
	execute the setup.sh

## 2. Create a job

* Go into your Jenkins server and click on the `New Item` button on the left.
* Name your new job "#team# gilded rose" and choose `Pipeline` and click OK
* Head down to the `Pipeline` section of the job, and click on the "try sample pipeline" and choose `Hello world`
* Save and Build it.

The result should very well be that you have a blue (successful) build. Verify in Console that hello world was printed out.


## 3. Link Jenkins with your repo

* Click the `configure` button in the job
* Scroll to the pipeline section
* In the `definition`, select `Pipeline script from SCM`
* Add the url to your newly forked repository
* Now build
* Expect the unexpected!


## 4. make stages

Edit the Jenkinsfile on your newly forked repository
Make three stages:

* `Preparation`
* `Build`
* `Results`

each of the states should have a sh part saying `echo "stage name"`

Find the decalarative syntax [here](https://jenkins.io/doc/book/pipeline/jenkinsfile/).

Commit and push your changes

Go to the Jenkins server and run the job to see that it's working, and all three stages are present in the UI afterwards.

## 5. Running a Gradle test and archiving

With `Preperation` now being done, we need to build the code and store the result.
For each of the bullit points, try to build it to make sure it works before moving to the next.

* In your `Build` stage,  execute `./gradlew clean test jar`
* In your `Results` stage, make jUnit display the results of `**/build/test-results/test/TEST-*.xml` (lookup jUnit in the pipeline syntax if needed)
* Also in your `Results` stage, `archive` the generated jar file in the `build/libs` folder

The archiving part can be verified by looking for a small blue arrow next to the build number in the overview. Make sure you get your Jar file with you there.

## 6. fixing Gilded Rose

Having your pipeline set up, now it is time to fix the software problem itself. Go back to [the gilded rose description to read about it](gildedrose.md)

## Xtra. Parallel and stashing

We also need to get the javadoc generated for the project.

Fortunately that can be done with a small `./gradlew javadoc` command.

* Create another stage called `Javadoc` where you execute the above command, and archive the result in the `build/docs/javadoc` folder.

Now we have two processes that actually can be run in parallel. The `build` and `javadoc` steps both take in the sourcecode and produces artifacts. So lets try to run them in parallel.

> This assignment is loosely formulated, so you need to [look things up yourself](https://jenkins.io/doc/pipeline/steps/) in order to complete this one

* Stash the source code cloned in `Preparation` and call it source
* `build` and `javadoc` steps needs to be included in a parallel step like the one below

* Unstash the source code in both stages, and perform the normal build steps
* Stash the results instead of archiving. Call them `jar` and `javadoc`
* Unstash them in the `Results` step in the end where you archive them.