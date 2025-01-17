## flutter_custom_dropdown_list

A customizable dropdown package for Flutter that allows developers to display a dropdown using bottom sheets or a full-screen modal. This package supports dynamic item rendering with optional itemBuilder, customizable search functionality, and three different bottom sheet display modes, making it easy to integrate into any Flutter project.


## Features
* Customizable Items: Use dynamic items of any type and display them using an optional itemBuilder.
* Multiple Bottom Sheet Modes: Supports modal, normal, and full-screen modes for the dropdown.
* Search Functionality: Easily enable or disable search in the dropdown to filter items.
* Generic Support: Allows usage of any class or data model as dropdown items.
* ToString Validation: Automatically checks if your custom class overrides the toString() method to ensure proper display.
* Optional ItemBuilder: Customize the way items are displayed using a builder function or default to a ListTile.
* Optional itemSearchCondition: Optional search functionality is an optional parameter that allows to define custom logic for searching through the dropdown items.
* Optional customizable color and theming for dropdown and search UI.


## Getting started
To use the package, add flutter_custom_dropdown_list to your pubspec.yaml:

```dart
dependencies:
 flutter_custom_dropdown_list: ^1.0.1

```

or 

```dart
dependencies:
  flutter_custom_dropdown_list:
    git:
      url:https://github.com/gmadhu27/flutter_custom_dropdown.git

```

Make sure to import the package in your Dart files:
```dart
import 'package:flutter_custom_dropdown_list/flutter_custom_dropdown.dart';
```

## Usage
Here’s a basic example of how to use the CustomDropdownHelper to show a dropdown with custom items and an optional search bar:

Simple Dropdown Example:

```dart
import 'package:flutter/material.dart';
import 'package:flutter_custom_dropdown_list/flutter_custom_dropdown.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Custom Dropdown Example',
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Custom Dropdown Example'),
        ),
        body: const Padding(
          padding: EdgeInsets.all(16.0),
          child: DropdownExample(),
        ),
      ),
    );
  }
}

class DropdownExample extends StatelessWidget {
  const DropdownExample({super.key});

  @override
  Widget build(BuildContext context) {
    final items = [
      DropdownItem(id: "1", name: 'Item 1'),
      DropdownItem(id: "2", name: 'Item 2'),
      DropdownItem(id: "3", name: 'Item 3'),
    ];

    return Center(
      child: ElevatedButton(
        onPressed: () {
          CustomDropdownHelper.showDropdown<DropdownItem>(
            context: context,
            items: items,
            title: "Select an Item",
            onItemSelected: (item) {
              if (item != null) {
                print("Selected: ${item.name}");
              }
            },
            itemBuilder: (item) {
              return ListTile(
                title: Text(item.name),
                subtitle: Text("ID: ${item.id}"),
              );
            },
          );
        },
        child: const Text("Show Dropdown"),
      ),
    );
  }
}
```
## DropdownItem Class Example
Here is a basic example of a DropdownItem class, which is used in the dropdown. This class implements the toString() method to ensure proper display of the item names.

```dart
class DropdownItem {
  final String id;
  final String name;

  DropdownItem({required this.id, required this.name});

  @override
  String toString() => name; // Override toString for display purposes
}
```
## Searchable Dropdown Example
```dart
CustomDropdownHelper.showDropdown<DropdownItem>(
  context: context,
  items: myItemsList, // List of DropdownItem or custom class
  title: "Select an Occupation",
  showSearch: true, // Enable search functionality
  onItemSelected: (selectedItem) {
    // Handle the selected item
    print('Selected: ${selectedItem.name}');
  },
);
```
## Optional itemSearchCondition
The itemSearchCondition is an optional parameter that allows you to define custom logic for searching through the dropdown items. Here’s an example of how to use the dropdown in your Flutter app, including the optional itemSearchCondition for custom search behavior:

```dart
CustomDropdownHelper.showDropdown<DropdownItem>(
  context: context,
  items: items,
  title: 'Select Item',
  onItemSelected: (selectedItem) {
  print('Selected: ${selectedItem?.name}');
   }, 
  showSearch: true,
  itemSearchCondition: (item, searchText) {
   // Custom search condition: match both id and name
   return item.name.toLowerCase().contains(searchText.toLowerCase()) ||
    item.id.contains(searchText);
 },
);
```
In the example above, the search checks if the name or id of the item contains the search text. You can customize this to fit your specific data structure and search needs.

If itemSearchCondition is not provided, the dropdown will use the default behavior of matching the toString() method for filtering items during search.

## Bottom Sheet Modes
You can display the dropdown using three different modes:

* Normal (default): Displays a normal bottom sheet that does not cover the screen fully.
* Modal: This displays the dropdown as a modal bottom sheet.
* Full-Screen: Displays the dropdown in full-screen mode.
  
