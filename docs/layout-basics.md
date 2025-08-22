# Flutter Layout Basics

Layout in Flutter is about arranging widgets on the screen. This guide covers the fundamental layout widgets and concepts.

## Layout Principles

### 1. Everything is a Widget
All layout elements are widgets, including containers, rows, and columns.

### 2. Widget Tree
Widgets are arranged in a tree structure, with parent widgets containing child widgets.

### 3. Constraints
Parent widgets provide constraints to their children (minimum/maximum size).

## Basic Layout Widgets

### Container Widget
A convenience widget that combines common painting, positioning, and sizing widgets.

```dart
Container(
  width: 200,
  height: 100,
  color: Colors.blue,
  child: Text('Hello'),
)

// Container with padding and margin
Container(
  padding: EdgeInsets.all(16),
  margin: EdgeInsets.all(8),
  decoration: BoxDecoration(
    color: Colors.white,
    borderRadius: BorderRadius.circular(8),
    border: Border.all(color: Colors.grey),
  ),
  child: Text('Styled Container'),
)
```

### Row Widget
Arranges children horizontally in a line.

```dart
Row(
  children: [
    Text('Left'),
    Text('Center'),
    Text('Right'),
  ],
)

// Row with alignment
Row(
  mainAxisAlignment: MainAxisAlignment.spaceBetween,
  crossAxisAlignment: CrossAxisAlignment.center,
  children: [
    Text('Left'),
    Text('Center'),
    Text('Right'),
  ],
)
```

### Column Widget
Arranges children vertically in a line.

```dart
Column(
  children: [
    Text('Top'),
    Text('Middle'),
    Text('Bottom'),
  ],
)

// Column with alignment
Column(
  mainAxisAlignment: MainAxisAlignment.center,
  crossAxisAlignment: CrossAxisAlignment.stretch,
  children: [
    Text('Top'),
    Text('Middle'),
    Text('Bottom'),
  ],
)
```

## Alignment Properties

### MainAxisAlignment
Controls how children are positioned along the main axis.

```dart
// Row examples
Row(
  mainAxisAlignment: MainAxisAlignment.start,    // Left
  children: [Text('A'), Text('B'), Text('C')],
)

Row(
  mainAxisAlignment: MainAxisAlignment.center,   // Center
  children: [Text('A'), Text('B'), Text('C')],
)

Row(
  mainAxisAlignment: MainAxisAlignment.end,      // Right
  children: [Text('A'), Text('B'), Text('C')],
)

Row(
  mainAxisAlignment: MainAxisAlignment.spaceBetween, // Space between
  children: [Text('A'), Text('B'), Text('C')],
)

Row(
  mainAxisAlignment: MainAxisAlignment.spaceAround,  // Space around
  children: [Text('A'), Text('B'), Text('C')],
)

Row(
  mainAxisAlignment: MainAxisAlignment.spaceEvenly,  // Equal space
  children: [Text('A'), Text('B'), Text('C')],
)
```

### CrossAxisAlignment
Controls how children are positioned along the cross axis.

```dart
// Row examples
Row(
  crossAxisAlignment: CrossAxisAlignment.start,   // Top
  children: [Text('A'), Text('B'), Text('C')],
)

Row(
  crossAxisAlignment: CrossAxisAlignment.center,  // Center
  children: [Text('A'), Text('B'), Text('C')],
)

Row(
  crossAxisAlignment: CrossAxisAlignment.end,     // Bottom
  children: [Text('A'), Text('B'), Text('C')],
)

Row(
  crossAxisAlignment: CrossAxisAlignment.stretch, // Stretch
  children: [Text('A'), Text('B'), Text('C')],
)
```

## Flexible and Expanded Widgets

### Expanded Widget
Makes a child of a Row, Column, or Flex expand to fill the available space.

```dart
Row(
  children: [
    Text('Fixed width'),
    Expanded(
      child: Text('This takes remaining space'),
    ),
    Text('Fixed width'),
  ],
)

// Multiple expanded widgets
Row(
  children: [
    Expanded(
      flex: 2,
      child: Container(color: Colors.red, child: Text('2/3 space')),
    ),
    Expanded(
      flex: 1,
      child: Container(color: Colors.blue, child: Text('1/3 space')),
    ),
  ],
)
```

