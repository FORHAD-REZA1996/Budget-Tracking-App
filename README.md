# Budget Tracking App
Design:	

UML Class Diagram:
During the design process, class and object diagrams were created to visually represent the structure and relationships of important components in the application. The classes included in the system are User, Transaction, Budget, Category, Inherited User Category, and Inherited Preset Category. Each class has its own set of properties and methods. Inheritance was employed to establish linkages between the category and the Inherited User Category and Inherited Preset Category.

![image](https://github.com/FORHAD-REZA1996/Budget-Tracking-App/assets/54482125/50f91cde-6712-4e87-873b-b6a578910e3c)


## Implementation

In implementation, the following points were taken into account while paying attention to maintainability and extensibility

### Design Patterns

#### Singleton Pattern

The Singleton pattern ensures that a class has only one instance and provides a global point of access to it. In this application, the `Logger` and `BudgetApp` classes utilize the Singleton pattern. These classes hold their instances statically and offer access to the unique instance through a `GetInstance()` method, preventing direct instantiation from outside. This pattern is beneficial for maintaining a consistent logging mechanism and managing core functionalities of the application across its entirety.

![image](https://github.com/FORHAD-REZA1996/Budget-Tracking-App/assets/54482125/4d5b692c-3b09-4b33-bb2a-de100e806485)


#### Factory Pattern

The Factory Pattern separates the instantiation logic from the client and allows subclasses to decide what to instantiate. The CategoryFactory class in this case is responsible for generating different types of category objects (PresetCategory and UserCategory). Each method creates a new category object of the requested type, assigns it a unique ID (CategoryId), sets the required information (name and user) and then returns the object. This allows categories to be uniquely identified throughout the application.

![image](https://github.com/FORHAD-REZA1996/Budget-Tracking-App/assets/54482125/399e1b89-be29-417a-9443-c61c4f24f359)


#### Repository Pattern (Bridge Pattern)

The Repository pattern introduces an abstraction layer between the data source (e.g., a database) and the business logic layer. It separates data access logic from business logic, allowing for changes in data sources without impacting the business logic. The `Logger` class adopts this pattern, interfacing with log storage through the `ILogRepository` interface, enabling flexible changes in log storage destinations. This facilitates easy switching between different logging methods for development and production environments.

![image](https://github.com/FORHAD-REZA1996/Budget-Tracking-App/assets/54482125/f7a9d8d6-efc6-4e5a-a0fd-f849954d2764)


### Inheritance

In managing categories, the application distinguishes between two types: categories created by the user (User Categories) and categories pre-defined by the application (Preset Categories). To efficiently manage these categories, an abstract class named `Category` consolidates common attributes. This class is then inherited by `PresetCategory` and `UserCategory` to cater to their specific needs.

- **UserCategory** needs to be linked with a `User`, hence an additional `User` attribute is incorporated to establish a clear relationship between the user and their categories.
- **PresetCategory** requires the instances created to be shared across all users of the application. To fulfill this requirement, static attributes within the PresetCategory class manage the instances, ensuring application-wide accessibility.

### Delegation

The `CalcSpent` method in the `User` class calculates spending for a user in a specific category, based on the category and transaction list. Here, a `User` object calculates expenditure through its transaction list for a given category, bearing the responsibility of deriving expenditures by category. This can be considered delegation in terms of distributing responsibilities among objects.

### Special Notes

- **Static Instance Management in PresetCategory**: The `PresetCategory` class manages all instances using a static list to track created instances. This facilitates easy access to preset category instances across the application while tracking each new instance as it's created.
- **Use Cases as Service Layer**: As the program expanded and the `Program.cs` file became lengthy and cluttered, files were segmented according to domain areas and stored within the `Services` directory. This reorganization improved code structure and manageability.
- **Defining Methods within Models**: Operations tied to a specific model and called multiple times within the service layer are defined as methods within the model itself. For example, the operation to retrieve all categories of a user is defined as the `GetCategories` method within the `User` model. This approach enhances code reusability and organization.
- **Transaction Management**: Transactions are categorized into two types: Expenses and Income. These categories are managed through an Enum named `TransactionType`, with attributes defined as `Type` within transactions. This distinction clarifies transaction types, simplifying their management.
