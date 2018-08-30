# Chapter 3 - Play MVC Framework

Now that we've spent some time working with functional programming concepts,
and have begun writing in Scala, it is time to take a look at one of the more popular
Scala frameworks, Play! Which supports both Java and Scala as a lightweight MVC
framework for web applications.

## Play Documentation

One of the benefits of utilizing Play is its
[extensive web documentation](https://www.playframework.com/documentation/2.6.x/Home).
Here you will find more fine-tuned examples of the various libraries that Play provides.

## Chapter Overview

This chapter will walk you through creating various web applications utilizing the
Play framework. We will progress from a simple Hello World application, to a Play
hosting a React front end, to a Vending Machine Application, moving on to creating
our own chat application.

## Outline
  * Section 1 - Hello World
  * Section 2 - Play with React
  * Section 3 - Play with Slick Evolutions (Vending Machine)
  * Section 4 - Play with SSE
  * Section 5 - Play with Websockets (Chatroom)

## 3.1 - Hello World
In this section, we'll walk through setting up and running a barebones application from a seed.
Then, we'll make the changes necessary to display a simple "Hello, World!" page.

### Setting Up an Empty Play App
The following instructions will set up an extremely basic Play application
that displays "Welcome to Play!" on the index page.

#### Using SBT (From the [Play 2.6.x documentation](https://www.playframework.com/documentation/2.6.x/NewApplication))
To create a barebones Play project using sbt, we can use a giter8 seed.
Be sure that your current working directory is where you want to create the project, then run the following command:

`sbt new playframework/play-scala-seed.g8`

#### Using IDEA (From the [JetBrains tutorials](https://www.jetbrains.com/help/idea/getting-started-with-play-2-x.html))

There is more than one way to set up a template Play Framework application in IDEA;
however, for consistency with the seed-based version from the SBT section, use the following method:

1. Open the **New Project** wizard and select **Scala** on the left,
then choose **Lightbend Project Starter** in the righthand pane.
1. Click the **Next** button to continue.
1. Provide a name and directory for your application. Giving the project and module the same name
is not always the best choice, but go ahead and do that as our application will be very simple.
1. Select **Play Scala Seed**.
1. Select the Java SDK for the Project (typically 1.8).
1. Click **Finish**.

Congratulations! You are now ready to run your first application with Play!

### The Hello World application
Now that we've created the basic application, we'll run it to see what it does.
Then, we can find and modify the proper files to display our own "Hello, World!" message.

#### Running the App...
If you followed the instructions in the previous section, you should already be able to run the application!
Let's do that now.
##### Using SBT (Preferred)
In a terminal, navigate to the root directory of your project and run `sbt`.
Once you are presented with the interactive prompt, enter `~run`.
The tilde (`~`) before the run command enables hot reloading &mdash; sources modified
in the project are detected and updated on the running application.
Without the tilde, you may need to run the `compile` command before running the app.

To stop the running application, follow the prompts or use `^C`.

*Note: you can also type `sbt ~run` on the command line, but you should get into the habit of keeping sbt open
when running multiple commands. Otherwise, every command loads a new JVM to launch sbt and run the command.*

##### In IDEA (using SBT)
There are other ways to run/re-run the application in IDEA, but we'll be using SBT from within the IDE for
consistency with the above approach. Below are two basic ways to run the app that won't force you to leave IDEA.

###### SBT Shell
In IDEA, select **View** > **Tool Windows...** > **sbt Shell**.
Once the shell loads completely, enter `~run` at the prompt.
This method is supposed to be better (more interactive) than the next one,
but it sometimes doesn't accept input like it would when run in a terminal.

###### Run Configuration (SBT Task)
In IDEA, select **Run** > **Edit Configurations...**

Add a new **sbt Task** configuration. Name it something descriptive like "Run Hello".
Check the box for "Single Instance Only" to be safe, as the application will use networking resources.
Be sure the working directory is set to your project root. In the tasks textbox, enter `~run`. Save the configuration,
then select it on the Run dropdown and click the green arrow.

#### Viewing the Application
Once you've started your application, navigate to the application's index page at
http://localhost:9000/, which is the default for Play Framework in development mode.

#### Modifying the Application
##### Analyze the Existing App
Right now, we're displaying "Welcome to Play" in both the title and content of our index page.
Let's find out what we need to update in order to change this text.

###### Routes
First, check the Routes file (*app/conf/routes*). This file maps incoming requests to controller actions or resources.

Inside the routes file, the root "/" GET action is mapped
to a controller action (`index`) in `controllers.HomeController`.
This controller's source code, including the action, is defined in *app/controllers/HomeController.scala*.

###### The Controller
Take a look at the controller's source. A Play controller may define many actions &mdash; e.g.,
when managing interactions with a resource &mdash; but this one only defines the `index` action.
We can see that this action is rendering the *index* view with status OK (HTTP 200).
The source for that view is *app/views/index.scala.html*.

###### The View (Twirl Templates)
The "*.scala.html" extension indicates a
[Twirl template](https://www.playframework.com/documentation/2.6.x/ScalaTemplates).
If you've ever worked with JSP or any other server-side web template engine before, Twirl is just like that.
Twirl templates are transpiled to Scala Objects &mdash; for example, you would find
*index.template.scala* in subdirectories of the *target* folder.

The source of *index.scala.html* should look like this:
```scala
@()

@main("Welcome to Play") {
  <h1>Welcome to Play!</h1>
}
```

The `@()` at the beginning of the template defines any arguments
to the "apply" function that are needed to render the template.
Think of it as a method signature &mdash; it is required for every Twirl template,
even if no arguments are necessary as in this case.

The invocation of `main` here refers to `views.html.main`, which as you may guess,
comes from *app/views/main.scala.html*. Invoking main here will render it with the arguments:
1. "Welcome to Play"
2. The HTML content delineated by curly braces. *Note: Pay special attention to the syntax &mdash;
the opening curly brace must be on the same line as the end of the previous argument,
or the HTML block isn't interpreted as an argument. This is a common pitfall for Twirl beginners.*

The main template is well-documented, so read through to understand what it does.

##### Make Our Changes
Since the main template is just a reusable wrapper, we can safely ignore it.
The index template is where we should make our changes. Update both the title text
and the heading with a Hello World message, then reload the index page.

#### But Wait! What about testing?
You might have noticed when you cloned the seed application that it contains a test package.
There is only one test class, `HomeControllerSpec`. And it doesn't just test `HomeController`,
it tests the rendered view! In a Test-Driven Development scenario, we would have:
* updated these tests first with our Hello World text requirements;
* run the tests to fail; and, finally,
* updated the template and re-run the tests to pass.

Update the tests referencing "Welcome to Play", then run them to make sure they pass.

