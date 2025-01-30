
Source 1: https://www.educative.io/answers/difference-between-implicit-and-explicit-animations-in-flutter#

Анимация — это процесс создания иллюзии движения путем последовательного отображения статических изображений или кадров. Каждый кадр отличается от предыдущего, создавая восприятие движения при быстром воспроизведении.

### Implicit и explicit анимация

![[Pasted image 20250130145039.png]]

#### Implicit (неявные)

Implicit анимация - это абстракция высокого уровня, которая позволяет разработчикам создавать плавные и визуально привлекательные переходы без ручного определения начального и конечного состояний анимации.

Пример implicit анимации через AnimatedOpacity, где по нажатию на кнопку в течение 3 секунд flutter logo меняет свою непрозрачность.

```dart
class LogoFade extends StatefulWidget {
	const LogoFade({super.key});
	  
	@override
	State<LogoFade> createState() => LogoFadeState();
}
class LogoFadeState extends State<LogoFade> {
	double opacityLevel = 1.0;
	  
	void _changeOpacity() {
		setState(() => opacityLevel = opacityLevel == 0 ? 1.0 : 0.0);
	}
	  
	@override
	Widget build(BuildContext context) {
		return Column(
			mainAxisAlignment: MainAxisAlignment.center,
			children: <Widget>[
				SizedBox(
					width: 50,
					height: 50,
					child: AnimatedOpacity(
						opacity: opacityLevel,
						duration: const Duration(seconds: 3),
						child: const FlutterLogo(),
					),
				),
				const SizedBox(height: 20),
				ElevatedButton(
					onPressed: _changeOpacity,
					child: const Text('Fade Logo'),
				),
			],
		);
	}
}
```

Еще пример с AnimatedContainer (AnimatedContainer постепенно меняет свои значения в течение определенного периода времени):

```dart
class _AnimatedContainerExampleState extends State<AnimatedContainerExample> {
  bool selected = false;

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: () {
        setState(() {
          selected = !selected;
        });
      },
      child: Center(
        child: AnimatedContainer(
          width: selected ? 200.0 : 100.0, // изменяются плавно при rebuild
          height: selected ? 100.0 : 200.0,
          color: selected ? Colors.red : Colors.blue,
          alignment:
              selected ? Alignment.center : AlignmentDirectional.topCenter,
          duration: const Duration(seconds: 2),
          curve: Curves.fastOutSlowIn,
          child: const FlutterLogo(size: 75),
        ),
      ),
    );
  }
}
```

#### Explicit (явные)

Explicit анимации требуют от разработчиков вручную указания начального и конечного состояний анимации, а также кривой интерполяции.
Этот уровень контроля обеспечивает большую гибкость и точность, позволяя разработчикам создавать пользовательские анимации в соответствии с их собственными предпочтениями. Flutter предлагает классы контроллеров анимации, такие как
- `AnimationController`
- `Tween`

Пример с Tween:

```dart
@override
void initState() {
	super.initState();
	
	_animationController = AnimationController(
		vsync: this,
		duration: const Duration(seconds: 3),
	);
	
	_animation = StepTween(begin: 0, end: endValue).animate(
		CurvedAnimation(
			parent: _animationController,
			curve: Curves.easeOutCubic, // небольшое ускорение в начале
		),
	)..addListener(
		() {
			setState(() {});
		},
	);
}
```

Можно применять анимации в AnimatedBuilder:

```dart
AnimatedBuilder(
	animation: _animation,
	builder: (context, child) {
		return Text(
			'>${_animation.value}',
		);
	}
),
```
