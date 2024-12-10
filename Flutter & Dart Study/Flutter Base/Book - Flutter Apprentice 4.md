
### About Scaffold

Scaffold is composed of the following parts:
- AppBar
- BottomSheet
- BottomNavigationBar
- Drawer
- FloatingActionButton
- SnackBar

![[{7E756505-A8CE-43DB-B595-C1A0B9DAFE88}.png|400]]

### Text from theme

![[{87404D27-C917-4306-8C17-2DF46D865EF7}.png]]

### What is widget?

The common definition: A widget is a building block for your user interface.

The right definition: A widget is a blueprint for displaying the state of your app.

![[{8564ECE4-F11F-4BFF-B4EE-C902F0181C08}.png]]

### Widget Trees

Every widget contains a build() method. In this method, you create a UI composition by nesting widgets within other widgets. 

![[{CD7A6C2C-CF45-4339-BC4D-DF0AB2F822A5}.png|550]]

The framework traverses the nodes in the tree and calls each build() method to compose your entire UI.

### Rendering Widgets

The main layers of Flutter's architecture:
![[{FF2E131D-A041-4489-8C17-B55244EB0AFD}.png|350]]

Focus on the **framework layer**:
![[{5A343EE0-7C81-4813-9A33-EFFC078DFDC7}.png|350]]

- **Material and Cupertino** are UI control libraries built on top of the widget layer.
- **The Widgets layer** is a composition abstraction on widgets. It contains all the primitive classes needed to create UI controls.
- **The Rendering layer** is a layout abstraction that draws and handles the widget’s layout.
- **Foundation**, also known as the dart:ui layer, contains core libraries that handle animation, painting and gestures.

### Three Trees

Flutter’s framework actually manages not one, but three trees in parallel: 
- Widget Tree 
- Element Tree 
- RenderObject Tree

![[{F2EA26AB-B483-4ED9-821D-928B7A7B19F7}.png|600]]

- **Widget**: The public API or blueprint for the framework. Developers usually just deal with this layer.
- **Element**: Manages a widget and a widget’s render object. For every widget instance in the tree, there is a corresponding element.
- **RenderObject**: Responsible for drawing and laying out a specific widget instance. Also handles user interactions, like hit-testing and gestures.

### Types of Elements

The two types:
- **ComponentElement**: A type of element composed of other elements. This corresponds to composing widgets inside other widgets.
- **RenderObjectElement**: A type of element that holds a render object.
You can think of **ComponentElement** as a group of elements and **RenderObjectElement** as a single element.

Example of the three trees:

![[Pasted image 20241021130610.png|600]]

### Types of Widgets

There are three major types of widgets: **Stateless**, **Stateful** and **Inherited**.

### Stateless  Widgets

The state or properties of a stateless widget can’t be altered once it’s built.

![[{1240C53B-14D0-44B0-8B6B-7F35269F426C}.png]]

### Stateful Widgets

Stateful widgets preserve state, which is useful when parts of your UI need to change dynamically.

![[{1501DBF9-6975-42C6-8F38-856FFD166BCC}.png]]

##### State Object Lifecycle

Every widget’s `build()` method takes a `BuildContext` as an argument. The build context tells you where you are in the tree of widgets. You can access the **element** for any widget through the `BuildContext`.

**L I F E C Y C L E**

![[{3CEE7626-A462-4F0C-85C4-6E87275BAC8A}.png|500]]

1. When you assign the build context to the widget, an flag `mounted` is `true`. That is, this widget on the widget tree.
2. `initState()` - first method called after a widget is created.
3. The first time the framework builds a widget, it calls `didChangeDependencies()` after `initState()`. It might call `didChangeDependencies()` again if your state object depends on an inherited widget that has changed.
4. Finally, the framework calls `build()` after `didChangeDependencies()`.
5. The framework calls `didUpdateWidget(_)` when a parent widget makes a change or needs to redraw the UI. . When that happens, you’ll get the oldWidget instance as a parameter so you can compare it with your current widget and do any additional logic.
6. If you want to modify the state in your widget, you call `setState()`. The framework then marks the widget as `dirty` and triggers a `build()` again.
7. When you remove the object from the tree, the framework calls `deactivate()`.
8. The framework calls `dispose()` when you permanently remove the object and its state from the tree.

