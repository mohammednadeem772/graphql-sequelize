## GraphQL with Apollo-Server.

## Installation: 
   - Run: ```npm install``` or ```yarn install```
   
## Starting the Project
   - Run: ```npm start```
      
companydb.sql: Import this file to your MySQL

## For documentation and instructions check out 
https://nisar1406.medium.com/crud-operations-with-graphql-apollo-server-sequelize-auto-f6c334c41506



## Step 1 — Setting Up the Database
We’ll start by creating our database. For this tutorial, we’ll use a very simple database schema related to the company.

![image](https://user-images.githubusercontent.com/46413937/185749959-c07464d2-f696-4762-b4f1-47dc658b34a2.png)

## Step 2— Setting Up the Project
Let us create a new project.

If you don’t have yarn installed you can make use of npm also.

Creating a new directory and initiating the project.

![image](https://user-images.githubusercontent.com/46413937/185750000-240cbccc-9292-4a31-89ce-5fd2e10c0753.png)

2. Adding the required dependencies.

![image](https://user-images.githubusercontent.com/46413937/185750040-4bc99c2b-547c-4a39-910d-57b39978c27d.png)

## Step 3— Connecting the database to the project.
First, we need to initialize i.e we need to connect our database to the project using Sequelize-CLI.

![image](https://user-images.githubusercontent.com/46413937/185750072-6bcefa37-42cb-4c4d-85e8-e77cc95535c8.png)

   1) The first command will create a configuration file for the database, where we’ll be specifying all the database details.
   2) In config.json you need to modify the fields as per requirements. The file should look like this
   
      ![image](https://user-images.githubusercontent.com/46413937/185750142-6b6d7e99-0846-442a-ad91-230182b94ee9.png)
      
   3) As we have our database ready we will make use Database-First approach i.e generating models from an existing database. For doing this we will make use of
      
      ![image](https://user-images.githubusercontent.com/46413937/185750187-eb4690ce-2d2b-4dd0-b5e7-e92616c32f20.png)   
 
      -o, --output (What directory to place the models.[string])
      -d, --database (Database name.[string] [required])
      -h, --host (IP/Hostname for the database.[string] [required])
      -u, --user (Username for database.[string])
      -x, --pass (Password for database.[string])
      -p, --port (Port number for database (not for sqlite))[number]
                 Ex: MySQL/MariaDB: 3306, Postgres: 5432, MSSQL: 1433 
      -e, --dialect (The dialect/engine that your are using: 
                    postgres,mysql,sqlite, mssql [string])
              
   The following command will automatically generate a models folder (one file per model) which will consist of models from the database. Here in the model's folder, we will have employee, departments, designation models, and one init-models.js file which consists of database models and their relationships.

## Step 4— Setting up resolvers and typeDefs.
What are resolvers?

A resolver is a function that’s responsible for populating the data for a single field in your schema. Whenever a client queries for a particular field, the resolver for that field fetches the requested data from the appropriate data source.

What are TypeDefs?

typeDefs is a required argument and should be a string or array of GraphQL schema language strings or a function that takes no arguments and returns an array of GraphQL schema language strings. This is a required argument. It represents a GraphQL query as a UTF-8 string.

Query allow clients to request data (similar to a GET request in REST).

We will start by creating a graphql folder inside our root directory, which will consist of resolvers and typeDefs. Your project hierarchy should look like this.

![image](https://user-images.githubusercontent.com/46413937/185750258-813685a9-4e07-4941-8c6f-12e80777782b.png)



   Now let us create our first typeDef by creating a typeDefs.graphql file inside the graphql folder. Let us start with employees.

   Here we’ll start with a type. We’ll have an Employee type which looks like id, name, email, designation, etc. Once we define our type we need to fetch Employees and      this will be done by Query object which will be an array of an Employee.
   
   ![image](https://user-images.githubusercontent.com/46413937/185750304-649e274b-b6be-46d1-b1e6-36a93c0fcf9b.png)

   Here getEmployeeDetails is the name of a Query. You can give any name as per your requirements.

   Now let us implement the functionality for what we have defined so far by creating a resolvers.js file inside the graphql folder. This is a simple js file.

   This file will consist of Query resolvers. Here we’ll have a resolver function for Employee which will return an array of the employees. Your file should look like this.
   
   ![image](https://user-images.githubusercontent.com/46413937/185750328-427916ce-8546-4251-b8b4-cc7c815de6b5.png)

   Similarly, you can do this for departments and designations.

   Once we are done with typeDefs and resolvers we are good to create our server.js file. In the server.js file first, we need to import ApolloServer and gql from the apollo-server package.

   Now we can create out typedefs by using the gql function. To import our typeDefs.graphql file we will be using the fs (file system) module. We will use the fs.readFileSync function to read our schemas and we’ll be passing the encoding option and we’ll be setting it to ‘utf-8’ to make sure it reads the file as a string.

   Now we can create a new apollo server instance by passing a configuration object typeDefs and revolvers.

   Your server.js file should look like this.
   
   Now time to run our server. To start our server go to the command prompt and hit npm start. This will be running on http://localhost:5000, and we will see GraphQL Playground running.
   
   Let’s try getting all employee detail:
   
   {
  getEmployeeDetails{
    id,
    Name,
    Email
  }
}

   We will see a result as below:

   ![image](https://user-images.githubusercontent.com/46413937/185750405-a74314ca-e814-43b0-93d5-9254c7382867.png)

      Here we are only fetching data from the employee table. What if we want to fetch data for employees from multiple tables? Yes, we can do this, as we have already defined the relationship. Now we just need to modify our schema for employee as well as we need to add a new type for the department, designation. Our modified schemas should look like this.
   
   ![image](https://user-images.githubusercontent.com/46413937/185750457-02c6b35f-1d0d-4f67-8498-ace25df04aa9.png)

      here, we have added a designation field to employee and the type of this field is Designation. So in a custom type, like Employee, we can use another custom type like Designation, that’s the way we represent the associations between different objects. Similarly, we can do it for Designation and Manager.

   Now let us implement these associations in the resolvers. Actually, let’s see what happens if we don't change our resolvers. We will query getEmployeeDetails again with id, Name, Email, department. Since the department is an object with fields, we need to specify which fields we want, let’s ask for id and name. Now let us try to run the query.
   
   ![image](https://user-images.githubusercontent.com/46413937/185750488-67713d3c-c50f-4a6c-8da1-cc8f8a9227c5.png)

   You can see that we get some data back, but since we didn't update our resolver the department in each employee is always null. So how can we return a department for employees?

   We already have relationships defined in our database. We have department_id as a foreign key in the employee table. Here we can make use of department_id as a reference to fetch the department related data. As we already have declared in our schemas that every employee has a department field which is an object. Our resolver should reflect our schema.

   We need to declare a new resolver object for the department where we can put functions to resolve the fields of the department. Now how to return the right department from this function?

   Here each resolver function receives some arguments and the argument is the parent object. In this case, since we are resolving the designation for the employee, the parent object is an emp and we want to return the associated department and add it to the exported type. Our code should look like this:


![image](https://user-images.githubusercontent.com/46413937/185750522-7662e071-92c0-402b-ad28-5e7154843e00.png)

   So we are saying that return a department whose id is the same as that of department_id for the employee.

   Now let us run the same query and let’s check the output:
   
   ![image](https://user-images.githubusercontent.com/46413937/185750550-00e44fee-f4f8-487c-ab26-892a6d2a5cf0.png)

      We ran the exact same query as that before. Now the department field is not null anymore, they are objects with id and name.

   Similarly, you can do it for designations and manager.

   Here we are getting a list of all employees what if we want to get only specific employee details? To do that we need to add a new field to the query type, we just don’t want any employee details to be returned, we want one with a specific id. In GraphQL we can pass arguments to query using the following syntax

   ![image](https://user-images.githubusercontent.com/46413937/185750610-111b5266-23cc-4a96-9c0f-8a8051cd0b43.png)

   Here exclamation (!) mark represents that the ID cannot be null. Now we need to write a resolver:
   
   ![image](https://user-images.githubusercontent.com/46413937/185750671-d0929b5c-6a2a-4c36-8176-26a549134e1d.png)

   This function accepts two arguments where the root is the parent object and the second is id. Let's test this:
   
   ![image](https://user-images.githubusercontent.com/46413937/185750721-55055d32-0f04-4a8a-b4ee-776f66ba6533.png)

   Here we get only a single employee object with id:2

   Step 4 — Writing mutations.
   
   In our graphql schemas, we only have queries, it is all about returning existing data, there is nothing that modifies the data.

   In GraphQL the operations that modify the data are called mutations. They must be kept separated from the queries. We need to add a separate type called Mutation. The mutation is a root type just like the query. Inside mutation, we can find operations like create, update, and delete.

   Mutations allow clients to manipulate data (i.e., create, update, or delete, similar to POST, PUT, or DELETE, respectively).

   Below we have a mutation for adding a new employee, for that we need to pass the arguments for adding the employee. If you look at Employee type you have name, email, designation_id, department_id, manager_id. Here we don't have an id field because in the database we have set it to auto-increment.

   ![image](https://user-images.githubusercontent.com/46413937/185750748-6c7cd013-bcff-415f-8b92-4b775dacab57.png)
   
   Mutations must return a result just like queries. In our case, we’ll return a String.

In resolvers we need to match the structure of schemas, so we need to define a new object called Mutation and add it to the exported object.
   
   ![image](https://user-images.githubusercontent.com/46413937/185750788-b8ce7c02-6ae5-4845-ae81-c1d0eb8525ad.png)

Now we are ready to create a mutation function for adding an employee.

![image](https://user-images.githubusercontent.com/46413937/185750825-44de3d97-1436-4d50-9bdd-ef5c8dbc2d3c.png)

   Let us test this. Go back to your browser and hit the following command:

   mutation{
     createEmployee(
       Name:"Jane Doe",
       Email:"jane@gmail.com",
       Designation_id: 1,
       Department_id: 1,
       Manager_id: 1
     )
   }

   The output will look like this:  

   ![image](https://user-images.githubusercontent.com/46413937/185750860-a20ba3fa-9247-46b7-bc90-447a3a3e4d10.png)

   Ours create statement works successfully. Similarly, we can update the employee. We need to update our schemas and resolvers.

   ![image](https://user-images.githubusercontent.com/46413937/185750890-0f3b199c-e5a7-4af0-98cd-fb2c5a845344.png)

   ![image](https://user-images.githubusercontent.com/46413937/185750900-59ddbb1c-b832-4404-a49d-a1339d438c9c.png)


   Once we have code in place we can run the following :

   ![image](https://user-images.githubusercontent.com/46413937/185750910-d0b75368-0a99-427e-bbaf-01f6b6cbf59b.png)

   Our update statement works successfully and employee with id 1 gets updated.

   Now let us try to delete an employee. For this, we need to add one more function to mutations in schemas as well as resolvers.
   ![image](https://user-images.githubusercontent.com/46413937/185750935-a0d77072-c722-40b0-a5b8-3b41f76a6daa.png)

   Here we are deleting an employee based on employee id and returning a string.

   ![image](https://user-images.githubusercontent.com/46413937/185750961-379e695f-8fd4-411f-8e80-4119baa35fe6.png)

   Let’s test this:

   ![image](https://user-images.githubusercontent.com/46413937/185750988-a0728850-676d-42fc-af35-eb3ab71a9eb5.png)

   The employee with a specific id is deleted successfully.




