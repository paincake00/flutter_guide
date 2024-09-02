
## Scaffold

Implements the basic Material Design visual layout structure. This class provides APIs for showing drawers, snack bars, and bottom sheets.

```dart
...
return Scaffold(
	backgroundColor: Colors.deepPurple[200],
	appBar: AppBar(
		title: const Text('The title'),
	),
	body: const Center(
		child: Text('The body'),
	),
	floatingActionButton: FloatingActionButton(
		onPressed: () {},
		tooltip: 'The button',
		child: const Icon(Icons.add),
	),
	bottomNavigationBar: BottomNavigationBar(
		items: const <BottomNavigationBarItem>[
			BottomNavigationBarItem(
	            icon: Icon(Icons.home),
	            label: 'Home',
	            backgroundColor: Colors.red,
	        ),
	        BottomNavigationBarItem(
		        icon: Icon(Icons.business),
		        label: 'Business',
		        backgroundColor: Colors.green,
	        ),
	        BottomNavigationBarItem(
		        icon: Icon(Icons.school),
		        label: 'School',
		        backgroundColor: Colors.blue,
	        ),
		],
		currentIndex: 0, // from 0, 1, 2
		selectItemColor: Colors.amber[800],
		onTap: (index) {},
	),
	drawer: ...,
	endDrawer: ...,
);
...
```

## Container

A convenience widget that combines common painting, positioning, and sizing widgets.

```dart
return Container(
	alignment: Alignment.center,
	constraints: BoxConstraints.tightForFinite(
		width: 200,
	),
	color: Colors.green,
	padding: EdgeInsets.all(25),
	margin: EdgeInsets.all(20),
	child: Text('The container'),
);
```

```dart
return Container(
	alignment: Alignment.center,
	width: 200,
	height: 200,
	transform: Matrxi4.rotationZ(0.05),
	color: Colors.green,
	child: Text('The container'),
);
```

```dart
return Container(
	child: Text('The container'),
	decoration: BoxDecoration(
		color: Colors.green,
		// shape: BoxShape.circle,
		borderRadius: BorderRadius.circular(20),
	),
	padding: EdgeInsets.all(25),
	margin: EdgeInsets.all(20),
);
```

## Text

A run of text with a single style.

```dart
return Text(
	"Some text",
	style: TextStyle(
		fontSize: 20,
		color: Colors.white,
		fontWeight: FontWeight.bold,
	),
);
```

## Icon

A Material Design icon.

```dart
Icon(
	Icons.favorite,
	color: Colors.red,
	size: 60,
),
```

## AppBar

Container that displays content and actions at the top of a screen.

```dart
return AppBar(
	title: const Text('My App Bar'),
	backgroundColor: Colors.deepPurple,
	elevation: 0,
	leading: IconButton(
		icon: const Icon(Icons.menu),
		onPressed: () {},
	),
	actions: [
		IconButton(
		  icon: const Icon(Icons.logout),
		  onPressed: () {},
		),
	],
);
```

## Column | Row

Layout a list of child widgets in the vertical / horizontal direction.

```dart
return Column( // or Row
	mainAxisAlignment: MainAxisAlignment.center, //start, end, spaceEvenly, spaceBetween
	crossAxisAlignment: CrossAxisAlignment.start, //center, end
	children: <Widget>[
		Expanded(
			child: Container(
				height: 200,
				width: 200,
				color: Colors.deepPurple,
			),
		),
		Expanded(
			flex: 2, // twice the size of the other
			child: Container(
				height: 200,
				width: 200,
				color: Colors.deepPurple[400],
			),
		),
		Expanded(
			child: Container(
				height: 200,
				width: 200,
				color: Colors.deepPurple[200],
			),
		),
	],
);
```

## ListView

A scrollable list of widgets arranged linearly.

```dart
return ListView(
	padding: const EdgeInsets.symmetric(vertical: 20, horizontal: 20),
	children: [
		Container(
			height: 300,
			color: Colors.deepPurple[900],
		),
		Container(
			height: 300,
			color: Colors.deepPurple[800],
		),
		Container(
			height: 300,
			color: Colors.deepPurple[700],
		),
		Container(
			height: 300,
			color: Colors.deepPurple[600],
		),
	],
);
```

