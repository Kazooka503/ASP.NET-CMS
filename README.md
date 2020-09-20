![LiveProject Logo](http://www.austinkrzciok.com/img/lp_logo.jpg)

# Introduction:

My second Tech Academy Live Project was both an exercise in the Agile/Scrum method of Project Management and a great experience in diving into the inner workings of ASP.NET MVC and Entity Framework. I spent two weeks working with a team of fellow students in developing software designed for a theater company to manage its website content. 

Working with ASP.NET on a major project for the first time was an excellent learning opportunity to understand MVC & Entity Framework on a deeper level as well as experience in troubleshooting code that was written by other developers. I was requried to fix various bugs in the code and come up with solutions to a number of problems. 

Here you will find development stories related to my project with supporting code snippets.

# Project Stories:
* [Styling Fixes](#styling-fixes)
* [Deleting Photos](#problem-deleting-production-photos)
* [Creating Photos](#problem-creating-production-photos)
* [Other Skills Acquired](#other-skills-acquired)

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

Detatching the sections was a simple correction of correcting the nested ```</div>``` tags.<br> 
Once done the page worked perfectly!

![Corrected Styling](http://www.austinkrzciok.com/img/parts.png)

## Problem Deleting Production Photos:

The theater's productions have associated images for displaying a production's artwork and live shot photos.<br>
When a user attempted to delete the primary photo associated with a production ASP.NET produced an error.
I fixed this error by creating a variable that included the photo and its dependancy. I then used that variable 
to remove both. 

### Before:

```

	public ActionResult DeleteConfirmed(int id)
		        {
		            ProductionPhotos productionPhotos = db.ProductionPhotos.Find(id);
			    db.ProductionPhotos.Remove(productionPhotos);
		            db.SaveChanges();
		            return RedirectToAction("Index");
		        }

```

### After:

```

	public ActionResult DeleteConfirmed(int id)
		        {
		            // Removes Photo and dependencies on delete
		            var photoDependency = db.ProductionPhotos.Include(b => b.Production)
		                                         .FirstOrDefault(b => b.ProPhotoId == id);
			    db.ProductionPhotos.Remove(photoDependency);
		            db.SaveChanges();
		            return RedirectToAction("Index");
		        }

```

## Problem Creating Production Photos:

Uploading new photos for productions was also throwing an error. I was tasked to find a solution to this problem as well.
The first piece of thie puzzle was a ViewData issue. Attempting to upload a photo always produced a "There is no ViewData of type
```'IEnumerable<SelectListItem>' that has the key "x"```.<br>
I fixed this by including the ViewData in the POST method, where before it was only being incuded in the GET method. 

### Before:

```
	
	if (ModelState.IsValid)
            {
                Production production = db.Productions.Find(productionID);
                productionPhotos.Production = production;

                if (production.DefaultPhoto == null)
                {
                    production.DefaultPhoto = productionPhotos;
                }

                db.ProductionPhotos.Add(productionPhotos);
                db.SaveChanges();      

                return RedirectToAction("Index");
            }
            
            return View(productionPhotos);
        }
	
```

### After:

```
	if (ModelState.IsValid)
            {
                Production production = db.Productions.Find(productionID);
                productionPhotos.Production = production;

                if (production.DefaultPhoto == null)
                {
                    production.DefaultPhoto = productionPhotos;
                }

                db.ProductionPhotos.Add(productionPhotos);
                db.SaveChanges();      

                return RedirectToAction("Index");
            }
            ViewData["Productions"] = new SelectList(db.Productions.ToList(), "ProductionId", "Title");

            return View(productionPhotos);
        }
```

The second problem arrived occured immediately after solving the first. 
When a user attempted to upload a photo they were met with,<br>
``` The parameter conversion from type 'System.String' to type ''X' failed because no type converter can convert between these types```<br>
This turned out to a pretty simple fix. The issued lied with the binding for the Create method.<br>
Changing ```public ActionResult Create([Bind(Include = "Title,Description,Production")] ProductionPhotos productionPhotos, HttpPostedFileBase file)```<br>
to ```public ActionResult Create([Bind(Include = "Title,Description")] ProductionPhotos productionPhotos, HttpPostedFileBase file)``` fixed the problem. 


## Other Skills Acquired

* Direct experience in AGILE/SCRUM 
* Experience using Azure DevOps
* A deepend understanding of SQL and databases
* Exerience in communicating ideas and concepts to others, including fellow peers and project leads.
* Experience in sourcing data and information to overcome unexpected problems. 
* Experience debugging code other people have written
* Experience metting code standards

*Back to: [top](#introduction), [styling fixes](#styling-fixes), [deleting photos](#problem-deleting-production-photos), [creating photos](#problem-creating-production-photos)*


