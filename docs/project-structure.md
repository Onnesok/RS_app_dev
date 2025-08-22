# Flutter Project Structure

A well-organized project structure makes your Flutter app easier to maintain, test, and scale. This guide covers best practices for organizing your Flutter project.

## Basic Flutter Project Structure

When you create a new Flutter project, you get this basic structure:

```
my_flutter_app/
├── android/                 # Android-specific files
├── ios/                    # iOS-specific files
├── lib/                    # Main Dart code
│   ├── main.dart          # App entry point
│   └── ...
├── test/                   # Test files
├── pubspec.yaml           # Dependencies and assets
├── pubspec.lock           # Locked dependency versions
└── README.md              # Project documentation
```

## Recommended Project Structure

Here's a recommended structure for a well-organized Flutter app:

```
lib/
├── main.dart                    # App entry point
├── app/                        # App-level configuration
│   ├── app.dart               # Main app widget
│   ├── routes.dart            # Route definitions
│   └── theme.dart             # App theme configuration
├── core/                       # Core functionality
│   ├── constants/             # App constants
│   ├── utils/                 # Utility functions
│   ├── services/              # Core services
│   └── errors/                # Error handling
├── data/                       # Data layer
│   ├── models/                # Data models
│   ├── repositories/          # Repository implementations
│   ├── datasources/           # Data sources (API, local)
│   └── dto/                   # Data transfer objects
├── domain/                     # Business logic layer
│   ├── entities/              # Business entities
│   ├── repositories/          # Repository interfaces
│   └── usecases/              # Business use cases
├── presentation/               # UI layer
│   ├── pages/                 # Screen widgets
│   ├── widgets/               # Reusable widgets
│   ├── providers/             # State management
│   └── controllers/           # UI controllers
└── shared/                     # Shared components
    ├── widgets/               # Shared widgets
    ├── services/              # Shared services
    └── helpers/               # Shared helpers
```

## Detailed Structure Explanation

### 1. main.dart
The entry point of your app.

```dart
import 'package:flutter/material.dart';
import 'app/app.dart';

void main() {
  runApp(MyApp());
}
```

### 2. app/ Directory
Contains app-level configuration.

#### app.dart
```dart
import 'package:flutter/material.dart';
import 'routes.dart';
import 'theme.dart';

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'My Flutter App',
      theme: AppTheme.lightTheme,
      routes: AppRoutes.routes,
      home: HomePage(),
    );
  }
}
```

#### routes.dart
```dart
import 'package:flutter/material.dart';
import '../presentation/pages/home_page.dart';
import '../presentation/pages/profile_page.dart';

class AppRoutes {
  static const String home = '/';
  static const String profile = '/profile';

  static Map<String, WidgetBuilder> get routes => {
    home: (context) => HomePage(),
    profile: (context) => ProfilePage(),
  };
}
```

#### theme.dart
```dart
import 'package:flutter/material.dart';

class AppTheme {
  static ThemeData get lightTheme {
    return ThemeData(
      primarySwatch: Colors.blue,
      brightness: Brightness.light,
      appBarTheme: AppBarTheme(
        elevation: 0,
        backgroundColor: Colors.blue,
        foregroundColor: Colors.white,
      ),
    );
  }

  static ThemeData get darkTheme {
    return ThemeData(
      primarySwatch: Colors.blue,
      brightness: Brightness.dark,
    );
  }
}
```

### 3. core/ Directory
Core functionality used throughout the app.

#### constants/
```dart
// app_constants.dart
class AppConstants {
  static const String appName = 'My Flutter App';
  static const String apiBaseUrl = 'https://api.example.com';
  static const int timeoutDuration = 30;
}

// api_endpoints.dart
class ApiEndpoints {
  static const String users = '/users';
  static const String posts = '/posts';
  static const String comments = '/comments';
}
```

#### utils/
```dart
// date_utils.dart
class DateUtils {
  static String formatDate(DateTime date) {
    return '${date.day}/${date.month}/${date.year}';
  }
}

// validation_utils.dart
class ValidationUtils {
  static bool isValidEmail(String email) {
    return RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$').hasMatch(email);
  }
}
```

#### services/
```dart
// api_service.dart
class ApiService {
  static const String baseUrl = AppConstants.apiBaseUrl;
  
  static Future<Map<String, dynamic>> get(String endpoint) async {
    // API implementation
  }
  
  static Future<Map<String, dynamic>> post(String endpoint, Map<String, dynamic> data) async {
    // API implementation
  }
}
```

