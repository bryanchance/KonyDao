# Data Access Object Generator for Kony WebSQL Javascript API

## What?

Model your database in java and generate a Kony database interface for your app. Avoid coding WebSQL this way entirely.

**It covers** 
Operations: Insert, Update, DeleteWhere, GetWhere, GetByPrimaryKey, Clear, GetCountWhere
Keep Section: Write your own code into the interface without it being deleted when you recreate it. 

**coming up:** Relationsships, Joins

Everyone is invited to contribute :)

## Usage 

### Step 1. Get the Dao Generator

Create a [submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules) or directly clone/download the [KonyDao Repository](https://github.com/konydeveloper/KonyDao) in your Kony project directory. Call it KonyDao for example. This is the Kony dao generator library which contains all the code required to generate your javascript file.

### Step 2. Import the library

As a java project into your workspace or simply copy your code into your project of step 3.

### Step 3. Create your modeling project

Create a java (eclipse) project inside your kony project. Call it MyProjectDaoGenerator for example. Here you will model your database and trigger javacript file generation.

### Step 4. Setup your modeling project

In your modeling project, add the KonyDao library to your build path. In eclipse, go to your project properties -> Java Build Path -> Projects -> Add -> then select "KonyDao" in our example.

### Step 5. Model your database

Create a class following this example:

```
import static de.arvato.konydao.model.Property.INTEGER;
import static de.arvato.konydao.model.Property.TEXT;
import static de.arvato.konydao.model.Property.REAL;

import java.io.File;

import de.arvato.konydao.model.Entity;
import de.arvato.konydao.model.Schema;

public class KonyDaoSampleDaoGenerator {
	
	public static final int DB_VERSION = 1;
	
	public static void main(String[] args) {
		Schema db = new Schema("konydaosampledb", "Kony Dao Sample Database", DB_VERSION);

		// Create Table Customer
		Entity customer = db.addEntity("Customer");
		
		// Add columns to Customer
		customer.addProperty(TEXT, "id").primaryKey();
		customer.addProperty(TEXT, "firstname");
		customer.addProperty(TEXT, "lastname");
		customer.addProperty(INTEGER, "gender");
		customer.addProperty(INTEGER, "age");
		customer.addProperty(REAL, "satisfaction");

		// Create Table Address
		Entity address = db.addEntity("Address");
		
		// Add columns to Address
		address.addProperty(TEXT, "id").primaryKey();
		address.addProperty(TEXT, "customerid");
		address.addProperty(TEXT, "street");
		address.addProperty(TEXT, "housenumber");
		address.addProperty(TEXT, "zipcode");
		address.addProperty(TEXT, "country");

		// Generate the javascript file from modeled entities (this is the correct path of kony visualizer 7)
		db.generate(new File("../modules/database.js"));
	}
}
```

### Step 6. Generate your interface
Execute your generator. You should have a generated javascript file called "database.js" (as in example) inside your modules folder of your kony project now.

### Step 7. Attach your interface in your app code.

Implement your code in your kony project following these examples:

**Open your database and initialize it (create tables) if necessary**
```
databaseMaster.openDatabase();
if(!kony.store.getItem("DBInit")) {
	databaseMaster.initDatabase();
	kony.store.setItem("DBInit", true);
}
```

**Do your operations using these signatures**

```
databaseMaster.insertCustomer(entity, successCallback, failCallback);
databaseMaster.updateCustomer(entity, successCallback, failCallback);
databaseMaster.deleteCustomer(whereString, successCallback, failCallback);
databaseMaster.getCustomers(successCallback, failCallback, whereString);
databaseMaster.getCustomer(primaryKey, successCallback, failCallback);

databaseMaster.clearCustomers(successCallback, failCallback);
databaseMaster.getCustomerCount(whereString, successCallback, failCallback);
databaseMaster.printCustomers();
```

where the entity might be as follows:

```
var customerNew = {
    id:"1000",
    firstname:"Hans",
    lastname:"Zimmer",
    gender:1,
	age:59
};
```

Check out the [sample project](https://github.com/konydeveloper/KonyDaoSample) for more code how to implement the different functions. But honestly, just check out your generated interface. It's pretty intuative.
