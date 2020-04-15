# MonthlyScheduler.Blazor

Very simple and straight forward goal. Use balzor to create a super simple application, use several features and components of blazor.

## Prerequisites
* Visual Studio 2019 or later
* Dot Net Core 3.1 or later

## Roadmap
- [x] Basic Implementation
    - [x] Creating Blazor Application
    - [x] Separate business logic and ui
    - [x] Dependency Injection
    - [x] CRUD operation
    - [x] Basic form validation
- [ ] Using Dialog
- [ ] Blazor application with external Web API
- [ ] Blazor WebAssembly

## Implementation
* [Basic Implemanation](https://github.com/abuzaforfagun/MonthlyScheduler.Blazor/tree/Basic-Implementation)
* [Dialog](https://github.com/abuzaforfagun/MonthlyScheduler.Blazor/tree/Dialog)

## Contribute

You are welcome to extend this repository. Please clone and create your feature specific folder at root, implement the feature with a well-defined readme file and create a pull request. You are done :)

## Creating Application
* Open visual studio and select Create New Project.
![Creat Blazor App 0](https://user-images.githubusercontent.com/24603959/79050322-0ff00100-7c4b-11ea-8750-371e68f3d77e.JPG)

* Search for Blazor App, select Blazor App Template and click on Create button.
![Creat Blazor App 1](https://user-images.githubusercontent.com/24603959/79050359-5b0a1400-7c4b-11ea-9aed-c049f47c33a1.JPG)

* Add your project name, select the location and click on Create button.
![Creat Blazor App 2](https://user-images.githubusercontent.com/24603959/79050390-812fb400-7c4b-11ea-9203-6982de0db44a.JPG)
 
* Select .NET Core 3.1, select Blazor Server App or later and click on Create button.
![Creat Blazor App 3](https://user-images.githubusercontent.com/24603959/79050413-a6bcbd80-7c4b-11ea-962b-c64145674cf6.JPG)
 
## Add domain project reference
To make the application decoupled and improve the reusability, we need to create a domain project and implement the business implementation theirs.
* Right-click on the solution explorer, navigate Add and select New Project.
![Add class library 0](https://user-images.githubusercontent.com/24603959/79051289-36189f80-7c51-11ea-8713-9e7f4a050a4d.JPG)

* Select Class Library template and click on Next.
![Add class library 1](https://user-images.githubusercontent.com/24603959/79051831-38c8c400-7c54-11ea-84ac-1a627c2c6e4c.JPG)

* Enter the library name and click on Create
* Right-click on dependencies of MonthlyBillScheduler.Server project and select Add Reference.
![Add reference](https://user-images.githubusercontent.com/24603959/79051910-ab39a400-7c54-11ea-9121-d305aeffa542.JPG)

* Check the domain project and click on the Ok button.
![Add reference 2](https://user-images.githubusercontent.com/24603959/79051919-b987c000-7c54-11ea-8ce7-c51970bbdcf2.JPG)

## Add bill service
Our goal to deal with monthly bills. We need to display the bill list on our dashboard. And because of that, we need to create a service to get a list of bill.
* Create a new folder in MonthlyBillScheduler.Domain project and name it Models. We are going to put all models there.
* Create a BillItem class with the following properties.
```public class BillItem
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Description { get; set; }
        public decimal Amount { get; set; }
        public DateTime CreatedOn { get; set; }
        public DateTime? ModifiedOn { get; set; }
    }
```
* Create another folder inside that project and name it Services. We are going to add our necessary services there.
* Because of the Dependency Inversion principle we want to talk to the interface. So let's create an interface first and name it IBillService. And add GetAll method declaration there for now. We will add more later. The declaration will be like below.
```Task<List<BillItem>> GetAll();```
* Add the implementation class, name it BillService. For simplicity, we are going to return some hardcoded values there.

## Add bill component
We need to add a component to display the bill list that we got from the bill service.
* Right-click on Pages folder of MonthlyBillScheduler.Server project, navigate Add and click on New Item.
![Add razor component](https://user-images.githubusercontent.com/24603959/79052284-1d12ed00-7c57-11ea-9d0a-e24b77363260.JPG)

* Select Razor Component, use Bills as the component name and click on Add button.
![Add razor component 2](https://user-images.githubusercontent.com/24603959/79052501-c3132700-7c58-11ea-8911-74d8da6a1daf.JPG)

We need to add a markup template inside the newly created razor component. For the very first let's add the page directive there. We need to specify the route path too.
```@page "/bills"```
Here bills is our path. So we will get this page when we navigate to {application base url}/bills.

To follow the Single Responsibility Principle and Open Close Principle, we need to segregate C# code from the razor page.
* Create a new class and name it BillsBase.
![image](https://user-images.githubusercontent.com/24603959/79052740-657fda00-7c5a-11ea-941c-17fb6b049863.png)

* Inherit it from ComponentBase. This class is provided by Razor to establish communication between view and the c# class.
* Remove the code block from razor page.
* Add bills routes in the navigation bar. To do the following, we need to add bills item inside NavMenu.razor file.
```
<li class="nav-item px-3">
    <NavLink class="nav-link" href="bills">
        <span class="oi oi-list" aria-hidden="true"></span> Bill List
    </NavLink>
</li>
 ```

## Add services dependency injection

To follow the dependency inversion principle we need to do the dependency injection of our BillService. 
* Add the following code on the ConfigureServices method of Startup class.
```services.AddScoped<IBillService, BillService>();```

## Integrate bills features

We want to display the listed bill schedules. For simplicity, we are going to use hardcoded dummy data. And we already added them under BillService class. 

* Do the dependency injection of BillService. We use ```[Inject]``` attribute to do so. Inject is an attribute provided by Blazor itself. We can use this only at the component level.
* Get the bill list and set to a property from bill service. In ASP .NET/ASP .NET Core application we use to do the data retrieving in the constructor method. But in Blazor you need to initialize the data inside OnInitialized method. There is asynchronise and synchronise version available for that method.
* Add the ```@inherits BillsBase``` directive on the top of our Bills.razor file. So now we can access the code behind in our razor file.
* Add a bootstrap table template.
* Use razor syntax to populate data in the following table.

![BillsBase Class](https://user-images.githubusercontent.com/24603959/79069019-cce66a00-7cec-11ea-8d84-466d3045a8da.JPG)
![Bills Razor Template](https://user-images.githubusercontent.com/24603959/79069022-ceb02d80-7cec-11ea-8423-5589326fbc44.JPG)

## Extend bill service with add bill feature

For simplicity again we are going to deal with the in-memory list. 

* Add method signature in ```IBillService``` interface.
* Implement the method in ```BillService``` class. Previously we generate and return the bill list from GetAll method. As we need to add a new bill using our Add method we need to move the initial list creation and data manipulation in constractor.
* Previously we inject BillService as a Scopped dependency. So that a single request life cycle uses the same implementation details. As we are using an in-memory list, its better to use Singleton, we can get the same result for multiple requests in the application life cycle. Open Startup.cs file and change the line ```services.AddScoped<IBillService, BillService>();``` by ```services.AddSingleton<IBillService, BillService>();```.

## Build up add fill feature razor components

* Like above we need to create a new Razor component, named ```BillForm```, we will use the same one for update bill. And because of that BillForm is the most appropriate name.
* Add BillFormBase class and inherit it from ```ComponentBase```.
* Add page directive as ```@page "/billform"``` to add route of '/billform'. So we can access this page using this route.
* Add inherits directive as ```@inherits BillFormBase``` to talk to the base class.
* Add a bootstrap form template to make it a little good.
* Use ```EditForm``` component insisted on using html form element. EditForm is provided by Blazor itself.
* We need to add a model attribute to bind the form data. Because of that, we have created a property inside BillFromBase class, named it Bill, and we used this as the value of the Model attribute.
* There is a couple of events available in EditForm component. We are going to fire an event when the user submits the form with valid data. We talk about the validation later. And ```OnValidSubmit``` event executed when a form submitted with valid data. So we create a method named ```AddBillEntry``` and use this as the value of OnValidSubmit.
* Add a button with submit type
* We need to inject BillService as like above. And call the newly added ```Add``` method to do the operation of adding a new bill.
* We need to add a newly created route in NavMenu razor component.

![Bill Form](https://user-images.githubusercontent.com/24603959/79104379-48e5be00-7d90-11ea-86cc-795ce8f6e681.JPG)
![Bills Base](https://user-images.githubusercontent.com/24603959/79104381-4b481800-7d90-11ea-94b6-50ecbd7ef7ed.JPG)


## Add edit bill feature

* To edit a bill we are going to use the same form that we created last time. We need to add a parameter over the component. To use the same form for adding and editing bill we need to add another page directive in our razor component. ```@page "/billform/{BillId}"```, here BillId would be mapped if the routes come with any parameter. We also need to add BillId property in our BillFormBase class.
* Insisted on creating Bill object we need to checkout BillId having value or not. If we found, BillId is null, need to create a new BillItem object otherwise, get the existing bill object from BillService. We know, ```OnInitialized``` is the best place to do that.
* We need to extend our IBillService to get a single bill from the list.
* Add an action button to go to the edit form in our Bills razor component.
![Add action button](https://user-images.githubusercontent.com/24603959/79109523-530cba00-7d9a-11ea-978b-f460c86985d2.JPG)
* Currently, we are using ```Add``` method of bill service to add a new bill. We need to change the implementation, as we are going to use the same component for add a new bill and update the existing ones.
* We changed our Add method to Upsert. And for clarification, the duty of upsert is inserting new objects if it is not available otherwise update the existing one. Note: as we are using an in-memory list, so we are using a random number as Id. Please keep in mind that for simplicity we are using manual mapping and this is not a good practice.
![Upsert](https://user-images.githubusercontent.com/24603959/79110422-307ba080-7d9c-11ea-83e9-9c8e6a00d6be.JPG)
* We also need to rename the ```AddBillEntry``` from our Component class. As the duty of this method is creating/updating bill we can use ```SaveBillEntry```.

## Add validation

Still, now, we have no validation in our application. Anyone can insert data with no values. We need to expose a minimum validation to protect the system from invalid data. For now, we are going to use attribute-based validation.

* Install ```System.ComponentModel.Annotations``` nuget package in MonthlyBillScheduler.Domain project.
* Add ```Required``` attribute for Name property and ```Range``` attribute for Amount property. We need to specify the minimum and maximum range. In our case, 1 is the minimum value and there is no limit behind the maximum one.
![Validation 1](https://user-images.githubusercontent.com/24603959/79152748-8f6a0580-7dee-11ea-9591-a5cf635cc6c4.JPG)
* Add ```DataAnnotationsValidator``` inside EditForm of BillForm razor page. It indicates this form need to be validated.
* Also insert ```ValidationSummary``` inside EditForm. It gonna display the validation summary when you submit with invalid data.
* Add ```ValidationMessage``` component to display specific validation error for a single property. ValidationMessage having ```For``` attribute, we need to pass a func there and that need to specify our property.
![Validation 2](https://user-images.githubusercontent.com/24603959/79154414-4ebfbb80-7df1-11ea-994e-657f7b9b4fa0.JPG)

When you run the application, you may get some ugly formatted error message. We will fix it later.

## Add delete feature
We are done with create, read and update. Now only delation remaining for this basic implementation project.

* Add new method declaration in IBillService interface. As ```void Delete(int id);```.
* Implement Delete method in ```BillService``` class. We need to get the existing bill item followed by that bill id and remove that item from the list.
![Delete Method](https://user-images.githubusercontent.com/24603959/79237585-25a23800-7e90-11ea-9340-40fa620183df.JPG)

* Add ```DeleteBill``` method in BillsBase class and implement it to call Delete method of bill service.
* Add new action button inside the table of Bills razor component. This time we need to pass an argument in the delete method from onclick action. ```onclick``` event expect a func, so we need to pass a func like ```@onclick="(() => DeleteBill(bill.Id))"```.
![Delete Button](https://user-images.githubusercontent.com/24603959/79237572-233fde00-7e90-11ea-8dfb-3b79e6b0ac2b.JPG)

## Add toaster

Currently we are not giving any information to user about their action. We should give some interactive notification regarding their action. We are going to use [https://github.com/sotsera/sotsera.blazor.toaster](Sotsera.Blazor.Toaster) to display a simple toast message.

* Run ```Install-Package Sotsera.Blazor.Toaster -Version 3.0.0``` from your package manager console.
* To integrate this library in dependency injection, add the following line inside ```ConfigurationService``` method of your startup class. ```services.AddToaster()```
* Sotsera.Blazor.Toaster required some css files, add the following code line inside head tag of _Host.cshtml. ```<link href="_content/Sotsera.Blazor.Toaster/toastr.min.css" rel="stylesheet" />```.
* Add the following code block in ```App.razor``` file. This one helps you to render the toaster component.
```@using Sotsera.Blazor.Toaster
<ToastContainer />
```
* To fire toaster we need to inject the toaster first. Like others, we use ```[Inject]``` attribute of Blazor in our BillFormBase class.
* Fire the toaster using ```Toaster.Info("Bill added sucessfully");``` in our ```SaveBillEntry``` method of BillFormBase class.