### Flexible Widget
Similar to Expanded but allows the child to be smaller than the available space.

```dart
Row(
  children: [
    Text('Fixed'),
    Flexible(
      child: Text('This can be smaller if needed'),
    ),
    Text('Fixed'),
  ],
)
```

## Stack Widget
Positions children relative to the edges of its box.

```dart
Stack(
  children: [
    // Background
    Container(
      width: 200,
      height: 200,
      color: Colors.blue,
    ),
    // Overlay positioned at top-right
    Positioned(
      top: 10,
      right: 10,
      child: Container(
        padding: EdgeInsets.all(8),
        decoration: BoxDecoration(
          color: Colors.red,
          borderRadius: BorderRadius.circular(12),
        ),
        child: Text('New', style: TextStyle(color: Colors.white)),
      ),
    ),
  ],
)

// Stack with alignment
Stack(
  alignment: Alignment.center,
  children: [
    Container(width: 200, height: 200, color: Colors.blue),
    Text('Centered text'),
  ],
)
```

## Wrap Widget
Displays children in multiple horizontal or vertical runs.

```dart
Wrap(
  spacing: 8,        // Horizontal spacing
  runSpacing: 8,     // Vertical spacing
  children: [
    Chip(label: Text('Flutter')),
    Chip(label: Text('Dart')),
    Chip(label: Text('Mobile')),
    Chip(label: Text('Development')),
    Chip(label: Text('Cross-platform')),
  ],
)

// Wrap with alignment
Wrap(
  alignment: WrapAlignment.center,
  children: [
    Chip(label: Text('A')),
    Chip(label: Text('B')),
    Chip(label: Text('C')),
  ],
)
```

## SizedBox Widget
A box with a specified size.

```dart
SizedBox(
  width: 100,
  height: 50,
  child: Text('Fixed size'),
)

// SizedBox for spacing
Column(
  children: [
    Text('First item'),
    SizedBox(height: 16),  // 16px vertical space
    Text('Second item'),
  ],
)

Row(
  children: [
    Text('Left'),
    SizedBox(width: 20),   // 20px horizontal space
    Text('Right'),
  ],
)
```

## Center Widget
Centers its child within itself.

```dart
Center(
  child: Text('Centered text'),
)

// Center with constraints
Center(
  child: Container(
    width: 200,
    height: 100,
    color: Colors.blue,
    child: Text('Centered container'),
  ),
)
```

## Padding Widget
Adds padding around its child.

```dart
Padding(
  padding: EdgeInsets.all(16),
  child: Text('Text with padding'),
)

// Different padding for each side
Padding(
  padding: EdgeInsets.only(
    left: 16,
    top: 8,
    right: 16,
    bottom: 8,
  ),
  child: Text('Custom padding'),
)

// Symmetric padding
Padding(
  padding: EdgeInsets.symmetric(
    horizontal: 16,
    vertical: 8,
  ),
  child: Text('Symmetric padding'),
)
```

## Align Widget
Aligns its child within itself.

```dart
Align(
  alignment: Alignment.topLeft,
  child: Text('Top left'),
)

Align(
  alignment: Alignment.center,
  child: Text('Center'),
)

Align(
  alignment: Alignment.bottomRight,
  child: Text('Bottom right'),
)
```

## Common Layout Patterns

### Card Layout
```dart
Card(
  child: Padding(
    padding: EdgeInsets.all(16),
    child: Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Text(
          'Card Title',
          style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
        ),
        SizedBox(height: 8),
        Text('Card content goes here'),
      ],
    ),
  ),
)
```

### List Item Layout
```dart
ListTile(
  leading: CircleAvatar(child: Text('A')),
  title: Text('List Item Title'),
  subtitle: Text('List item subtitle'),
  trailing: Icon(Icons.arrow_forward),
  onTap: () {
    print('Item tapped');
  },
)
```

