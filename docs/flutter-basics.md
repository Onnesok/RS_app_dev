# Flutter Basics

Flutter is Google's UI toolkit for building beautiful, natively compiled applications for mobile, web, and desktop from a single codebase using the Dart programming language.

## What is Flutter?

Flutter is like a magic box that lets you write code once and run it on multiple platforms (iOS, Android, Web, Desktop). It's built by Google and uses a language called Dart.

## Key Concepts

### 1. Everything is a Widget

In Flutter, everything you see on screen is a **Widget**. Think of widgets as building blocks for your app.

```dart
// Simple text widget
Text('Hello, Flutter!')

// Button widget
ElevatedButton(
  onPressed: () {
    print('Button pressed!');
  },
  child: Text('Click Me'),
)
```

### 2. Stateless vs Stateful Widgets

**Stateless Widget**: Doesn't change over time (like a static image)
**Stateful Widget**: Can change over time (like a counter that increases)

```dart
// Stateless Widget Example
class MyText extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Text('This text never changes');
  }
}

// Stateful Widget Example
class Counter extends StatefulWidget {
  @override
  _CounterState createState() => _CounterState();
}

class _CounterState extends State<Counter> {
  int count = 0;

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text('Count: $count'),
        ElevatedButton(
          onPressed: () {
            setState(() {
              count++;
            });
          },
          child: Text('Increment'),
        ),
      ],
    );
  }
}
```

### 3. Basic Layout Widgets

#### Row and Column
```dart
// Row - arranges widgets horizontally
Row(
  children: [
    Text('Left'),
    Text('Center'),
    Text('Right'),
  ],
)

// Column - arranges widgets vertically
Column(
  children: [
    Text('Top'),
    Text('Middle'),
    Text('Bottom'),
  ],
)
```

#### Container
```dart
// Container - like a box that can hold other widgets
Container(
  width: 200,
  height: 100,
  color: Colors.blue,
  child: Text('Hello'),
)
```

### 4. Your First Flutter App

Here's a complete simple app:

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'My First App',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Hello Flutter'),
        ),
        body: Center(
          child: Text('Welcome to Flutter!'),
        ),
      ),
    );
  }
}
```

## Common Widgets You'll Use

### Text Widgets
```dart
Text('Simple text')
Text(
  'Styled text',
  style: TextStyle(
    fontSize: 20,
    fontWeight: FontWeight.bold,
    color: Colors.red,
  ),
)
```

### Button Widgets
```dart
// Basic button
ElevatedButton(
  onPressed: () {},
  child: Text('Click Me'),
)

// Text button
TextButton(
  onPressed: () {},
  child: Text('Text Button'),
)

// Icon button
IconButton(
  onPressed: () {},
  icon: Icon(Icons.favorite),
)
```

### Input Widgets
```dart
// Text field
TextField(
  decoration: InputDecoration(
    labelText: 'Enter your name',
    border: OutlineInputBorder(),
  ),
)

// Checkbox
Checkbox(
  value: isChecked,
  onChanged: (value) {
    setState(() {
      isChecked = value!;
    });
  },
)
```

## Hot Reload

One of Flutter's best features is **Hot Reload**. When you save your code, the app updates instantly without restarting. This makes development super fast!

## Next Steps

1. **Practice**: Try creating simple widgets
2. **Layout**: Learn more about Row, Column, and Container
3. **Navigation**: Learn how to move between screens
4. **State Management**: Understand how to manage app data

## Tips for Beginners

- Start with simple widgets
- Use Hot Reload frequently
- Don't worry about making mistakes
- Practice with small projects
- Read the Flutter documentation

## Common Mistakes to Avoid

1. **Forgetting setState()**: Always call setState() when changing state in StatefulWidget
2. **Not handling null**: Dart is null-safe, so handle null values properly
3. **Over-complicating**: Start simple and add complexity gradually
4. **Ignoring errors**: Read error messages carefully - they're helpful!

---

**Remember**: Flutter is all about widgets and composition. Start small, build up gradually, and don't be afraid to experiment!
