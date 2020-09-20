![LiveProject Logo](http://www.austinkrzciok.com/img/lp_logo.jpg)

# Introduction:

My second Tech Academy Live Project was both an exercise in the Agile/Scrum method of Project Management and a great experience in diving into the inner workings of ASP.NET MVC and Entity Framework. I spent two weeks working with a team of fellow students in developing software designed for a theater company to manage its website content. 

Working with ASP.NET on a major project for the first time was an excellent learning opportunity to understand MVC & Entity Framework on a deeper level as well as experience in troubleshooting code that was written by other developers. I was requried to fix various bugs in the code and come up with solutions to a number of problems. 

Here you will find development stories related to my project with supporting code snippets.

## Styling Fixes:

Content on one of the pages that displayed details for the theater's production details was not centering properly on smaller screens.

![Bugged Styling](http://www.austinkrzciok.com/img/center.png)

In addition to this bug, there was a problem with one of the sections of the details page being attached to the section above it.

I solved the first problem by removing unneeded margin tags that were pushing the div too far to the left on smaller screens.
Fixing the second problem required setting display to None on the div card containing the section, that way when being viewed on smaller screen
sizes the section would not display.

### Before:

```        
	<!--Iterate through Parts for a specific Type of role and if there is data it will display the below item in the Parts card. If no data is returned it
            will not display-->
		        <!--===================================================
		              The below refactors the parts section
		        ===================================================-->
		        <!-- Parts Card Section visible when screen size is bigger-->
		        <!-- when screen size is med or bigger this will display. It is otherwise hidden-->
		        <!-- Parts Card Section -->
			        <div class="card mr-6 d-md-block" id="cardss">
	          <div class="card-header" id="head">
	            <b>Parts</b>
	          </div>

	        <!-- End Parts Card Section-->
	      </div>

```

### After:

```        
	   <!--Iterate through Parts for a specific Type of role and if there is data it will display the below item in the Parts card. If no data is returned it
            will not display-->
		        <!--===================================================
		              The below refactors the parts section
		        ===================================================-->
		        <!-- Parts Card Section visible when screen size is bigger-->
		        <!-- when screen size is med or bigger this will display. It is otherwise hidden-->
		        <!-- Parts Card Section -->
			        <div class="card d-none d-md-block" id="cardss">
	          <div class="card-header" id="head">
	            <b>Parts</b>
	          </div>

	        <!-- End Parts Card Section-->
	      </div>
```

Detatching the sections was a simple correction of correcting the nested ```</div>``` tags. 
Once done the page worked perfectly!

![Corrected Styling](http://www.austinkrzciok.com/img/parts.png)

