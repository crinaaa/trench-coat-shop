# Trench Coat Shop

A C++ desktop application for managing and purchasing trench coats, built with **Qt 6**.

The application provides two modes: an administrator interface for managing the inventory and a customer interface for browsing coats and creating a shopping basket.

## Features

### Administrator mode
- View all available trench coats in a table.
- Add new coats with size, colour, photo link, price, and quantity.
- Update existing coat information.
- Remove coats from the inventory when their stock reaches zero.
- Sort the inventory by colour in ascending or descending order.
- Undo and redo add, remove, and update operations.
- Use `Ctrl + Z` and `Ctrl + Y` keyboard shortcuts for undo and redo.

### Customer mode
- Browse available trench coats.
- Filter coats by size.
- Navigate through the filtered coats.
- Open a coat's photo link in the default browser.
- Add coats to a shopping basket while automatically updating the available stock.
- View the basket and its total price.
- Export the basket as a CSV or HTML file and open it using the default application.

## Technologies

- **C++17**
- **Qt 6 Widgets** – graphical user interface
- **CMake** – build configuration
- **STL** – vectors, algorithms, and smart pointers
- **File handling** – persistent inventory storage using a text file

## Architecture

The application follows a layered architecture:

- **UI layer:** Qt widgets responsible for displaying information and handling user interactions.
- **Service layer:** Implements application logic, validation coordination, inventory operations, shopping basket management, sorting, and undo/redo.
- **Domain layer:** Contains the `TrenchCoat` entity and validation logic.
- **Repository layer:** Defines the repository interface and implements file-based inventory storage.

For more details, see [Architecture](docs/architecture.md).

## Project Structure

```text
trench-coat-shop/
├── AdminWidget.*
├── UserWidget.*
├── MainWindow.*
├── BasketDialog.*
├── BasketModel.*
├── TrenchCoat.*
├── IRepository.h
├── FileRepository.*
├── Service.*
├── CoatValidator.*
├── Exceptions.h
├── UndoRedoAction.*
├── FileBasket.h
├── CSV_Basket.*
├── HTML_Basket.*
├── coats.txt
├── CMakeLists.txt
└── docs/
    └── architecture.md
```

## Build and Run

### Prerequisites
- Windows
- Visual Studio with C++ desktop development tools
- CMake
- Qt 6 Widgets

### Steps
1. Clone the repository:
   ```bash
   git clone https://github.com/crinaaa/trench-coat-shop.git
   ```
2. Open the project folder in Visual Studio using **File → Open → Folder**.
3. Make sure your Qt 6 installation is configured for the CMake project.
4. Allow CMake to configure the project.
5. Build and run the `TrenchCoatShop` target.

The application uses `coats.txt` as its inventory data file. Run it with the working directory set to the folder containing this file.

## Data Persistence

The inventory is stored in `coats.txt`. Changes made through the administrator interface are saved to the file.

The shopping basket can be exported to `basket.csv` or `basket.html`.