### 4. data/ Directory
Data layer for handling data operations.

#### models/
```dart
// user_model.dart
class UserModel {
  final int id;
  final String name;
  final String email;

  UserModel({
    required this.id,
    required this.name,
    required this.email,
  });

  factory UserModel.fromJson(Map<String, dynamic> json) {
    return UserModel(
      id: json['id'],
      name: json['name'],
      email: json['email'],
    );
  }

  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'name': name,
      'email': email,
    };
  }
}
```

#### repositories/
```dart
// user_repository_impl.dart
import '../models/user_model.dart';
import '../../core/services/api_service.dart';

class UserRepositoryImpl {
  Future<List<UserModel>> getUsers() async {
    final response = await ApiService.get('/users');
    final List<dynamic> usersJson = response['data'];
    return usersJson.map((json) => UserModel.fromJson(json)).toList();
  }

  Future<UserModel> getUser(int id) async {
    final response = await ApiService.get('/users/$id');
    return UserModel.fromJson(response);
  }
}
```

#### datasources/
```dart
// local_storage.dart
import 'package:shared_preferences/shared_preferences.dart';

class LocalStorage {
  static Future<void> saveString(String key, String value) async {
    final prefs = await SharedPreferences.getInstance();
    await prefs.setString(key, value);
  }

  static Future<String?> getString(String key) async {
    final prefs = await SharedPreferences.getInstance();
    return prefs.getString(key);
  }
}
```

### 5. domain/ Directory
Business logic layer.

#### entities/
```dart
// user_entity.dart
class UserEntity {
  final int id;
  final String name;
  final String email;

  UserEntity({
    required this.id,
    required this.name,
    required this.email,
  });
}
```

#### repositories/
```dart
// user_repository.dart
import '../entities/user_entity.dart';

abstract class UserRepository {
  Future<List<UserEntity>> getUsers();
  Future<UserEntity> getUser(int id);
  Future<void> createUser(UserEntity user);
}
```

#### usecases/
```dart
// get_users_usecase.dart
import '../entities/user_entity.dart';
import '../repositories/user_repository.dart';

class GetUsersUseCase {
  final UserRepository repository;

  GetUsersUseCase(this.repository);

  Future<List<UserEntity>> execute() async {
    return await repository.getUsers();
  }
}
```

### 6. presentation/ Directory
UI layer with pages, widgets, and state management.

#### pages/
```dart
// home_page.dart
import 'package:flutter/material.dart';
import '../widgets/user_list_widget.dart';
import '../providers/user_provider.dart';

class HomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Home')),
      body: UserListWidget(),
    );
  }
}
```

#### widgets/
```dart
// user_list_widget.dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import '../providers/user_provider.dart';
import 'user_card_widget.dart';

class UserListWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Consumer<UserProvider>(
      builder: (context, userProvider, child) {
        if (userProvider.isLoading) {
          return Center(child: CircularProgressIndicator());
        }
        
        return ListView.builder(
          itemCount: userProvider.users.length,
          itemBuilder: (context, index) {
            return UserCardWidget(user: userProvider.users[index]);
          },
        );
      },
    );
  }
}
```

#### providers/
```dart
// user_provider.dart
import 'package:flutter/foundation.dart';
import '../../domain/entities/user_entity.dart';
import '../../domain/usecases/get_users_usecase.dart';

class UserProvider extends ChangeNotifier {
  final GetUsersUseCase _getUsersUseCase;
  
  UserProvider(this._getUsersUseCase);

  List<UserEntity> _users = [];
  bool _isLoading = false;
  String? _error;

  List<UserEntity> get users => _users;
  bool get isLoading => _isLoading;
  String? get error => _error;

  Future<void> loadUsers() async {
    _isLoading = true;
    _error = null;
    notifyListeners();

    try {
      _users = await _getUsersUseCase.execute();
    } catch (e) {
      _error = e.toString();
    } finally {
      _isLoading = false;
      notifyListeners();
    }
  }
}
```

### 7. shared/ Directory
Shared components used across the app.

#### widgets/
```dart
// custom_button.dart
import 'package:flutter/material.dart';

class CustomButton extends StatelessWidget {
  final String text;
  final VoidCallback onPressed;
  final bool isLoading;

  const CustomButton({
    required this.text,
    required this.onPressed,
    this.isLoading = false,
  });

  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: isLoading ? null : onPressed,
      child: isLoading 
        ? CircularProgressIndicator(color: Colors.white)
        : Text(text),
    );
  }
}
```

