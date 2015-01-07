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

