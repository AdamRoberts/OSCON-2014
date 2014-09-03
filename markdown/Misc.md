##Misc
Note: There is so much that was covered at OSCON that I can't ever try to mention everything in a short presentation.

But here is some random interesting tidbits.


######Android
##RetroFit
[square.github.io/retrofit](http://square.github.io/retrofit)
Note: RetroFit is an open source library that allows Android java clients to access REST services in a type safe manner.


######Android
##Picasso
[square.github.io/picasso](http://square.github.io/picasso)
Note: Picasso is an open source library that makes it easy to download, display and cache images.


######Android
###GenyMotion
[www.genymotion.com](http://www.genymotion.com)
Note: GenyMotion is a commercial tool that gives you virtual machines images that mimic all of the most popular Android devices. 

This makes testing for different devices much more accurate than the free android emulator.


###### Fault Tolerance 
![](images/Misc/RabbitMQLogo.png)
Note: RabbitMQ is a message brokering queue written in Erlang.  The New York Time uses RabbitMQ deployed in the Amazon Cloud to handle their global business.

They have configured RabbitMQ in a mesh network with instances running in every Amazon region.

Their design is so robust that an IT employee accidentally deleted every single server from an entire Amazon region and the end users never noticed.


###### OpenGL 3D Library
## Amino 
[https://github.com/joshmarinacci/aminogfx](github.com/joshmarinacci/aminogfx)
Note: Amino is a library for Java or JavaScript that allows the caller to directly render OpenGL 3D graphics.

The libraries work on normal computers, but they were designed to run on the Raspberry PI to write 3D monitoring dashboards. 

If you have been down to IT you have probably noticed their wall of monitors with dashboards monitoring the status of PROS IT network. 

Now imaging rather than those relatively simple data displays running on an expensive computer, you could display more information on a $35 dollar computer.


######NetFlix API Design
#### One Size Doesn't Fit All
![](images/Misc/NoRestApi.png)
Note: NetFlix gave quite a few talks, but the one that I though was most interesting was on how to do service API design. They started 
by discussing how everyone has started using Resource based API design after the REST services became so popular. But after trying REST 
services for a time they found that Resource Based APIs make for a bad user experience.  

If you have a screen that displays 3 or 4 resources at the same time, the user doesn't want the different portions of the screen to redraw 
when the individual requests respond, the user wants the entire screen to appear quickly. Now in NetFlix case they don't actually control 
the user experience, that is created by one of their hundreds of certified partner's devices.  

So they solved this problem by providing Java Interfaces for each of the Resources that they provide.  Then the partner writes a Groovy script
that makes use of the provided Java Interfaces to generate a custom Experience-Based API for their specific device.  This script then runs on
NetFlix's servers as shown in the purple band on the diagram.


###### Better Presentations 
### revealJs + MarkDown + GitHub
[fghaas.github.io/oscon2014-presentationtoolbox](http://fghaas.github.io/oscon2014-presentationtoolbox/#/)
Note: I had used revealJs before on my introduction to Gradle presentation, and I liked it better than PowerPoint but it still wasn't perfect.

The Gradle presentation was built using standard HTML and by assigning the correct class to the different DIV tags it became a presentation.

But this presentation on how to prepare a presentation by Florian Hass now has me hooked.  He showed how you can just create a DIV tag and point it to a Markdown file.  

If you don't know Markdown yet, you will be learning soon as that is the Wiki language used by both GitHub and Stash.

So Florian showed how easy it is to create your slides using Markdown text, he then showed how to use GitHub pages for free hosting of your presentation.

Show Notes here!


###### Philanthropy 
[![GirlDevelopIt.com](images/Misc/girldevelopit.png) ](http://girldevelopit.com/)
Note: There were a large number of not for profit organizations represented at OSCON, I am just going to mention two.

Exists to provide affordable and accessible programs to women who want to learn web and software development through mentorship and hands-on instruction.

The Houston chapter is in the process of starting now.


###### Philanthropy
[![www.ushahidi.com](images/Misc/ushahidi.gif) ](http://www.ushahidi.com)
Note: Ushahidi is the swahili word for "testimony", and this not for profit started as a website to map the violence in Kenya after their election in 2008.

Now they are a not for profit that is attempting to use the that is latent in the fact that even in the remotest parts of the globe people now have cell phones. 
They aren't making apps that run on fancy smart phones, they are working on tools to allow every phone be used to solve local problems.

One example they gave was in the rural arid middle east (turkey) where clean water shortages due to malfunctioning wells was a
leading cause of health issues. They setup a program where anyone could text in the current status of the local well, at first they didn't get any
participation, but after they started paying the person doing the notification in cell phone minutes they started getting extremely frequent notifications,
both of when a well is down, but also when the well is fine.  The savings by not having to send well repairmen to check working wells more than
paid for the cell phone minutes.

Ushahidi has software projects on Github, so if you are interested in contributing it is easy to get started.


### Broken Build?
![](images/Misc/Furby.png) 
Note: This if a Furby, if you haven't heard of a Furby before you are lucky, they are one of the most irritating toys ever invented,
if you stop playing with them they will actually complain.

This image is from a presentation that I actually didn't attend but wanted to. 

This team wired an arduino board to a Furby so that whenever a build breaks the furby starts to complain until the build goes green again.  Talk about evil.


### Business Plans?
###### Simon Wardley
[![Simon Wardley OSCON 2014 Keynote: "Introduction to Value Chain Mapping"](http://img.youtube.com/vi/NnFeIt-uaEc/0.jpg)](https://www.youtube.com/watch?v=NnFeIt-uaEc)
Note: All of the OSCON keynote speaches are available for free on YouTube. 

This keynote was my favourite, Simon Wardley gave a talk about how most business plans are not worth the time to read, they are filled with meaningless buzzwords that give no actual direction to the company.

Hey then goes on to discuss a technique called value chain mapping that can be used to create a business plan that will actually help guide the company through the changing business environment.


### The Concert Programmer
### Andrew Sorensen
[![Andrew Sorensen OSCON 2014 Keynote: "The Concert Programmer"](http://img.youtube.com/vi/yY1FSsUV-8c/0.jpg)](https://www.youtube.com/watch?v=yY1FSsUV-8c)
Note: This keynote was just for pure fun, the presenter starts with a small program that generates a beat.  He starts the program running in a repl, so the audience can hear it. 

Then he modifies the program on the fly, and every time he hits save the repl updates to turn the simple beat into music.

Honestly you need to watch the video to understand how impressive this is.