```dart
return ListView.builder(
	itemCount: 10,
	itemBuilder: (context, index) => ListTile(
			title: Text('$index'),
	),
);
```

```dart
return ListView.separated(
	itemCount: 10,
	itemBuilder: (context, index) => ListTile(
			title: Text('$index'),
	),
	separatorBuilder: (context, index) {
		return const Divider(color: Colors.cyan);
	},
);
```

## GridView

A scrollable, 2D array of widgets.

```dart
return GridView.count(
	mainAxisSpacing: 20,
	crossAxisSpacing: 20,
	crossAxisCount: 4,
	children: <Widget>[
		...
	],
);
```

```dart
return GridView.builder(
	itemCount: 64,
	gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
		crossAxisCount: 3,
	),
	itemBuilder: (context, index) => Container(
		color: Colors.deepPurple,
		margin: EdgeInsets.all(2),
	),
);
```

## Stack

A widget that positions its children relative to the edges of its box.

```dart
return Stack(
	alignment: Alignment.center,
	children: [
		// big box
		Container(
			height: 300,
			width: 300,
			color: Colors.deepPurple,
		),
		// medium box
		Container(
			height: 200,
			width: 200,
			color: Colors.deepPurple[400],
		),
		// small box
		Container(
			height: 100,
			width: 100,
			color: Colors.deepPurple[200],
		),
	],
);
```

## GestureDetector

A widget that detects gestures.

```dart
return GestureDetector(
	onTap: () {}, // one from many functuions
	child: Container(),
);
```

## Drawer

A Material Design panel that slides in horizontally from the edge of a Scaffold to show navigation links in an application.

```dart
return Drawer(
	backgroundColor: Colors.deepPurple[100],
	child: Column(
		children: [
			const DrawerHeader(
				child: Icon(
					Icons.favorite,
					color: Colors.black,
					size: 50.0,
				),
			),
			ListTile(
				title: const Text('H O M E'),
				leading: const Icon(Icons.home),
				iconColor: Colors.black,
				onTap: () {
					Navigator.pushNamed(context, '/home');
				},
			),
			ListTile(
				title: const Text('S E T T I N G S'),
				leading: const Icon(Icons.settings),
				iconColor: Colors.black,
				onTap: () {
					Navigator.pushNamed(context, '/settings');
				},
			),
		],
	),
);
```

## Bottom Nav Bar

A material widget that's displayed at the bottom of an app for selecting among a small number of views, typically between three and five.

```dart
return BottomNavigationBar(
	items: const <BottomNavigationBarItem>[
		BottomNavigationBarItem(
			icon: Icon(Icons.home),
			label: 'Home',
			backgroundColor: Colors.red,
		),
		BottomNavigationBarItem(
			icon: Icon(Icons.business),
			label: 'Business',
			backgroundColor: Colors.green,
		),
		BottomNavigationBarItem(
			icon: Icon(Icons.school),
			label: 'School',
			backgroundColor: Colors.blue,
		),
	],
	currentIndex: 0, // from 0, 1, 2
	selectItemColor: Colors.amber[800],
	onTap: (index) {},
);
```

# TextField

A Material Design text field.

```dart
return SizedBox(
	width: 250,
	TextField(
		controller: controller,
		decoration: InputDecoration(
			border: OutlineInputBorder(),
			labelText: 'input some text',
		),
	),
);
```

## Image

```dart
const String assetPath = 'assets/images/maze_cube.png';

const String assetUrl = 'https://sun9-20.userapi.com/impg/q0uirW3UT2AWfdYNdnRskJQBVw0VgfFsdky4wQ/ds8lUG8dICs.jpg?size=320x180&quality=96&sign=71cd645acb4a1fe14b1ade5c515d493a&type=album';

Image imageFromAsset = Image.asset(assetPath, fit: BoxFit.cover);
  
Image imageFromNetwork = Image.network(assetUrl);

Image changedImage = Image(
	image: imageFromNetwork.image,
	// color: Colors.blue,
	// colorBlendMode: BlendMode.color,
	// fit: BoxFit.cover,
);
```

