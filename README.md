entity framework 
	ContosoUnivesity
	
1
			DatabaseGenerated
			DropCreateDatabaseIfModelChanges
			Web.Config
				databaseInitializer 
				disableDatabaseInitialization="true"
			http://www.entityframeworktutorial.net/code-first/database-initialization-strategy-in-code-first.aspx
			
2
	[Bind(Include = "ID,LastName,FirstMidName,EnrollmentDate")]			
		 Include - whitelist principe
		 Exclude - blacklist principe
		 
		 
ValidateAntiForgeryToken 

@Html.AntiForgeryToken()

TryUpdateModel
	http://www.asp.net/whitepapers/aspnet-data-access-content-map
	
	
3 


4

DbConfiguration
IDbExecutionStrategy

SqlAzureExecutionStrategy + RetryLimitExceededException


Best practice to logging

http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log


DbCommandInterceptor


5

http://msdn.microsoft.com/en-us/library/hh829293(v=vs.103).aspx
 
Configuration : DbMigrationsConfiguration<SchoolContext>

MigrateDatabaseToLatestVersion 

public SchoolContext() : base("SchoolContext")
        {
            Database.SetInitializer(new MigrateDatabaseToLatestVersion<SchoolContext, Configuration>());
        }

6
7 
8 

9 Async and Stored Procedures with the Entity Framework in an ASP.NET MVC Application

Four changes were applied to enable the Entity Framework database query to execute asynchronously:

The method is marked with the async keyword, which tells the compiler to generate callbacks for parts of the method body and to automatically create the Task<ActionResult> object that is returned.
The return type was changed from ActionResult to Task<ActionResult> . The Task<T> type represents ongoing work with a result of type T.
The await keyword was applied to the web service call. When the compiler sees this keyword, behind the scenes it splits the method into two parts. The first part ends with the operation that is started asynchronously. The second part is put into a callback method that is called when the operation completes.
The asynchronous version of the ToList extension method was called.


Some things to be aware of when you are using asynchronous programming with the Entity Framework:

The async code is not thread safe. In other words, in other words, don't try to do multiple operations in parallel using the same context instance.
If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.

MapToStoredProcedures

protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
	...
	modelBuilder.Entity<Department>().MapToStoredProcedures();
}

10 Handling Concurrency with the Entity 
public class Department
{
...
	[Timestamp]
    public byte[] RowVersion { get; set; }
...
}

modelBuilder.Entity<Department>().Property(p => p.RowVersion).IsConcurrencyToken();


public async Task<ActionResult> Edit(int? id, byte[] rowVersion)


try
        {
            db.Entry(departmentToUpdate).OriginalValues["RowVersion"] = rowVersion;
            db.Entry(departmentToUpdate).State = EntityState.Modified;
            await db.SaveChangesAsync();

            return RedirectToAction("Index");
        }
        catch (DbUpdateConcurrencyException ex)
        {
		
		
var databaseValues = (Department)databaseEntry.ToObject();


@Html.HiddenFor(model => model.RowVersion)


11 Implementing Inheritance with the Entity Framework 6 in an ASP.NET MVC 5 Application 

This code takes care of the following database update tasks:

Removes foreign key constraints and indexes that point to the Student table.
Renames the Instructor table as Person and makes changes needed for it to store Student data:
	Adds nullable EnrollmentDate for students.
	Adds Discriminator column to indicate whether a row is for a student or an instructor.
	Makes HireDate nullable since student rows won't have hire dates.
	Adds a temporary field that will be used to update foreign keys that point to students. When you copy students into the Person table they'll get new primary key values.
Copies data from the Student table into the Person table. This causes students to get assigned new primary key values.
Fixes foreign key values that point to students.
Re-creates foreign key constraints and indexes, now pointing them to the Person table.


