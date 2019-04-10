# Live Project
## Introduction
Over the course of the Live Project I learned many different aspects of building an MVC web application through the uses of HTML/CSS, Javascript, but mainly C#. We worked with our peers to help build something that will go into use, and that is pretty satisfactory because I know that the code we all wrote will be put to good use. Whether it was a simple bug fix to making a new view or adding other features in, the experience was valuable to obtain. I mainly worked on back end stories because I like to see what is going on "behind the scenes" in our code we wrote and seeing it be not only functional, but with no bugs and concise and nice looking code. I also did some front end stories as well, while I am not the best at front end work I do think I did a fairly good job making it work. I even learned how to do some other stuff besides writing code, this entails: working with team members, working with Github and Azure DevOps was definitely a great thing to learn how to do (not to mention how easy it is to use). Below I will "show off" some of the work I had done towards the project. 
## Front End Work
Some of the front end work I did was first I needed to delete some code but I feel that's not super important to show off so instead I will include some work that I did to help make the requested feature implemented (it is a long list of code in one file so I'm going to shorten it up).

First I had to fix some bugs with the code where it would get rid of the text boxes so that we could edit information, so I fixed that.
```
    <div class="form-group">
        @Html.LabelFor(model => model.JPStudent.LinkedIn, htmlAttributes: new { @class = "control-label col-md-2" })
        <div class="col-md-10">
            @Html.EditorFor(model => model.JPStudent.LinkedIn, new { htmlAttributes = new { @class = "form-control" } })
            @Html.ValidationMessageFor(model => model.JPStudent.LinkedIn, "", new { @class = "text-danger" })
        </div>
    </div>

    <div class="form-group">
        @Html.LabelFor(model => model.JPStudent.GitHub, htmlAttributes: new { @class = "control-label col-md-2" })
        <div class="col-md-10">
            <div class="checkbox">
                @Html.EditorFor(model => model.JPStudent.GitHub)
                @Html.ValidationMessageFor(model => model.JPStudent.GitHub, "", new { @class = "text-danger" })
            </div>
        </div>
    </div>
```
And then next I had to just add a checkbox with asking if the web application can "Contact you"
```
FROM THE VIEWS FILE
    <div class="form-group">
        @Html.LabelFor(model => model.JPStudent.JPContact, htmlAttributes: new { @class = "control-label col-md-2" })
        <div class="col-md-10">
            <div class="checkbox">
                @Html.EditorFor(model => model.JPStudent.JPContact)
                @Html.ValidationMessageFor(model => model.JPStudent.JPContact, "", new { @class = "text-danger" })
            </div>
        </div>
    </div>
    
NEXT FROM THE MODELS FILE
[DisplayName("Can we contact you?")]
```
## Back End Work
I was really happy with my work on some of the back end stories we worked on because I felt that I learned a lot, and made me only wanting to learn more. I worked on adding some stuff in like making certain views accessible to admins only or adding new views for the web application to use.

This first bit of code I did was making a view that allowed admin-users able to look at each application the student submitted, so I had to create a "Student" view
```
@model IEnumerable<JobPlacementDashboard_2.Models.JPApplication>

@{
    ViewBag.Title = "Student";
}

<h2>Student</h2>

<p>
    @Html.ActionLink("Create New", "Create")
</p>
<table class="table">
    <tr>
        <th>
            @Html.DisplayNameFor(model => model.Company_name)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.Job_title)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.JPJob_catagory)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.Company_city)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.State_code)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.Application_date)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.Student_id)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.Heard_back)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.Interview)
        </th>
        <th></th>
    </tr>

    @foreach (var item in Model)
    {
        <tr>
            <td>
                @Html.DisplayFor(modelItem => item.Company_name)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Job_title)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.JPJob_catagory)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Company_city)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.State_code)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Application_date)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Student_id)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Heard_back)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Interview)
            </td>
            <td>
                @Html.ActionLink("Edit", "Edit", new { id = item.Application_id }) |
                @Html.ActionLink("Details", "Details", new { id = item.Application_id }) |
                @Html.ActionLink("Delete", "Delete", new { id = item.Application_id })
            </td>
        </tr>
    }

</table>
```
Then I added in a new ActionResult method that would redirect you to a list of applications that students put in, as well as taking a Guid of a student_id
```
        public ActionResult Student(Guid? id)
        {
            List<JPApplication> list = new List<JPApplication>();

            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            list = db.JPApplications.Where(application => application.Student_id == id).ToList();
            if (list.Count() == 0)
            {
                return HttpNotFound();
            }
            return View(list);
        }
```
This next example I have is just a simple way to make pages admin-only accessible so that non-admin users cannot access this specific view. Like this:
```
        public ActionResult Index()
        {
            if (User.IsInRole("Admin"))
            {
                return RedirectToAction("StudentSummary", "Home");
            }
            else
            {
                return RedirectToAction("Index", "JPBulletins");
            }
        }
```
Or this:
```
        public ActionResult Delete(Guid? id)
        {
            if (User.IsInRole("Admin"))
            {
                if (id == null)
                {
                    return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
                }
                JPStudent jPStudent = db.JPStudents.Find(id);
                if (jPStudent == null)
                {
                    return HttpNotFound();
                }
                return View(jPStudent);
            }
            else
            {
                return RedirectToAction("Index", "Home");
            }
        }
```
I had done that for a good amount of views so let's move onto something else I did on the back end.

Something simple I had to do was just create a new controller with scaffolded views.
```
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.Entity;
using System.Linq;
using System.Net;
using System.Web;
using System.Web.Mvc;
using JobPlacementDashboard.Models;
using JobPlacementDashboard_2.Models;

namespace JobPlacementDashboard.Controllers
{
    public class JPHiresController : Controller
    {
        private ApplicationDbContext db = new ApplicationDbContext();

        // GET: JPHires
        public ActionResult Index()
        {
            return View(db.JPHires.ToList());
        }

        // GET: JPHires/Details/5
        public ActionResult Details(Guid? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            JPHire jPHire = db.JPHires.Find(id);
            if (jPHire == null)
            {
                return HttpNotFound();
            }
            return View(jPHire);
        }

        // GET: JPHires/Create
        public ActionResult Create()
        {
            return View();
        }
        ...
```
As far as anything else I worked on in this project it was either deleted or commenting out code or just working on bug fixing like making sure everything worked properly.

## Conclusion
I had learned a lot while we were doing the Live Project, it is really amazing how much you can learn from just working with others on a project that is meaningful and knowing your code is going to. And even if it wasn't going to anything, it was still an awesome experience working in a "real world" work place and working with others. 

### Skills learned
- Inner workings of CSHTML and making it work together.
- C# MVC, working with C# and using it's language to make a web application functional.
- CSS was fun because you get to play around with how the website looks, and it was cool getting to do just a little bit although I could have done more.
- Working in a team-oriented environment.