## File Naming Conventions

### 1. Use snake_case for files
```
user_profile_page.dart
api_service.dart
validation_utils.dart
```

### 2. Use PascalCase for classes
```dart
class UserProfilePage extends StatelessWidget {}
class ApiService {}
class ValidationUtils {}
```

### 3. Use descriptive names
```
// Good
user_authentication_service.dart
product_list_widget.dart
date_formatter_utils.dart

// Avoid
service.dart
widget.dart
utils.dart
```

## Dependency Injection Setup

### Using GetIt for DI
```dart
// di/injection.dart
import 'package:get_it/get_it.dart';
import '../data/repositories/user_repository_impl.dart';
import '../domain/repositories/user_repository.dart';
import '../domain/usecases/get_users_usecase.dart';
import '../presentation/providers/user_provider.dart';

final GetIt getIt = GetIt.instance;

void setupInjection() {
  // Repositories
  getIt.registerLazySingleton<UserRepository>(
    () => UserRepositoryImpl(),
  );

  // Use cases
  getIt.registerLazySingleton(
    () => GetUsersUseCase(getIt<UserRepository>()),
  );

  // Providers
  getIt.registerFactory(
    () => UserProvider(getIt<GetUsersUseCase>()),
  );
}
```

## Testing Structure

```
test/
├── unit/                      # Unit tests
│   ├── domain/
│   │   └── usecases/
│   └── data/
│       └── repositories/
├── widget/                    # Widget tests
│   └── presentation/
│       └── pages/
└── integration/               # Integration tests
```

### Example Test
```dart
// test/unit/domain/usecases/get_users_usecase_test.dart
import 'package:flutter_test/flutter_test.dart';
import 'package:mockito/mockito.dart';
import 'package:mockito/annotations.dart';
import 'package:my_app/domain/usecases/get_users_usecase.dart';
import 'package:my_app/domain/repositories/user_repository.dart';

@GenerateMocks([UserRepository])
void main() {
  late GetUsersUseCase useCase;
  late MockUserRepository mockRepository;

  setUp(() {
    mockRepository = MockUserRepository();
    useCase = GetUsersUseCase(mockRepository);
  });

  test('should get users from repository', () async {
    // Arrange
    final users = [UserEntity(id: 1, name: 'John', email: 'john@example.com')];
    when(mockRepository.getUsers()).thenAnswer((_) async => users);

    // Act
    final result = await useCase.execute();

    // Assert
    expect(result, users);
    verify(mockRepository.getUsers()).called(1);
  });
}
```

## Assets Organization

```
assets/
├── images/                    # Image files
│   ├── icons/
│   └── backgrounds/
├── fonts/                     # Custom fonts
└── data/                      # JSON files, etc.
```

### pubspec.yaml Configuration
```yaml
flutter:
  assets:
    - assets/images/
    - assets/data/
  fonts:
    - family: CustomFont
      fonts:
        - asset: assets/fonts/CustomFont-Regular.ttf
        - asset: assets/fonts/CustomFont-Bold.ttf
          weight: 700
```

## Best Practices

### 1. Keep Files Small
- Aim for files under 300 lines
- Split large files into smaller, focused files

### 2. Use Consistent Imports
```dart
// Standard library imports
import 'dart:async';
import 'dart:convert';

// Third-party imports
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';

// Local imports
import '../widgets/custom_widget.dart';
import '../utils/helpers.dart';
```

### 3. Follow Single Responsibility Principle
- Each file should have one clear purpose
- Each class should have one responsibility

### 4. Use Meaningful Names
- Choose descriptive names for files, classes, and functions
- Avoid abbreviations unless they're widely understood

### 5. Organize by Feature (Alternative)
For larger apps, consider organizing by feature:

```
lib/
├── features/
│   ├── auth/
│   │   ├── data/
│   │   ├── domain/
│   │   └── presentation/
│   ├── home/
│   │   ├── data/
│   │   ├── domain/
│   │   └── presentation/
│   └── profile/
│       ├── data/
│       ├── domain/
│       └── presentation/
└── shared/
    ├── widgets/
    ├── services/
    └── utils/
```

## Summary

A well-organized project structure:

1. **Improves maintainability** - Easy to find and modify code
2. **Enhances scalability** - Easy to add new features
3. **Facilitates testing** - Clear separation of concerns
4. **Promotes collaboration** - Team members understand the structure
5. **Reduces complexity** - Clear boundaries between layers

Start with a simple structure and evolve it as your app grows!

