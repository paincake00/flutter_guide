
This package lets to implement Router API (or Navigator 2.0) for your app.

### GoRouter config

Firstly, add GoRouter configuration to your app:

```dart
import 'package:go_router/go_router.dart';

// GoRouter config
final _router = GoRouter(
	initialRoute: '/login',
	redirect: _appRedirect,
	routes: [
		GoRoute(
			path: '/login',
			builder: (context, state) => LoginPage(),
		),
		GoRoute(
			path: '/users/:userId',
			builder: (context, state) => 
				const UserScreen(id: state.pathParameters['userId']),
		),
	],
	errorPageBuilder: (context, state) {
      return MaterialPage(
        key: state.pageKey,
        child: Scaffold(
          body: Center(
            child: Text(
              '😞 ${state.error.toString()}',
            ),
          ),
        ),
      );
    },
);

Future<String?> _appRedirect(context, state) async {
    final loggedIn = await _auth.loggedIn;
    final isOnLoginPage = state.matchedLocation == '/login';
  
    // Go to /login if the user is not signed in
    if (!loggedIn) {
      return '/login';
    }
    // Go to root if the user is already signed in
    else if (loggedIn && isOnLoginPage) {
      return '/home';
    }
    // no redirect
    return null;
  }
```

To use this configuration in your app, use the `MaterialApp.router` (or `CupertinoApp.router`):

```dart
class MyApp extends StatelessWidget {
	@override
	Widget build(BuildContext context) {
		return MaterialApp.router(
			routerConfig: _router,
		);
	}
}
```

### Navigation topic

To change to a new screen, call `context.go()` with a URL:

```dart
build(BuildContext context) {
	return TextButton(
		onPressed: () => context.go('/users/123'),
		// GoRouter.of(context).go('/users/123)
	);
}
```

To build a URI with query parameters, you can use the `Uri` class from the Dart standard library:

```dart
context.go(Uri(path: '/users/123', queryParameters: {'filter': 'abc'}).toString());
```

### Named routes topic

Instead of navigating to a route based on the URL, a GoRoute can be given a unique name. To configure a named route, use the `name` parameter:

```dart
GoRoute(
	name: 'song',
	path: 'songs/:songId',
	builder: /* ... */
),
```

To navigate to a route using its name, call `goNamed`:

```dart
TextButton(
  onPressed: () {
    context.goNamed('song', pathParameters: {'songId': 1});
  },
  child: const Text('Go to song 2'),
),
```

Alternatively, you can look up the location for a name using `namedLocation`:

```dart
TextButton(
	onPressed: () {
		final String location = context.namedLocation(
			'song',
			pathParameters: {
				'songId': 1,
			},
		);
		context.go(location);
	},
	child: const Text('Go to song 2'),
),
```

