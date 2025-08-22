# State Management in Flutter

State management is how you handle data that changes over time in your Flutter app. This guide covers different approaches to managing state.

## What is State?

State is any data that can change over time and affects the UI. Examples:
- User input in forms
- Data from APIs
- App settings
- Navigation state

## Types of State

### 1. Local State
State that belongs to a single widget.

### 2. Global State
State that needs to be shared across multiple widgets.

## Built-in State Management

### setState() - Local State
The simplest way to manage state within a single widget.

```dart
class CounterWidget extends StatefulWidget {
  @override
  _CounterWidgetState createState() => _CounterWidgetState();
}

class _CounterWidgetState extends State<CounterWidget> {
  int _count = 0;

  void _increment() {
    setState(() {
      _count++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text('Count: $_count'),
        ElevatedButton(
          onPressed: _increment,
          child: Text('Increment'),
        ),
      ],
    );
  }
}
```

### InheritedWidget - Simple Global State
For sharing data down the widget tree.

```dart
class AppState extends InheritedWidget {
  final int counter;
  final VoidCallback increment;

  const AppState({
    required this.counter,
    required this.increment,
    required Widget child,
  }) : super(child: child);

  static AppState of(BuildContext context) {
    return context.dependOnInheritedWidgetOfExactType<AppState>()!;
  }

  @override
  bool updateShouldNotify(AppState oldWidget) {
    return counter != oldWidget.counter;
  }
}

class MyApp extends StatefulWidget {
  @override
  _MyAppState createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  int _counter = 0;

  void _increment() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return AppState(
      counter: _counter,
      increment: _increment,
      child: MaterialApp(
        home: MyHomePage(),
      ),
    );
  }
}

class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final appState = AppState.of(context);
    
    return Scaffold(
      appBar: AppBar(title: Text('Counter App')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text('Count: ${appState.counter}'),
            ElevatedButton(
              onPressed: appState.increment,
              child: Text('Increment'),
            ),
          ],
        ),
      ),
    );
  }
}
```

## Popular State Management Solutions

### 1. Provider
A wrapper around InheritedWidget for easier state management.

#### Setup
```yaml
# pubspec.yaml
dependencies:
  provider: ^6.0.5
```

#### Basic Usage
```dart
import 'package:provider/provider.dart';

// Create a model class
class CounterModel extends ChangeNotifier {
  int _count = 0;
  
  int get count => _count;
  
  void increment() {
    _count++;
    notifyListeners();
  }
}

// Wrap your app with ChangeNotifierProvider
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return ChangeNotifierProvider(
      create: (context) => CounterModel(),
      child: MaterialApp(
        home: MyHomePage(),
      ),
    );
  }
}

// Use the state in widgets
class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Provider Example')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Consumer<CounterModel>(
              builder: (context, counter, child) {
                return Text('Count: ${counter.count}');
              },
            ),
            ElevatedButton(
              onPressed: () {
                context.read<CounterModel>().increment();
              },
              child: Text('Increment'),
            ),
          ],
        ),
      ),
    );
  }
}
```

#### Multiple Providers
```dart
MultiProvider(
  providers: [
    ChangeNotifierProvider(create: (_) => UserModel()),
    ChangeNotifierProvider(create: (_) => ThemeModel()),
    ChangeNotifierProvider(create: (_) => CartModel()),
  ],
  child: MyApp(),
)
```

### 2. Riverpod
A more modern alternative to Provider with better compile-time safety.

#### Setup
```yaml
# pubspec.yaml
dependencies:
  flutter_riverpod: ^2.4.9
```

#### Basic Usage
```dart
import 'package:flutter_riverpod/flutter_riverpod.dart';

// Create a provider
final counterProvider = StateNotifierProvider<CounterNotifier, int>((ref) {
  return CounterNotifier();
});

// Create a notifier
class CounterNotifier extends StateNotifier<int> {
  CounterNotifier() : super(0);

  void increment() {
    state++;
  }
}

// Wrap your app
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return ProviderScope(
      child: MaterialApp(
        home: MyHomePage(),
      ),
    );
  }
}

// Use in widgets
class MyHomePage extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final count = ref.watch(counterProvider);
    
    return Scaffold(
      appBar: AppBar(title: Text('Riverpod Example')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text('Count: $count'),
            ElevatedButton(
              onPressed: () {
                ref.read(counterProvider.notifier).increment();
              },
              child: Text('Increment'),
            ),
          ],
        ),
      ),
    );
  }
}
```

### 3. Bloc (Business Logic Component)
A pattern for managing state using events and states.

#### Setup
```yaml
# pubspec.yaml
dependencies:
  flutter_bloc: ^8.1.3
```