### Inherited Widgets

**InheritedWidget** — это специальный тип виджета в Flutter, который позволяет передавать данные вниз по дереву виджетов без необходимости передавать их через конструкторы. Это особенно полезно для управления состоянием и обеспечения доступа к данным в разных частях вашего приложения.

Inherited widgets let you access state information from the parent elements in the tree hierarchy.

![[{95C0EF65-596F-4BC9-ADB9-FCCD6BBEF2D2}.png|600]]

NOTE: There are several different types of scroll physics you can play with: 
- AlwaysScrollableScrollPhysics 
- BouncingScrollPhysics 
- ClampingScrollPhysics  
- FixedExtentScrollPhysics 
- NeverScrollableScrollPhysics 
- PageScrollPhysicsRange 
- MaintainingScrollPhysics 

### Slivers

**SliverAppBar** - AppBar, который способен складываться при прокрутке. Сам по себе виджет имеет скроллинг.

**SliverToBoxAdapter** - виджет, имеющий собственный скроллинг и содержащий любой дочерний виджет. 

### Routes & Navigation

Here’s a top-level and a side-level view of the navigation stack:

![[{4EDF19A3-4F05-42AD-AE11-01CAEE811EB3}.png]]

##### Navigator 1.0

Base syntax:

Push:
```dart
bool result = await Navigator.push( 
	context, 
	MaterialPageRoute( 
		builder: (BuildContext context) =>RestaurantPage( 
			restaurant: restaurants[index], 
			cartManager: cartManager, 
			ordersManager: orderManager, 
		),
	),
);
```

Pop:
```dart
Navigator.pop(context);
```

The main disadvantage of Navigator 1.0 (imperative style): *it's hard to manage and scale!*

##### Router API (Navigator 2.0)

Its main goals include: 
- Exposing the navigator’s page stack: You can now manipulate and manage your page routes. More power, more control! 
- Backward compatibility with imperative API: You can use imperative and declarative styles in the same app. 
- Handling operating system events: It works better with events like the Android and Web system’s Back button. 
- Managing nested navigators: It gives you control over which navigator has priority. 
- Managing navigation state: You can parse routes and handle web URLs and deep linking.

Here are the new abstractions that make up Router’s declarative API:

![[{ECECE075-6B9F-4A18-92A3-1DF4900EA8EC}.png]]

The new API includes the following key components: 
- Page: An abstract class that describes the configuration for a route. 
- Router: Handles configuring the list of pages the Navigator displays. 
- RouterDelegate: Defines how the router listens for changes to the app state to rebuild the navigator’s configuration. 
- RouteInformationProvider: Provides RouteInformation to the router. Route information contains the location info and state objects to configure your app. 
- RouteInformationParser: Parses route information into a user-defined data type.  
- BackButtonDispatcher: Reports presses on the platform system’s Back button to the router.
- TransitionDelegate: Decides how pages transition into and out of the screen.

Example:

![[{EDAE741A-5F9A-42F1-A364-F363A60BC993}.png]]

Here’s how it works: 
1. A user taps a button. 
2. The button handler tells the app state to update.
3. The router is a listener of the state, so it receives a notification when the state changes.
4. Based on the new state changes, the router reconfigures the list of pages for the navigator.
5. The navigator detects if there’s a new page in the list and handles the transitions to show the page.

### Deep Links

**Deep link** - a URL that navigates to a specific destination in your mobile app. 
Think of deep links like a URL address you enter into a web browser to go to a specific page of a website rather than the home page.

##### Types of Deep Links 

There are three types of deep links: 

- URI schemes: An app’s own URI scheme. yummy://kodeco.com/home is an example of Yummy’s URI scheme. This form of deep link only works if the user has installed your app.

- iOS Universal Links: In the root of your web domain, you place a file that points to a specific app ID to say whether to open your app or to direct the user to the App Store. You must register that specific app ID with Apple to handle links from that domain. 

- Android App Links: Like iOS Universal Links, Android App Links take users to a link’s specific content directly in your app. They leverage HTTP URLs and are associated with a website. For users that don’t have your app installed, these links go directly to the content of your website.