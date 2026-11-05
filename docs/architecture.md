# Architecture

The Trench Coat Shop application is organized into layers that separate the graphical interface, application logic, domain entities, and data persistence.

## 1. Overview

The main components are:

- **UI layer:** Handles user interaction and presentation using Qt Widgets.
- **Service layer:** Coordinates application operations and enforces business rules.
- **Domain layer:** Defines the main entity and input validation.
- **Repository layer:** Provides an abstraction for inventory storage and its file-based implementation.

The main dependency flow is:

```text
        UI Layer
            |
            v
       Service Layer
            |
            v
       IRepository
            |
            v
     FileRepository
            |
            v
        coats.txt
```

## 2. Domain Layer

### TrenchCoat

`TrenchCoat` represents a coat in the inventory.

Each coat stores:
- Size
- Colour
- Photo link
- Price
- Quantity

The class provides getters and setters, equality comparison, and stream operators for reading from and writing to the inventory file.

Two coats are considered equal when they have the same size and colour.

### CoatValidator

`CoatValidator` validates coat information before it is added or updated.

It checks that:
- The size is one of `XS`, `S`, `M`, `L`, `XL`, or `XXL`.
- Colour and photo link are not empty.
- Price and quantity are non-negative.

Invalid data results in a `ValidationException`.

## 3. Repository Layer

### IRepository

`IRepository` defines the operations required for inventory management:
- Add a coat.
- Remove a coat.
- Update a coat.
- Retrieve all coats.
- Find a coat by size and colour.

This abstraction separates the service from the concrete storage implementation.

### FileRepository

`FileRepository` implements `IRepository` using a text file.

It:
- Loads inventory data when the repository is constructed.
- Stores coats in a vector.
- Saves changes to the file after add, remove, and update operations.
- Searches for coats using their size and colour.

Each line in `coats.txt` follows this format:

```text
size|colour|photoLink|price|quantity
```

## 4. Service Layer

`Service` coordinates the application's operations and communicates with the repository.

Its responsibilities include:

- Managing inventory operations.
- Validating information before adding or updating coats.
- Retrieving all coats or filtering them by size.
- Managing the shopping basket and calculating its total price.
- Decreasing stock when a coat is added to the basket.
- Sorting the inventory by colour.
- Coordinating undo and redo operations.

### Undo and Redo

Undo and redo are implemented using two stacks of actions:

- `undoStack` stores operations that can be reversed.
- `redoStack` stores operations that can be reapplied.

`UndoRedoAction` is an abstract base class. Its concrete implementations are:

- `ActionAdd`
- `ActionRemove`
- `ActionUpdate`

Each action stores the information necessary to reverse or reapply its corresponding operation.

A new administrator operation clears the redo history.

## 5. User Interface Layer

The graphical interface is implemented using Qt 6 Widgets.

### MainWindow

`MainWindow` provides the initial mode-selection screen and manages navigation between the administrator and customer interfaces using a `QStackedWidget`.

### AdminWidget

`AdminWidget` provides the administrator interface.

It displays the inventory in a table and allows the administrator to add, remove, and update coats. It also provides sorting controls and undo/redo buttons, including keyboard shortcuts.

### UserWidget

`UserWidget` provides the customer interface.

It allows customers to filter coats by size, browse the results, navigate between coats, and add available items to their basket.

The photo link of the currently displayed coat is opened using the system's default browser.

### BasketDialog and BasketModel

`BasketDialog` displays the shopping basket, its total price, and export options.

`BasketModel` inherits from `QAbstractTableModel` and provides the basket data to a `QTableView`.

## 6. Basket Export

Basket export is implemented using a small class hierarchy.

`FileBasket` is an abstract base class that defines the operations for writing and displaying a basket.

Two implementations are provided:

- `CSV_Basket` writes the basket to a CSV file.
- `HTML_Basket` generates an HTML table containing the basket items.

Both implementations open the generated file using the system's default application.