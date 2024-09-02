![[Pasted image 20240223175149.png]]
Serialization is the process of translating a data structure into a sequence of bites. 
Deserialization is creating a data structure from a bit sequence.
Serialization is used to transfer objects over the network and to save them to files.

- Data exchange formats - JSON & XML & YAML
Create the data model:
```dart
class Todo {
	final int userId;
	final int id;
	final String title;
	final bool completed;
	
	Todo({
		required this.userId;
		required this.id;
		required this.title;
		required this.completed;
	});
	
	factory Todo.fromJson(Map<String, dynamic> json) {
		return Todo(
			userId: json['userId'] as int,
			id: json['id'] as int,
			title: json['title'] as String,
			completed: json['completed'] as bool,
		);
	}
}
```

Create the method for fetching of data from server.
```dart
import 'dart:convert';
import 'package:http/http.dart' as http;

Future<void> fetchTodo() async {
	final response = await http.get(
		Uri.parse('https://jsonplaceholder.typicode.com/todos/1'),
	);
	
	final String data = response.body;
	
	final map = jsonDecode(data) as Map<String, dynamic>;
	
	return Todo.fromJson(map);
}
```

Retrofit is package is used for generation of sample code. Automatic serialization & deserialization.
Example:
```dart
import 'package:retrofit/retrofit.dart';
import 'package:dio/dio.dart';

part 'todo_api.g.dart';

@RestApi()
abstract class TodoApi {
	factory TodoApi(Dio dio, {String baseUrl}) = _TodoApi;
	
	@GET("/todos/{id}")
	Future<Todo> fetchTodo(@Path("id") int id);
}
```