```dart
CustomDropdownHelper.showDropdown<DropdownItem>(
  context: context,
  items: myItemsList,
  title: "Choose Item",
  bottomSheetMode: BottomSheetMode.full, // Full-screen mode
  onItemSelected: (selectedItem) {
    print('Selected: ${selectedItem.name}');
  },
);
```
## Custom Item Display
You can provide a custom item builder to display items in the dropdown. If not provided, the package defaults to a ListTile using the toString() method of the items.
```dart
CustomDropdownHelper.showDropdown<DropdownItem>(
  context: context,
  items: myItemsList,
  title: "Custom Item Display",
  itemBuilder: (item) {
    return Card(
      child: ListTile(
        title: Text(item.name),
        subtitle: Text("ID: ${item.id}"),
      ),
    );
  },
  onItemSelected: (selectedItem) {
    print('Selected: ${selectedItem.name}');
  },
);
```
## Custom Dropdown Theme
You can create customizable dropdowns with themes for background color, text style, icons, and more. Here’s an example of how to use it.

```dart
CustomDropdownHelper.showDropdown(
  context: context,
  items: items,
  title: "Select an Item",
  // Bottom sheet mode (optional, default is normal mode)
  bottomSheetMode: BottomSheetMode.full,
  // Show search bar (optional, default is true)
  showSearch: true,
  onItemSelected: (DropdownItem? selectedItem) {
    // Handle the selected item
    setState(() {
      _selectedItemName = selectedItem?.name;
    });
  },
  // Custom item builder (optional)
  itemBuilder: (item) {
    return ListTile(
      title: Text(item.name),
      subtitle: Text(item.id),
    );
  },
  // Custom search logic (optional)
  itemSearchCondition: (item, searchText) {
    return item.id.toLowerCase().contains(searchText) ||
        item.name.toLowerCase().contains(searchText);
  },
  // Apply CustomDropdownTheme (optional)
  theme: CustomDropdownTheme(
    // Background color for the dropdown (optional)
    backgroundColor: Colors.deepOrange,
    // Back icon color (optional)
    backIconColor: Colors.white,
    // Title text style (optional)
    titleTextStyle: const TextStyle(color: Colors.white, fontSize: 22),
    // Search box decoration (optional)
    searchBoxDecoration: InputDecoration(
      hintText: 'Search here',
      hintStyle: const TextStyle(color: Colors.white, fontSize: 18),
      filled: true,
      fillColor: Colors.orange.shade100,
      border: OutlineInputBorder(
        borderRadius: BorderRadius.circular(10),
        borderSide: const BorderSide(color: Colors.orange, width: 2),
      ),
      prefixIcon: const Icon(Icons.search, color: Colors.white),
    ),
    // Bottom sheet box decoration (optional)
    bottomSheetBoxDecoration: const BoxDecoration(
      color: Colors.deepOrange,
      borderRadius: BorderRadius.vertical(top: Radius.circular(30.0)),
    ),
  ),
);
```
## CustomDropdownTheme Properties
| Property | Type | Description |
| -------- | ---- | :------: |
| backgroundColor | Color? | The background color of the dropdown and the bottom sheet. Default is Colors.grey.withOpacity(0.6). |
| backIconColor | Color? | The color of the back icon or close button. Default is Colors.black. |
| titleTextStyle | TextStyle? | The text style for the title of the dropdown or bottom sheet. |
| searchBoxDecoration | InputDecoration? | Custom decoration for the search box input field, including hint text, border, and prefix icon. |
| bottomSheetBoxDecoration | BoxDecoration? | Custom decoration for the bottom sheet, allowing you to control background color, shape, etc. |

1. Background Color: You can change the background color of the dropdown by passing a custom color to the backgroundColor property. This affects the entire bottom sheet background.

```dart
backgroundColor: Colors.deepOrange
```
2. Back Icon Color: You can change the backicon color of the dropdown by passing a custom color to the backIconColor property. This affects the entire bottom sheet close icon.

```dart
backIconColor: Colors.deepOrange
```
3. Title Text Style: Modify the appearance of the title text inside the dropdown by using the titleTextStyle property. This allows for font size, color, and weight adjustments.

```dart
titleTextStyle: TextStyle(color: Colors.white, fontSize: 22)
```
4. Search Box Decoration: Customize the look of the search input field by passing a custom InputDecoration to searchBoxDecoration. You can change the hint text, input border, and the prefix icon.

```dart
searchBoxDecoration: InputDecoration(
  hintText: 'Search here',
  hintStyle: TextStyle(color: Colors.white, fontSize: 18),
  filled: true,
  fillColor: Colors.orange.shade100,
  border: OutlineInputBorder(
    borderRadius: BorderRadius.circular(10),
    borderSide: BorderSide(color: Colors.orange, width: 2),
  ),
  prefixIcon: Icon(Icons.search, color: Colors.white),
)
```
5. Bottom Sheet Box Decoration: Customize the shape and color of the bottom sheet using the bottomSheetBoxDecoration. You can modify the shape by adding a border radius.

```dart
bottomSheetBoxDecoration: BoxDecoration(
  color: Colors.deepOrange,
  borderRadius: BorderRadius.vertical(top: Radius.circular(30.0)),
)
```

## Exception Handling
* Empty List: If the items list is empty, an exception will be thrown to notify the developer.
* ToString Check: The package checks if the class used for the items overrides the toString() method. If not, an exception will be thrown, ensuring that the dropdown can display items correctly.


## Additional information
Feel free to contribute to this package, raise issues, or suggest new features by creating an issue in this GitHub repository. This package is open-source, and contributions are always welcome.

For more details and examples, visit the /example folder.

This README provides a concise overview of how to use the package, examples, and additional information for users. You can modify the details based on your specific project requirements, including the repository link or contributing guidelines.