#### Basic Usage
```dart
import 'package:flutter_bloc/flutter_bloc.dart';

// Events
abstract class CounterEvent {}

class IncrementEvent extends CounterEvent {}
class DecrementEvent extends CounterEvent {}

// States
abstract class CounterState {
  final int count;
  CounterState(this.count);
}

class CounterInitial extends CounterState {
  CounterInitial() : super(0);
}

class CounterUpdated extends CounterState {
  CounterUpdated(int count) : super(count);
}

// Bloc
class CounterBloc extends Bloc<CounterEvent, CounterState> {
  CounterBloc() : super(CounterInitial()) {
    on<IncrementEvent>((event, emit) {
      emit(CounterUpdated(state.count + 1));
    });
    
    on<DecrementEvent>((event, emit) {
      emit(CounterUpdated(state.count - 1));
    });
  }
}

// Use in app
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return BlocProvider(
      create: (context) => CounterBloc(),
      child: MaterialApp(
        home: MyHomePage(),
      ),
    );
  }
}

// Use in widgets
class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Bloc Example')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            BlocBuilder<CounterBloc, CounterState>(
              builder: (context, state) {
                return Text('Count: ${state.count}');
              },
            ),
            ElevatedButton(
              onPressed: () {
                context.read<CounterBloc>().add(IncrementEvent());
              },
              child: Text('Increment'),
            ),
            ElevatedButton(
              onPressed: () {
                context.read<CounterBloc>().add(DecrementEvent());
              },
              child: Text('Decrement'),
            ),
          ],
        ),
      ),
    );
  }
}
```

### 4. GetX
A lightweight and powerful solution for state management.

#### Setup
```yaml
# pubspec.yaml
dependencies:
  get: ^4.6.6
```

#### Basic Usage
```dart
import 'package:get/get.dart';

// Create a controller
class CounterController extends GetxController {
  var count = 0.obs;

  void increment() {
    count++;
  }
}

// Use in app
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return GetMaterialApp(
      home: MyHomePage(),
    );
  }
}

// Use in widgets
class MyHomePage extends StatelessWidget {
  final CounterController controller = Get.put(CounterController());

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('GetX Example')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Obx(() => Text('Count: ${controller.count}')),
            ElevatedButton(
              onPressed: controller.increment,
              child: Text('Increment'),
            ),
          ],
        ),
      ),
    );
  }
}
```

## Choosing the Right Solution

### For Simple Apps
- **setState()**: Local state only
- **Provider**: Simple global state

### For Medium Apps
- **Provider**: Good balance of simplicity and power
- **Riverpod**: Better type safety and testing

### For Complex Apps
- **Bloc**: Clear separation of business logic
- **GetX**: All-in-one solution with navigation and dependency injection

## Best Practices

### 1. Keep State Minimal
```dart
// Good - Only store what changes
class UserModel extends ChangeNotifier {
  String _name = '';
  String _email = '';
  
  String get name => _name;
  String get email => _email;
}

// Avoid - Don't store computed values
class UserModel extends ChangeNotifier {
  String _firstName = '';
  String _lastName = '';
  String _fullName = ''; // Computed, don't store
}
```

### 2. Use Appropriate State Management
```dart
// Local state - use setState
class LocalWidget extends StatefulWidget {
  // ...
}

// Global state - use Provider/Riverpod/Bloc
class GlobalState extends ChangeNotifier {
  // ...
}
```

### 3. Separate Business Logic
```dart
// Good - Separate logic from UI
class UserService {
  Future<User> fetchUser(int id) async {
    // API call logic
  }
}

class UserModel extends ChangeNotifier {
  final UserService _service;
  
  UserModel(this._service);
  
  Future<void> loadUser(int id) async {
    final user = await _service.fetchUser(id);
    // Update state
  }
}
```

### 4. Handle Loading and Error States
```dart
enum LoadingState { initial, loading, loaded, error }

class DataModel extends ChangeNotifier {
  LoadingState _state = LoadingState.initial;
  String? _error;
  List<Data> _data = [];
  
  LoadingState get state => _state;
  String? get error => _error;
  List<Data> get data => _data;
  
  Future<void> loadData() async {
    _state = LoadingState.loading;
    notifyListeners();
    
    try {
      _data = await _service.fetchData();
      _state = LoadingState.loaded;
    } catch (e) {
      _error = e.toString();
      _state = LoadingState.error;
    }
    
    notifyListeners();
  }
}
```

## Testing State Management

### Testing Provider
```dart
testWidgets('should increment counter', (WidgetTester tester) async {
  await tester.pumpWidget(
    ChangeNotifierProvider(
      create: (_) => CounterModel(),
      child: MyHomePage(),
    ),
  );

  expect(find.text('Count: 0'), findsOneWidget);
  
  await tester.tap(find.text('Increment'));
  await tester.pump();
  
  expect(find.text('Count: 1'), findsOneWidget);
});
```

### Testing Bloc
```dart
blocTest<CounterBloc, CounterState>(
  'emits [1] when increment is added',
  build: () => CounterBloc(),
  act: (bloc) => bloc.add(IncrementEvent()),
  expect: () => [CounterUpdated(1)],
);
```

## Summary

Key points about state management:

1. **Choose the right tool** for your app's complexity
2. **Keep state minimal** and avoid storing computed values
3. **Separate business logic** from UI logic
4. **Handle loading and error states** properly
5. **Test your state management** thoroughly
6. **Use local state** for simple widget state
7. **Use global state** for data shared across widgets

Start with simple solutions like `setState()` and `Provider`, then move to more complex solutions as your app grows!

