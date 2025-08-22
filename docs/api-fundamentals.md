# API Fundamentals

An API (Application Programming Interface) is like a waiter in a restaurant - it takes your request, communicates with the kitchen (server), and brings back your food (data). In the digital world, APIs allow different applications to talk to each other.

## What is an API?

An API is a set of rules that allows one software application to interact with another. Think of it as a contract that defines how applications can request and exchange data.

![api](https://voyager.postman.com/illustration/diagram-what-is-an-api-postman-illustration.svg)

### Real-World Analogy

Imagine you're ordering food:
1. **You** (Client) → **Waiter** (API) → **Kitchen** (Server)
2. **Kitchen** (Server) → **Waiter** (API) → **You** (Client)

In web development:
1. **Your App** (Client) → **API** → **Server**
2. **Server** → **API** → **Your App** (Client)

## How APIs Work

### 1. Request-Response Cycle

```dart
// Your Flutter app makes a request
final response = await http.get(Uri.parse('https://api.example.com/users'));

// Server processes the request and sends back data
// Your app receives the response
```

### 2. HTTP Methods

APIs use different HTTP methods for different operations:

#### GET - Retrieve Data
```dart
// Get user information
final response = await http.get(
  Uri.parse('https://api.example.com/users/123'),
);
```

#### POST - Create Data
```dart
// Create a new user
final response = await http.post(
  Uri.parse('https://api.example.com/users'),
  headers: {'Content-Type': 'application/json'},
  body: jsonEncode({
    'name': 'John Doe',
    'email': 'john@example.com',
  }),
);
```

#### PUT - Update Data
```dart
// Update user information
final response = await http.put(
  Uri.parse('https://api.example.com/users/123'),
  headers: {'Content-Type': 'application/json'},
  body: jsonEncode({
    'name': 'John Smith',
    'email': 'johnsmith@example.com',
  }),
);
```

#### DELETE - Remove Data
```dart
// Delete a user
final response = await http.delete(
  Uri.parse('https://api.example.com/users/123'),
);
```

## API Endpoints

An endpoint is a specific URL that your app can communicate with.

### Example API Structure
```
Base URL: https://api.example.com
├── /users          (GET: list users, POST: create user)
├── /users/123      (GET: get user, PUT: update user, DELETE: delete user)
├── /posts          (GET: list posts, POST: create post)
└── /posts/456      (GET: get post, PUT: update post, DELETE: delete post)
```

## Making API Calls in Flutter
![api call](https://project-static-assets.s3.amazonaws.com/APISpreadsheets/APIMemes/StanleyScreaming.jpeg)

### 1. Add HTTP Package
```yaml
# pubspec.yaml
dependencies:
  http: ^1.1.0
```

### 2. Basic API Call
```dart
import 'package:http/http.dart' as http;
import 'dart:convert';

Future<void> fetchUserData() async {
  try {
    final response = await http.get(
      Uri.parse('https://jsonplaceholder.typicode.com/users/1'),
    );

    if (response.statusCode == 200) {
      // Success - parse the data
      Map<String, dynamic> userData = jsonDecode(response.body);
      print('User name: ${userData['name']}');
    } else {
      // Error
      print('Failed to load user data');
    }
  } catch (e) {
    print('Error: $e');
  }
}
```

### 3. POST Request with Data
```dart
Future<void> createUser(String name, String email) async {
  try {
    final response = await http.post(
      Uri.parse('https://jsonplaceholder.typicode.com/users'),
      headers: {
        'Content-Type': 'application/json',
      },
      body: jsonEncode({
        'name': name,
        'email': email,
      }),
    );

    if (response.statusCode == 201) {
      Map<String, dynamic> newUser = jsonDecode(response.body);
      print('User created with ID: ${newUser['id']}');
    }
  } catch (e) {
    print('Error creating user: $e');
  }
}
```

## API Authentication

### 1. API Key Authentication
```dart
Future<void> fetchDataWithApiKey() async {
  final response = await http.get(
    Uri.parse('https://api.example.com/data'),
    headers: {
      'Authorization': 'Bearer YOUR_API_KEY_HERE',
      'Content-Type': 'application/json',
    },
  );
}
```

### 2. Basic Authentication
```dart
Future<void> fetchDataWithBasicAuth() async {
  final credentials = base64Encode(utf8.encode('username:password'));
  
  final response = await http.get(
    Uri.parse('https://api.example.com/data'),
    headers: {
      'Authorization': 'Basic $credentials',
    },
  );
}
```

## Handling API Responses

### 1. JSON Response
```dart
// API returns JSON data
{
  "id": 1,
  "name": "John Doe",
  "email": "john@example.com",
  "posts": [
    {"id": 1, "title": "First Post"},
    {"id": 2, "title": "Second Post"}
  ]
}

// Parse in Flutter
Map<String, dynamic> data = jsonDecode(response.body);
String userName = data['name'];
List<dynamic> posts = data['posts'];
```

### 2. Error Response
```dart
// API returns error
{
  "error": "User not found",
  "code": 404,
  "message": "The requested user does not exist"
}

// Handle in Flutter
if (response.statusCode != 200) {
  Map<String, dynamic> errorData = jsonDecode(response.body);
  String errorMessage = errorData['message'];
  print('API Error: $errorMessage');
}
```

## Creating API Service Classes

### 1. Basic API Service
```dart
class ApiService {
  static const String baseUrl = 'https://api.example.com';
  
  // Get user by ID
  static Future<Map<String, dynamic>> getUser(int userId) async {
    final response = await http.get(
      Uri.parse('$baseUrl/users/$userId'),
    );
    
    if (response.statusCode == 200) {
      return jsonDecode(response.body);
    } else {
      throw Exception('Failed to load user');
    }
  }
  
  // Create new user
  static Future<Map<String, dynamic>> createUser(String name, String email) async {
    final response = await http.post(
      Uri.parse('$baseUrl/users'),
      headers: {'Content-Type': 'application/json'},
      body: jsonEncode({
        'name': name,
        'email': email,
      }),
    );
    
    if (response.statusCode == 201) {
      return jsonDecode(response.body);
    } else {
      throw Exception('Failed to create user');
    }
  }
}
```

### 2. Using the API Service
```dart
class UserScreen extends StatefulWidget {
  @override
  _UserScreenState createState() => _UserScreenState();
}

class _UserScreenState extends State<UserScreen> {
  Map<String, dynamic>? userData;
  bool isLoading = true;

  @override
  void initState() {
    super.initState();
    loadUserData();
  }

  Future<void> loadUserData() async {
    try {
      final data = await ApiService.getUser(1);
      setState(() {
        userData = data;
        isLoading = false;
      });
    } catch (e) {
      setState(() {
        isLoading = false;
      });
      print('Error: $e');
    }
  }

  @override
  Widget build(BuildContext context) {
    if (isLoading) {
      return CircularProgressIndicator();
    }
    
    if (userData == null) {
      return Text('Failed to load user data');
    }
    
    return Column(
      children: [
        Text('Name: ${userData!['name']}'),
        Text('Email: ${userData!['email']}'),
      ],
    );
  }
}
```

## Common API Patterns

### 1. Pagination
```dart
// API with pagination
{
  "data": [...],
  "page": 1,
  "per_page": 10,
  "total": 100,
  "total_pages": 10
}

// Fetch with pagination
Future<List<dynamic>> fetchUsers(int page) async {
  final response = await http.get(
    Uri.parse('https://api.example.com/users?page=$page&per_page=10'),
  );
  
  if (response.statusCode == 200) {
    Map<String, dynamic> data = jsonDecode(response.body);
    return data['data'];
  }
  throw Exception('Failed to load users');
}
```

### 2. Search and Filtering
```dart
// Search users by name
Future<List<dynamic>> searchUsers(String query) async {
  final response = await http.get(
    Uri.parse('https://api.example.com/users?search=$query'),
  );
  
  if (response.statusCode == 200) {
    Map<String, dynamic> data = jsonDecode(response.body);
    return data['data'];
  }
  throw Exception('Failed to search users');
}
```

## Error Handling Best Practices

### 1. Network Error Handling
```dart
Future<void> makeApiCall() async {
  try {
    final response = await http.get(url);
    // Handle response
  } catch (e) {
    if (e is SocketException) {
      print('No internet connection');
    } else if (e is TimeoutException) {
      print('Request timed out');
    } else {
      print('Network error: $e');
    }
  }
}
```

### 2. Retry Logic
```dart
Future<Map<String, dynamic>> fetchWithRetry(String url, {int maxRetries = 3}) async {
  for (int i = 0; i < maxRetries; i++) {
    try {
      final response = await http.get(Uri.parse(url));
      if (response.statusCode == 200) {
        return jsonDecode(response.body);
      }
    } catch (e) {
      if (i == maxRetries - 1) throw e;
      await Future.delayed(Duration(seconds: 1 * (i + 1)));
    }
  }
  throw Exception('Failed after $maxRetries attempts');
}
```

## Testing APIs

### 1. Using Postman or Similar Tools
- Test API endpoints before implementing in Flutter
- Verify request/response formats
- Check authentication

### 2. Unit Testing
```dart
test('should fetch user data successfully', () async {
  // Mock the HTTP response
  when(mockHttpClient.get(any))
      .thenAnswer((_) async => http.Response(
        '{"id": 1, "name": "John Doe"}',
        200,
      ));

  final result = await ApiService.getUser(1);
  
  expect(result['name'], 'John Doe');
  expect(result['id'], 1);
});
```

## Popular APIs for Learning

### 1. JSONPlaceholder (Free)
```
https://jsonplaceholder.typicode.com
```
- Perfect for learning and testing
- No authentication required
- Simulates real API responses

### 2. OpenWeatherMap
```
https://openweathermap.org/api
```
- Weather data API
- Requires free API key
- Good for real-world practice

### 3. GitHub API
```
https://developer.github.com/v3/
```
- Real-world API with authentication
- Comprehensive documentation
- Good for advanced learning

## Summary

APIs are essential for modern app development. Key points to remember:

1. **APIs enable communication** between different applications
2. **HTTP methods** (GET, POST, PUT, DELETE) define the operation
3. **Authentication** secures API access
4. **Error handling** is crucial for robust apps
5. **Testing** ensures your API integration works correctly

Start with simple APIs like JSONPlaceholder to practice, then move to more complex ones as you gain confidence!
