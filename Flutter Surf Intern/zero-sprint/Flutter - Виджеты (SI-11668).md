
Source 1: https://flutter.su/note/254
Source 2: https://dev.to/pranjal-barnwal/the-journey-of-a-widget-understanding-the-lifecycle-in-flutter-3plp

### Stateless и Stateful виджеты

Stateless виджеты неизменяемы и не имеют состояния. Они реализуют статичный интерфейс. 
Stateful виджет имеет состояние, которое можно изменять, что позволяет создавать динамический интерфейс.

### BuildContext

BuildContext - это элемент. Это абстрактный класс для реализации объектов (например, StatelessElement <- ComponentElement <- Element <- BuildContext), наследуемых от класса Element.

Объект context передается в каждый метод build().
Каждый элемент (контекст) содержит отсылку на свой «родительский» элемент, который создает тот самый элемент (контекст) при запуске приложения **Flutter**. 

Можно сказать, что приложение **Flutter** состоит из связанных друг с другом объектов **BuildContext**, которые и составляют «древо элементов». 
Они остаются неизменными в течение всего цикла приложения, в то время как их аналоги-«виджеты» могут быть удалены и воссозданы вновь и вновь.

### Методы BuildContext

Некоторые методы BuildContext:

- `findAncestorWidgetOfExactType`: находит предка виджета определенного типа.

- `findAncestorStateOfType`: находит состояние предка виджета определенного типа. Например, найти `ScaffoldState` для получения доступа к методам `Scaffold`.

- `findRootAncestorStateOfType`: находит корневое состояние предка виджета определенного типа.

- `dependOnInheritedWidgetOfExactType`: подписывается на изменения значения InheritedWidget определенного типа. Часто к нему обращаемся, когда используем метод `of`. Например, мы слушаем изменения у объекта класса `Theme`, чтобы динамически менять UI.

### State у Stateful виджета

State - это объект, который хранит состояние Stateful виджета. Он создается при создании виджета и уничтожается при удалении виджета.

Чтобы получить какой-то State, необходимо пройтись по дереву элементов, чтобы найти определенный элемент виджета, который содержит этот экземпляр класса State.

### Кто "знает" о State и кого "знает" State

State знает о своем виджете и его элементе. Виджет знает о своем состоянии через свойство `state`.

### Кто вызывает методы State

Методы State вызываются самим State или другими объектами, которые имеют доступ к State.

### Widget Lifecycle & Methods

### Construction
Когда создается виджет, вызывается конструктор, принимающий все необходимые параметры.

### Initialization
Затем фреймворк вызывает метод initState() у State объекта. 
Здесь вы можете выполнять любые задачи инициализации, требующие доступ к BuildContext. Можно создавать переменные, подписываться на потоки, настраивать слушателей и т.д.

### Build
Метод build() вызывается всякий раз, когда виджет отрисовывается или обновляется.
Он возвращает древо виджетов, задающее UI. 

### State Updates
Когда состояние виджета меняется, либо через взаимодействие с пользователем, изменение данных, либо через другие события, структура вызывает метод setState() объекта State.
### Rebuilding
Флаг dirty=true и необходимо повторно вызвать метод build(). Фреймворк сравнивает новое древо виджетов со старым, что оптимально внести изменения.

### Deactivation
Если виджет удален из дерева виджетов, но может быть добавлен обратно позже, фреймворк вызывает метод deactivate() объекта State виджета. Например, вы можете приостановить анимацию, отменить подписки или выпустить ресурсы.

### Disposal
Когда виджет удаляется из дерева виджетов навсегда, фреймворк вызывает метод dispose() объекта State виджета. Это последняя возможность для виджета выпустить ресурсы, отменить подписки или выполнить любые задачи по очистке. После вызова dispose() виджет и его состояние больше не пригодны для использования.


![Flutter Widget Lifecycle|400](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fdmycivygvvok2g4l1vd3.png)

### Методы жизненного цикла виджета

- `createState()`: создание State объекта этого виджета, который содержит изменяемое состояние.

- `initState()`: инициализирует состояние виджета. Вызывается единожды при создании виджета в первый раз. Внутри этого метода нельзя использовать `BuildContext.dependOnInheritedWidgetOfExactType`. Его можно использовать уже в следующем методе.

- `didChangeDependencies()`: вызывается сразу после `initState()`. Вызывается также если поменялся объект, от которого зависит данный виджет. Конкретно слушает`InheritedWidget`. 
  В нем можно использовать метода контекста: `dependOnInheritedWidgetOfExactType`, `findAncestorWidgetOfExactType`, `findAncestorStateOfType`, `findRootAncestorStateOfType`.

```dart
class _DefaultTabControllerListenerState
extends State<DefaultTabControllerListener> {
	TabController? _controller;
  
	@override
	void didChangeDependencies() {
		super.didChangeDependencies();
		final defaultTabController = DefaultTabController.maybeOf(context);
		if (defaultTabController != _controller) {
			_controller?.removeListener(_listener);
			_controller = defaultTabController;
			_controller?.addListener(_listener);
		}
	}
	  
	void _listener() {
		final controller = _controller;
		if (controller == null || controller.indexIsChanging) {
			return;
		}
		widget.onTabChanged(controller.index);
	}
	  
	@override
	void dispose() {
		_controller?.removeListener(_listener);
		super.dispose();
	}
	  
	@override
	Widget build(BuildContext context) {
		return widget.child;
	}
}
```

