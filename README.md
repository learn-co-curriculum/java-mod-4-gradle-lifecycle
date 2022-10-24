# Lifecycles, phases, and goals

## Learning Goals

- Define the Gradle Lifecycle
- Define the relationships between Lifecycle Phases, Tasks, and Plugins.

## Introduction

A Gradle build follows a clearly defined process to deploy and distribute a project.  

- A **lifecycle** is defined by a list of phases. 
- A **phase** represents a stage of a lifecycle.
- A **task** is a piece of work that a build performs.
- A **plugin** is an artifact that provides tasks.

## Lifecycle Phases

Every Gradle build proceeds through three lifecycle phases
in precisely the same order: initialization, configuration, and execution.

- **Initialization**:  Gradle locates the build files it must process and
  determines whether the build is single-project or multi-project. The build files
  are passed to the next phase.
- **Configuration**: Gradle executes each build file as a Groovy or Kotlin script
  to create a directed acyclic graph (DAG) of task objects.
  - “Directed” means each dependency arrow goes in one direction. 
  - “Acyclic” means that there are no loops in the graph.
- **Execution**: Gradle identifies the tasks in the task DAG  
  and executes them in dependency order to ensure each task is executed only once.

For example, the following image shows the DAG for a Java build:

![java plugin DAG](https://curriculum-content.s3.amazonaws.com/6002/gradle-lifecycle/java-dag.png)

Each task defines a set of inputs and outputs.
The inputs for **compileJava** are the Java source files, while the
outputs are class files. If none of the inputs or outputs
have changed, Gradle will skip that task. 
We call this Gradle’s **incremental build support**.

Let's explore some of this using IntelliJ's Gradle Tool Window,
which can be opened either by clicking on "Gradle" along
the edge of the IntelliJ window, or by selecting ** View | Tool Windows | Gradle**
from the main menu.
  
![gradle tool window](https://curriculum-content.s3.amazonaws.com/6002/gradle-lifecycle/gradletoolwindow.png)

### Tasks

Expand the **Tasks** folders to see tasks available for execution.

![gradle tasks](https://curriculum-content.s3.amazonaws.com/6002/gradle-lifecycle/gradletasks.png)

- Gradle's Base plugin provides general lifecycle tasks such as *clean*,
  *assemble*, and *build*.
- The Java plugin provides additional tasks related to compiling and
  packaging a Java project: *compileJava*, *processResources*, *classes*,
  *compileTestJava*, *processTestResources*, *testClasses*, *jar*, *javadoc*,
  *test*, and *cleanTaskName*.

We can get a description of each task by typing "gradle tasks --all"
in the terminal window.

## Build a project

Let's see how IntelliJ builds the project when we change the code
and need to recompile. 

Edit the `main` method to print "Hello" instead of "Hello World!", then:

1. Press the build icon on the bottom toolbar to display the build view.
2. Press the build icon on the top toolbar or
   select **Build | Build Project** from the main menu to build the project.  

![build](https://curriculum-content.s3.amazonaws.com/6002/gradle-lifecycle/gradlebuild.png)

Building from the main menu executes the **classes**
and **testClasses** tasks, along with other dependent tasks
based on the DAG. Tasks that are up-to-date or have no inputs are skipped.

What if we want to execute the **build** task itself?
There are two different ways to do this:

1. Right-click on the task in the Gradle Tool Window and select "Run":
   ![run build](https://curriculum-content.s3.amazonaws.com/6002/gradle-lifecycle/runbuild.png)
2. Click on the "Execute Gradle Task" icon and type "gradle build":
   ![run build#2](https://curriculum-content.s3.amazonaws.com/6002/gradle-lifecycle/runbuild2.png)


We can see the tasks executed as part of executing the **build** task.  Notice
the tasks related to testing are skipped because we haven't defined any Junit
test classes.

![build results](https://curriculum-content.s3.amazonaws.com/6002/gradle-lifecycle/buildresults.png)
## Running the project

We usually just want to run the program and see the output.
IntelliJ uses the Gradle build system to make
sure the code is compiled, necessary libraries and
resources are available, etc.

Let's run the `main` method to how the build process is triggered:

1. Edit the `main` method and change the code to print "Goodbye".  
2. Press the run button on the toolbar or select **Run | Run** from the main menu.
   Make sure the configuration next to the run button is set to
   run "Main" and not a Gradle lifecycle task.

![run main](https://curriculum-content.s3.amazonaws.com/6002/gradle-lifecycle/runmain.png)

Since we've changed the source code, the classes must be recompiled
before we can run the code. We can see the Gradle tasks associated
with compilation are executed prior to running the `main()` method.


## Conclusion

We learned about the Gradle build lifecycle and Directed Acyclic
Graph of tasks related to a Java build.  Gradle is efficient because
it avoids executing a task if the inputs have not changed.

## Resources

[Gradle Build Lifecycle](https://docs.gradle.org/current/userguide/build_lifecycle.html)    
[Gradle Java Plugin](https://docs.gradle.org/current/userguide/java_plugin.html)    

