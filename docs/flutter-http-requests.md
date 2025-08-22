# Flutter HTTP Requests

Making HTTP requests in Flutter allows your app to communicate with web servers and APIs. This guide covers the essentials.

## Setup

### Add HTTP Package
```yaml
# pubspec.yaml
dependencies:
  http: ^1.1.0
```

### Import
```dart
import 'package:http/http.dart' as http;
import 'dart:convert';
```

## Basic HTTP Methods

### GET Request
```dart
Future<void> fetchData() async {
  try {
    final response = await http.get(
      Uri.parse('https://jsonplaceholder.typicode.com/posts/1'),
    );

    if (response.statusCode == 200) {
      Map<String, dynamic> data = jsonDecode(response.body);
      print('Title: ${data['title']}');
    }
  } catch (e) {
    print('Error: $e');
  }
}
```

### POST Request
```dart
Future<void> createPost(String title, String body) async {
  final response = await http.post(
    Uri.parse('https://jsonplaceholder.typicode.com/posts'),
    headers: {'Content-Type': 'application/json'},
    body: jsonEncode({
      'title': title,
      'body': body,
      'userId': 1,
    }),
  );

  if (response.statusCode == 201) {
    print('Post created successfully');
  }
}
```

### PUT Request
```dart
Future<void> updatePost(int postId, String title) async {
  final response = await http.put(
    Uri.parse('https://jsonplaceholder.typicode.com/posts/$postId'),
    headers: {'Content-Type': 'application/json'},
    body: jsonEncode({'title': title}),
  );

  if (response.statusCode == 200) {
    print('Post updated successfully');
  }
}
```

### DELETE Request
```dart
Future<void> deletePost(int postId) async {
  final response = await http.delete(
    Uri.parse('https://jsonplaceholder.typicode.com/posts/$postId'),
  );

  if (response.statusCode == 200) {
    print('Post deleted successfully');
  }
}
```

## Working with JSON

### Parsing JSON Response
```dart
// Single object
Future<Map<String, dynamic>> fetchUser(int userId) async {
  final response = await http.get(
    Uri.parse('https://jsonplaceholder.typicode.com/users/$userId'),
  );

  if (response.statusCode == 200) {
    return jsonDecode(response.body);
  } else {
    throw Exception('Failed to load user');
  }
}

// List of objects
Future<List<Map<String, dynamic>>> fetchUsers() async {
  final response = await http.get(
    Uri.parse('https://jsonplaceholder.typicode.com/users'),
  );

  if (response.statusCode == 200) {
    List<dynamic> usersList = jsonDecode(response.body);
    return usersList.cast<Map<String, dynamic>>();
  } else {
    throw Exception('Failed to load users');
  }
}
```

## Authentication

### API Key
```dart
Future<void> fetchWithApiKey() async {
  final response = await http.get(
    Uri.parse('https://api.example.com/data'),
    headers: {
      'Authorization': 'Bearer YOUR_API_KEY',
      'Content-Type': 'application/json',
    },
  );
}
```

### Basic Auth
```dart
import 'dart:convert';

Future<void> fetchWithBasicAuth() async {
  final credentials = base64Encode(utf8.encode('username:password'));
  
  final response = await http.get(
    Uri.parse('https://api.example.com/data'),
    headers: {
      'Authorization': 'Basic $credentials',
    },
  );
}
```

## Query Parameters

```dart
Future<void> fetchWithParams() async {
  final uri = Uri.parse('https://api.example.com/users').replace(
    queryParameters: {
      'page': '1',
      'limit': '10',
      'search': 'john',
    },
  );

  final response = await http.get(uri);
}
```

## Error Handling

### Comprehensive Error Handling
```dart
Future<Map<String, dynamic>> fetchDataSafely() async {
  try {
    final response = await http.get(
      Uri.parse('https://api.example.com/data'),
    );

    if (response.statusCode == 200) {
      return jsonDecode(response.body);
    } else if (response.statusCode == 404) {
      throw Exception('Data not found');
    } else if (response.statusCode == 401) {
      throw Exception('Unauthorized');
    } else {
      throw Exception('HTTP ${response.statusCode}');
    }
  } catch (e) {
    if (e is SocketException) {
      throw Exception('No internet connection');
    } else if (e is TimeoutException) {
      throw Exception('Request timed out');
    } else {
      throw Exception('Network error: $e');
    }
  }
}
```