- `build()`: Вызывается каждый раз, когда необходимо построить (или перестроить) и вернуть древо виджетов. 
   В нем можно использовать метода контекста: `dependOnInheritedWidgetOfExactType`, `findAncestorWidgetOfExactType`, `findAncestorStateOfType`, `findRootAncestorStateOfType`.

- `didUpdateWidget()`: этот метод вызывается, когда родительский виджет изменяет свою конфигурацию и требует перестройки виджета. Он принимает старый виджет в качестве аргумента, позволяя сравнить его с новым виджетом. Используйте этот метод для обработки изменений в конфигурации виджета.
   В нем можно использовать метода контекста: `dependOnInheritedWidgetOfExactType`, `findAncestorWidgetOfExactType`, `findAncestorStateOfType`, `findRootAncestorStateOfType`.

```dart
class _LoveRepublicRadioCardState<T> extends State<LoveRepublicRadioCard<T>>
with SingleTickerProviderStateMixin {

late final AnimationController _controller;
final _easeInOutTween = CurveTween(curve: Curves.easeInOutCubic);
final _colorTween = ColorTween(begin: grayLine, end: black);
final _widthTween = Tween<double>(begin: 1, end: 1.3);
late final Animation<Color?> _borderColor;
late final Animation<double?> _borderWidth;
  
@override
void initState() {
	super.initState();
	_controller = AnimationController(duration: _kDuration, vsync: this);
	  
	_borderColor = _controller.drive(_colorTween.chain(_easeInOutTween));
	_borderWidth = _controller.drive(_widthTween.chain(_easeInOutTween));
	
	if (widget.groupValue == widget.value) _controller.value = 1.0;
}
  
@override
void dispose() {
	_controller.dispose();
	super.dispose();
}
  
@override
void didUpdateWidget(covariant LoveRepublicRadioCard<T> oldWidget) {
	super.didUpdateWidget(oldWidget);
	  
	if (oldWidget.groupValue != widget.groupValue) {
	_makeSelection(widget.groupValue == widget.value);
	}
}

void _makeSelection(bool isSelected) {
	isSelected ? _controller.forward() : _controller.reverse();
}
  
@override
Widget build(BuildContext context) {
	final child = Padding(
		padding: widget.pagging,
		child: LoveRepublicRadioTile(
			value: widget.value
			groupValue: widget.groupValue,
			title: widget.title,
			onChanged: widget.onChanged,
		),
	);
	return AnimatedBuilder(
		animation: _controller.view,
		builder: _builder,
		child: child,
	);
}
  
Widget _builder(BuildContext _, Widget? child) {
	return InkWell(
		focusColor: Colors.transparent,
		onTap: (widget.onChanged == null || widget.groupValue == widget.value)
			? null
			: () => widget.onChanged!.call(widget.value),
		child: DecoratedBox(
			decoration: BoxDecoration(
				border: Border.all(
					color: _borderColor.value!,
					width: _borderWidth.value!,
				),
			),
			child: child,
		),
	);
}
}
```

- `setState()`: метод setState() уведомляет структуру о том, что внутреннее состояние виджета изменилось и нуждается в обновлении.

- `deactivate()`: Этот метод вызывается, когда виджет удаляется из дерева виджетов (состояние сохраняется), но может быть повторно вставлен до завершения текущих изменений кадра. Методы контекста нельзя использовать, т.к. контекст уничтожен.

- `dispose()`: уничтожает состояние виджета при удалении виджета. Методы контекста нельзя использовать, т.к. контекст уничтожен.

### InheritedWidget

**InheritedWidget** — это специальный тип виджета в Flutter, который позволяет передавать данные вниз по дереву виджетов без необходимости передавать их через конструкторы. Это особенно полезно для управления состоянием и обеспечения доступа к данным в разных частях вашего приложения.

Пример:

```dart
import 'package:flutter/material.dart';
import 'package:counter_with_inherit/features/counter_app/domain/counter_controller.dart';

class CounterInherited extends InheritedWidget {
  final CounterController counterController;

  const CounterInherited({
    super.key,
    required this.counterController,
    required super.child,
  });

  static CounterController? maybeOf(BuildContext context) {
    return context
        .dependOnInheritedWidgetOfExactType<CounterInherited>()
        ?.counterController;
  }

  static CounterController of(BuildContext context) {
    final CounterController? counterController = maybeOf(context);
    assert(counterController != null, 'No CounterController found in context');
    return counterController!;
  }

  @override
  bool updateShouldNotify(covariant InheritedWidget oldWidget) => false;
}
```

На основе InheritedWidget основан пакет Provider, который добавляет новые виджеты для управления состоянием, новые конструкторы и т.д.

### Проекты с демонстрацией использования InheritedWidget и Provider

InheritedWidget: https://github.com/paincake00/counter_with_iw

Provider: https://github.com/paincake00/surf-flutter-summer-school-24