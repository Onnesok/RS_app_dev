# Flutter Widgets Guide

Widgets are the building blocks of Flutter apps. Everything you see on screen is a widget.

## Widget Types

### Stateless Widgets
Don't change over time.

### Stateful Widgets
Can change over time.

## Basic Widgets

### Text Widget
```dart
Text('Hello, Flutter!')

Text(
  'Styled Text',
  style: TextStyle(
    fontSize: 24,
    fontWeight: FontWeight.bold,
    color: Colors.blue,
  ),
)
```

### Container Widget
```dart
Container(
  width: 200,
  height: 100,
  color: Colors.blue,
  child: Text('Hello'),
)

Container(
  padding: EdgeInsets.all(16),
  decoration: BoxDecoration(
    color: Colors.white,
    borderRadius: BorderRadius.circular(8),
    boxShadow: [
      BoxShadow(color: Colors.grey.withOpacity(0.3)),
    ],
  ),
  child: Text('Styled Container'),
)
```

### Image Widget
```dart
Image.network('https://example.com/image.jpg')
Image.asset('assets/images/logo.png')
```

## Layout Widgets

### Row Widget
```dart
Row(
  mainAxisAlignment: MainAxisAlignment.spaceBetween,
  children: [
    Text('Left'),
    Text('Center'),
    Text('Right'),
  ],
)

Row(
  children: [
    Expanded(flex: 2, child: Text('2/3 space')),
    Expanded(flex: 1, child: Text('1/3 space')),
  ],
)
```

### Column Widget
```dart
Column(
  mainAxisAlignment: MainAxisAlignment.center,
  children: [
    Text('Top'),
    Text('Middle'),
    Text('Bottom'),
  ],
)
```

### Stack Widget
```dart
Stack(
  children: [
    Container(width: 200, height: 200, color: Colors.blue),
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
```

## Input Widgets

### TextField Widget
```dart
TextField(
  decoration: InputDecoration(
    labelText: 'Enter your name',
    border: OutlineInputBorder(),
    prefixIcon: Icon(Icons.person),
  ),
  onChanged: (value) {
    print('Text changed: $value');
  },
)
```

### ElevatedButton Widget
```dart
ElevatedButton(
  onPressed: () {
    print('Button pressed!');
  },
  child: Text('Click Me'),
)

ElevatedButton(
  onPressed: () {},
  style: ElevatedButton.styleFrom(
    primary: Colors.blue,
    padding: EdgeInsets.symmetric(horizontal: 32, vertical: 16),
  ),
  child: Text('Styled Button'),
)
```

### Checkbox and Switch
```dart
bool isChecked = false;

Checkbox(
  value: isChecked,
  onChanged: (value) {
    setState(() {
      isChecked = value!;
    });
  },
)

Switch(
  value: isChecked,
  onChanged: (value) {
    setState(() {
      isChecked = value;
    });
  },
)
```

## List Widgets

### ListView Widget
```dart
ListView(
  children: [
    ListTile(title: Text('Item 1')),
    ListTile(title: Text('Item 2')),
    ListTile(title: Text('Item 3')),
  ],
)

ListView.builder(
  itemCount: 100,
  itemBuilder: (context, index) {
    return ListTile(
      title: Text('Item $index'),
      leading: Icon(Icons.list),
    );
  },
)
```

### GridView Widget
```dart
GridView.count(
  crossAxisCount: 2,
  children: [
    Container(color: Colors.red, child: Text('Item 1')),
    Container(color: Colors.blue, child: Text('Item 2')),
    Container(color: Colors.green, child: Text('Item 3')),
    Container(color: Colors.yellow, child: Text('Item 4')),
  ],
)
```

## Navigation Widgets

### AppBar Widget
```dart
AppBar(
  title: Text('My App'),
  backgroundColor: Colors.blue,
  actions: [
    IconButton(icon: Icon(Icons.search), onPressed: () {}),
    IconButton(icon: Icon(Icons.more_vert), onPressed: () {}),
  ],
)
```

### BottomNavigationBar Widget
```dart
int _selectedIndex = 0;

BottomNavigationBar(
  currentIndex: _selectedIndex,
  onTap: (index) {
    setState(() {
      _selectedIndex = index;
    });
  },
  items: [
    BottomNavigationBarItem(icon: Icon(Icons.home), label: 'Home'),
    BottomNavigationBarItem(icon: Icon(Icons.business), label: 'Business'),
    BottomNavigationBarItem(icon: Icon(Icons.school), label: 'School'),
  ],
)
```

## Custom Widgets

### Custom Stateless Widget
```dart
class CustomCard extends StatelessWidget {
  final String title;
  final String subtitle;
  final VoidCallback? onTap;

  const CustomCard({
    required this.title,
    required this.subtitle,
    this.onTap,
  });

  @override
  Widget build(BuildContext context) {
    return Card(
      child: ListTile(
        title: Text(title),
        subtitle: Text(subtitle),
        trailing: Icon(Icons.arrow_forward),
        onTap: onTap,
      ),
    );
  }
}
```

### Custom Stateful Widget
```dart
class CounterWidget extends StatefulWidget {
  final int initialValue;

  const CounterWidget({this.initialValue = 0});

  @override
  _CounterWidgetState createState() => _CounterWidgetState();
}

class _CounterWidgetState extends State<CounterWidget> {
  late int _count;

  @override
  void initState() {
    super.initState();
    _count = widget.initialValue;
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text('Count: $_count'),
        ElevatedButton(
          onPressed: () {
            setState(() {
              _count++;
            });
          },
          child: Text('Increment'),
        ),
      ],
    );
  }
}
```

## Common Patterns

### Loading State
```dart
Center(
  child: Column(
    mainAxisAlignment: MainAxisAlignment.center,
    children: [
      CircularProgressIndicator(),
      SizedBox(height: 16),
      Text('Loading...'),
    ],
  ),
)
```

### Error State
```dart
Center(
  child: Column(
    mainAxisAlignment: MainAxisAlignment.center,
    children: [
      Icon(Icons.error, size: 64, color: Colors.red),
      SizedBox(height: 16),
      Text('Error: $error'),
      ElevatedButton(
        onPressed: () {
          // Retry logic
        },
        child: Text('Retry'),
      ),
    ],
  ),
)
```

### Empty State
```dart
Center(
  child: Column(
    mainAxisAlignment: MainAxisAlignment.center,
    children: [
      Icon(Icons.inbox, size: 64, color: Colors.grey),
      SizedBox(height: 16),
      Text('No items found'),
      Text('Add some items to get started'),
    ],
  ),
)
```

## Best Practices

1. **Use const constructors** when possible
2. **Extract widgets** for reusability
3. **Use appropriate widgets** for the job
4. **Handle widget lifecycle** properly
5. **Follow material design** guidelines

## Summary

Key points about Flutter widgets:

1. Everything is a widget
2. Widgets are immutable
3. Use appropriate widgets for the job
4. Extract reusable widgets
5. Handle state properly
6. Follow material design
7. Optimize performance

Start with basic widgets and gradually explore more complex ones!