### Retry Logic
```dart
Future<Map<String, dynamic>> fetchWithRetry(String url, {int maxRetries = 3}) async {
  for (int attempt = 1; attempt <= maxRetries; attempt++) {
    try {
      final response = await http.get(Uri.parse(url));
      if (response.statusCode == 200) {
        return jsonDecode(response.body);
      }
    } catch (e) {
      if (attempt == maxRetries) throw e;
      await Future.delayed(Duration(seconds: attempt * 2));
    }
  }
  throw Exception('Failed after $maxRetries attempts');
}
```

## API Service Class

```dart
class ApiService {
  static const String baseUrl = 'https://jsonplaceholder.typicode.com';
  
  static Future<Map<String, dynamic>> get(String endpoint) async {
    final response = await http.get(Uri.parse('$baseUrl$endpoint'));
    
    if (response.statusCode == 200) {
      return jsonDecode(response.body);
    } else {
      throw Exception('GET failed: ${response.statusCode}');
    }
  }
  
  static Future<Map<String, dynamic>> post(String endpoint, Map<String, dynamic> data) async {
    final response = await http.post(
      Uri.parse('$baseUrl$endpoint'),
      headers: {'Content-Type': 'application/json'},
      body: jsonEncode(data),
    );
    
    if (response.statusCode == 201) {
      return jsonDecode(response.body);
    } else {
      throw Exception('POST failed: ${response.statusCode}');
    }
  }
}
```

## Using Models

### User Model
```dart
class User {
  final int id;
  final String name;
  final String email;

  User({
    required this.id,
    required this.name,
    required this.email,
  });

  factory User.fromJson(Map<String, dynamic> json) {
    return User(
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

### API Service with Models
```dart
class UserApiService {
  static Future<User> getUser(int userId) async {
    final response = await http.get(
      Uri.parse('https://jsonplaceholder.typicode.com/users/$userId'),
    );

    if (response.statusCode == 200) {
      return User.fromJson(jsonDecode(response.body));
    } else {
      throw Exception('Failed to load user');
    }
  }

  static Future<List<User>> getUsers() async {
    final response = await http.get(
      Uri.parse('https://jsonplaceholder.typicode.com/users'),
    );

    if (response.statusCode == 200) {
      List<dynamic> usersJson = jsonDecode(response.body);
      return usersJson.map((json) => User.fromJson(json)).toList();
    } else {
      throw Exception('Failed to load users');
    }
  }
}
```

## UI Integration

### Loading State Management
```dart
class UserListScreen extends StatefulWidget {
  @override
  _UserListScreenState createState() => _UserListScreenState();
}

class _UserListScreenState extends State<UserListScreen> {
  List<User> users = [];
  bool isLoading = true;
  String? error;

  @override
  void initState() {
    super.initState();
    loadUsers();
  }

  Future<void> loadUsers() async {
    try {
      setState(() {
        isLoading = true;
        error = null;
      });

      final userList = await UserApiService.getUsers();
      
      setState(() {
        users = userList;
        isLoading = false;
      });
    } catch (e) {
      setState(() {
        error = e.toString();
        isLoading = false;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    if (isLoading) {
      return Center(child: CircularProgressIndicator());
    }

    if (error != null) {
      return Center(
        child: Column(
          children: [
            Text('Error: $error'),
            ElevatedButton(
              onPressed: loadUsers,
              child: Text('Retry'),
            ),
          ],
        ),
      );
    }

    return ListView.builder(
      itemCount: users.length,
      itemBuilder: (context, index) {
        final user = users[index];
        return ListTile(
          title: Text(user.name),
          subtitle: Text(user.email),
        );
      },
    );
  }
}
```

## Best Practices

1. **Always handle errors** - Network requests can fail
2. **Check status codes** - Don't assume success
3. **Show loading states** - Keep users informed
4. **Use models** - Type-safe data handling
5. **Implement retry logic** - Handle temporary failures
6. **Add timeouts** - Prevent hanging requests
7. **Cache responses** - Improve performance when appropriate

## Testing

```dart
test('should fetch user successfully', () async {
  when(mockHttpClient.get(any))
      .thenAnswer((_) async => http.Response(
        '{"id": 1, "name": "John Doe"}',
        200,
      ));

  final user = await UserApiService.getUser(1);
  
  expect(user.name, 'John Doe');
  expect(user.id, 1);
});
```

## Summary

Key points for HTTP requests in Flutter:

1. Use the `http` package for network requests
2. Handle different HTTP methods (GET, POST, PUT, DELETE)
3. Parse JSON responses properly
4. Implement proper error handling
5. Use models for type safety
6. Show loading and error states in UI
7. Test your API calls

Start with simple GET requests and gradually add more complex functionality!