### Form Layout
```dart
Column(
  children: [
    TextField(
      decoration: InputDecoration(
        labelText: 'Name',
        border: OutlineInputBorder(),
      ),
    ),
    SizedBox(height: 16),
    TextField(
      decoration: InputDecoration(
        labelText: 'Email',
        border: OutlineInputBorder(),
      ),
    ),
    SizedBox(height: 16),
    ElevatedButton(
      onPressed: () {},
      child: Text('Submit'),
    ),
  ],
)
```

### Header Layout
```dart
Container(
  padding: EdgeInsets.all(16),
  color: Colors.blue,
  child: Row(
    children: [
      Icon(Icons.menu, color: Colors.white),
      SizedBox(width: 16),
      Text(
        'App Title',
        style: TextStyle(
          color: Colors.white,
          fontSize: 20,
          fontWeight: FontWeight.bold,
        ),
      ),
      Spacer(),
      Icon(Icons.search, color: Colors.white),
    ],
  ),
)
```

## Responsive Layout

### MediaQuery
Access screen size and other device information.

```dart
class ResponsiveWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final screenWidth = MediaQuery.of(context).size.width;
    final screenHeight = MediaQuery.of(context).size.height;
    
    if (screenWidth > 600) {
      // Tablet layout
      return Row(
        children: [
          Expanded(child: Text('Left panel')),
          Expanded(child: Text('Right panel')),
        ],
      );
    } else {
      // Phone layout
      return Column(
        children: [
          Text('Top panel'),
          Text('Bottom panel'),
        ],
      );
    }
  }
}
```

### LayoutBuilder
Build different layouts based on available space.

```dart
LayoutBuilder(
  builder: (context, constraints) {
    if (constraints.maxWidth > 600) {
      return Row(
        children: [
          Expanded(child: Text('Wide layout')),
          Expanded(child: Text('Wide layout')),
        ],
      );
    } else {
      return Column(
        children: [
          Text('Narrow layout'),
          Text('Narrow layout'),
        ],
      );
    }
  },
)
```

## Best Practices

### 1. Use Appropriate Widgets
```dart
// For simple spacing
SizedBox(height: 16)

// For complex spacing with decoration
Container(
  margin: EdgeInsets.all(8),
  padding: EdgeInsets.all(16),
  decoration: BoxDecoration(
    color: Colors.white,
    borderRadius: BorderRadius.circular(8),
  ),
)
```

### 2. Avoid Nested Containers
```dart
// Good
Container(
  padding: EdgeInsets.all(16),
  margin: EdgeInsets.all(8),
  child: Text('Hello'),
)

// Avoid
Container(
  padding: EdgeInsets.all(16),
  child: Container(
    margin: EdgeInsets.all(8),
    child: Text('Hello'),
  ),
)
```

### 3. Use Flexible Widgets for Dynamic Content
```dart
// Good for lists that might overflow
Wrap(
  children: [
    Chip(label: Text('Tag 1')),
    Chip(label: Text('Tag 2')),
    // ... more tags
  ],
)

// Instead of Row which might overflow
Row(
  children: [
    Chip(label: Text('Tag 1')),
    Chip(label: Text('Tag 2')),
    // This might cause overflow
  ],
)
```

### 4. Handle Overflow Gracefully
```dart
// Text overflow
Text(
  'This is a very long text that might overflow',
  overflow: TextOverflow.ellipsis,
  maxLines: 2,
)

// Row overflow
Row(
  children: [
    Flexible(
      child: Text('This text will wrap if needed'),
    ),
    Text('Fixed text'),
  ],
)
```

## Summary

Key layout concepts:

1. **Container** - Basic box with styling
2. **Row/Column** - Linear arrangements
3. **Stack** - Overlapping elements
4. **Expanded/Flexible** - Dynamic sizing
5. **Padding/SizedBox** - Spacing
6. **Center/Align** - Positioning
7. **MediaQuery** - Responsive design

Start with simple Row and Column layouts, then add more complex widgets as needed!

