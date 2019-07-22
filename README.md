<h1>C# .NET Live Project</h1>
<hr>
<h2>Introduction</h2>
<hr>
<h5>
For the last two weeks of my time  at the Tech Academy, I worked with my peers in a team developing a full scale MVC/MVVM Web Application in C#.  
<br>
<br>
Working on a legacy codebase was a great learning opportunity for fixing bugs, cleaning up code, and adding requested features. 
There were some big changes and challenges in the project that could have caused project delays.  
However, our team utilized our team’s strengths and banded together to complete the project in a timely manner.  
This live project demonstrated to me how a proactive and effective software developer works and what a software developer has to do 
to make a quality product. I worked on several back-end stories that I am very proud of.  
Over the two-week sprint I also had the opportunity to work on some other project management and team programming skills 
that  I am confident I will use again and again on future projects.  
<br>
<br>
Below are descriptions of the stories I worked on, along with code snippets and navigation links. 
I also have some full code files in this repo for the larger functionalities I implemented.
</h5>
<hr>
<h2>Back-End Task: Adding Job Seed Data to the Project</h2>
<hr>
<h5>
A major task for this live project was creating a way to seed test data to the database.  
This was a challenging back-end task because to ensure that the data would take over multiple migrations of the database, 
the seed data had to be integrated into the startup procedure when the web application first runs instead of in the Migrations Folder 
in the configuration file as is typically done with a MVC web application. 
Below is a snippet of the code that I created to solve this problem, and provide way to seed the database to ensure database migration integrity. 
</h5>
<h3>Method used in Startup.cs file to seed data to database:</h3>
<code>
  private void addDataToTables()
        {
            ApplicationDbContext context = new ApplicationDbContext();

            var jobs = new List<Job>
            {
                   new Job
                     {
                        Id = "101",
                        Email = "john.irons@companyname.com",
                        PasswordHash = "**********",
                        FirstName = "John",
                        LastName = "Irons",
                        WorkType = WorkType.Leadman,
                        PhoneNumber = "703-212-6573",
                        UserRole = "Wielder",
                        Suspended = false,
                        State = "Arizona",
                        County = "Yuma",
                        ZipCode = "58392",
                     },
                   new Job
                       {
                         Id = "201",
                         Email = "roberto.tran@companyname.com",
                         PasswordHash = "**********",
                         FirstName = "Roberto",
                         LastName = "Tran",
                         WorkType = WorkType.Foreman,
                         PhoneNumber = "424-369-1256",
                         UserRole = "Construction Manager",
                         Suspended = false,
                         State = "Florida",
                         County = "Orange",
                         ZipCode = "32819"
                        },
                  new Job
                        {
                            Id = "301",
                            Email = "bruce.wayne@companyname.com",
                            PasswordHash = "**********",
                            FirstName = "Bruce",
                            LastName = "Wayne",
                            WorkType = WorkType.ExpMBA,
                            PhoneNumber = "212-720-2071",
                            UserRole = "Senior Executive",
                            Suspended = false,
                            State = "New York",
                            County = "Gotham",
                            ZipCode = "53540"
                        }, 
                  new Job
                        {
                           Id = "401",
                            Email = "diania.prince@companyname.com",
                            PasswordHash = "**********",
                            FirstName = "Diana",
                            LastName = "Prince",
                            WorkType = WorkType.NewMBA,
                            PhoneNumber = "351-639-5488",
                            UserRole = "Corporate Lawyer",
                            Suspended = false,
                            State = "California",
                            County = "Cisco",
                            ZipCode = "53540"
                        }, 
                  new Job
                        {
                           Id = "501",
                            Email = "richard.parker@companyname.com",
                            PasswordHash = "**********",
                            FirstName = "Richard",
                            LastName = "Parker",
                            WorkType = WorkType.Foreman,
                            PhoneNumber = "963-675-1259",
                            UserRole = "Land Surveyour",
                            Suspended = false,
                            State = "Oregon",
                            County = "Klamath",
                            ZipCode = "97601"
                        }
            };
            jobs.ForEach(s => context.Jobs.AddOrUpdate(p => p.LastName, s));
            try
            {
                context.SaveChanges();
            }
            catch (System.Data.Entity.Validation.DbEntityValidationException ex)
            {
                var errorMessages = ex.EntityValidationErrors.SelectMany(x => x.ValidationErrors).Select(x => x.ErrorMessage);
                //Join the list to a single string. 
                var fullErrorMessage = string.Join("; ", errorMessages);
                throw new Exception(fullErrorMessage);
            }
        }
</code>
<h4>Snapshot of seed data in MVC Web Application</h4>
<img src="./images/Slide2.JPG" alt="Job Seed Data in Application"/>
<hr>
<h2>Back-End Task: Validating User Input When Creating Jobs</h2>
<hr>
<h5>
An important aspect for processing user input when uploading to a database is validating that the data entered is correct.  
If user input is not validated, then incorrect data can be added to the database.  To insure job information for employees are entered correctly, I coded into the Job Model parameters that would validate the properties of the model.  
Below is a code snippet of the validated model:
</h5>
<code>
    public class Job
    {
      [Key]
      [Required(ErrorMessage="Required Field. Please enter an ID #: "),Display(Name = "User Id")]
      public string Id { get; set; }
      [Required(ErrorMessage ="Required Field. Please enter a valid Email: "),Display(Name = "Email"),DataType(DataType.EmailAddress)] 
      public string Email { get; set; }  
      [Required(ErrorMessage ="Required Field. Please enter a Password: "),Display(Name = "PasswordHash")]
      public string PasswordHash { get; set; }
      [Required(ErrorMessage ="Required Field. Please enter a First Name:"),Display(Name = "First Name")]
      public string FirstName { get; set; }
      [Required(ErrorMessage ="Required Field. Please enter a Last Name: "),Display(Name = "Last Name")]
      public string LastName { get; set; }
      [Required(ErrorMessage ="Required Field. Please enter a Work Type >> Leadman,Foreman,ExpMBA, or NewMBA: "),Display(Name = "Work Type")]
      public WorkType WorkType { get; set; }
     [Required(ErrorMessage = "Required Field. Please enter a valid Phone Number: "),Display(Name = "Phone Number")]
     public string PhoneNumber { get; set; }
     [Required(ErrorMessage = "Required Field. Please enter a User Role: "),Display(Name = "User Role")]
     public string UserRole { get; set; }
     [Required(ErrorMessage ="Required Field. Please enter a State: "),Display(Name = "State")]
     public string State { get; set; }
     public IEnumerable<SelectListItem> States { get; set; } 
     [Required(ErrorMessage ="Required Field. Please enter a County: "),Display(Name = "County")]
     public string County { get; set; }
     [Required(ErrorMessage ="Required Field.  Please enter a Zip Code: "),Display(Name = "Zip Code")]
     public string ZipCode { get; set; } 
     [Display(Name = "Suspended")]
     public bool Suspended { get; set; }
    }
</code>
