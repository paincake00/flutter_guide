
Source 1: https://simpleone.ru/blog/mv-patterny-v-razrabotke-veb-prilozheniya
Source 2: https://medium.com/@mohammedkhudair57/mvi-architecture-pattern-in-android-0046bf9b8a2e
Source 3: https://docs.flutter.dev/app-architecture/case-study/ui-layer
##  MVC (Model-View-Controller)

![[Pasted image 20251013205120.png]]

- Model - бизнес-логика
- View - ответственен за отображение UI
- Controller - отвечает за взаимодействия с UI

#### Принцип



КРАТКО: User взаимодействует через Controller, который передает управление View.

Данные передаются по кругу:
-> View -> Controller -> Model ->

Это самый старый паттерн для разделения UI и бизнес-логики. 
- Сначала мы отправляем запрос на Controller (то есть, событие с данными). 
- Controller проводит определенные действия над данными (допустим, делает запрос на обновления данных в Model) и передает управление View.
- View уже получает новые данные у Model и пересоздает новый UI (например, генерирует HTML). Грубо говоря, в MVC паттерне View обновляется каждый раз по какому-либо событию.

## MVP (Model-View-Presenter)

![[Pasted image 20241223133845.png]]
![[Pasted image 20241223143231.png]]

- Model - всё та же бизнес-логика
- View - сам UI, с которым взаимодействует User.
- Presenter - компонент, который подписывается на события от View и в случае чего может получать запрос на обновление или получение данных от Model.

#### Принцип

КРАТКО: User взаимодействует с View, а Presenter подписывается на события от UI.

Presenter получает/передает данные для Model и также обращается View (меняет состояние). 

Этот принцип разделяет View и Controller.
- Пользователь взаимодействует с UI (слой View), вводя данные или совершая разные действия. Это все события.
- Presenter слушает эти события (обычно с данными), обрабатывает их и передает запрос в Model (на получение/обновления данных).
- Далее Presenter получает данные от Model и передает состояние обратно на View.

## MVVM (Model-View-ViewModel)

![[Снимок экрана 2024-12-23 в 15.32.02.png]]
![[Снимок экрана 2024-12-25 в 12.52.29.png]]

- Model - это бизнес-логика
- View - это UI
- ViewModel - модель представления, которая связывает Model и View через механизм привязки данных (data binding). 
#### Принцип

КРАТКО: View и ViewModel общаются посредством команд (binding). View слушает изменения от ViewModel при обновлении данных.

Этот принцип разделяет View и ViewModel: так можно связать любые View и ViewModel, главное, чтобы имелись нужные свойства (которые вызываются командами). 
ViewModel взаимодействует с моделью для получения, обновления и сохранения данных.
ViewModel - совмещение Model и Controller. При обновлении данных ViewModel уведомляет View. 

Code snippets:

Это пример реализации ViewModel:
```dart
class HomeViewModel extends ChangeNotifier {
	final BookingRepository _bookingRepository;
	final UserRepository _userRepository;

	late Command0 load; 
	late Command1<void, int> deleteBooking;
	
	HomeViewModel({
		required BookingRepository bookingRepository,
		required UserRepository userRepository,
	}) : _bookingRepository = bookingRepository,
      _userRepository = userRepository {
      // Load required data when this screen is built
      load = Command0(_load)..execute(); 
      deleteBooking = Command1(_deleteBooking);
    }

	User? _user;
	User? get user => _user;

	List<BookingSummary> _bookings = [];
	List<BookingSummary> get bookings => _bookings;

	Future<Result> _load() async {
		try {
		  final userResult = await _userRepository.getUser();
		  switch (userResult) {
			case Ok<User>():
			  _user = userResult.value;
			  _log.fine('Loaded user');
			case Error<User>():
			  _log.warning('Failed to load user', userResult.error);
		  }
	
		  // ...
	
		  return userResult;
		} finally {
		  notifyListeners();
		}
	}

	Future<Result<void>> _deleteBooking(int id) async {
	  try {
	    final resultDelete = await _bookingRepository.delete(id);
	    switch (resultDelete) {
	      case Ok<void>():
	        _log.fine('Deleted booking $id');
	      case Error<void>():
	        _log.warning('Failed to delete booking $id', resultDelete.error);
	        return resultDelete;
	    }
	
	    return resultLoadBookings;
	  } finally {
	    notifyListeners();
	  }
	}
}
```

Commands - это объекты, ответственные за взаимодействие, которое начинается в слое View и заканчивается в слое Model.
Command является типом, который помогает безопасно обновлять UI, независимо от времени и результата.

Код класса Command:
```dart
abstract class Command<T> extends ChangeNotifier {
  Command();
  bool running = false;
  Result<T>? _result;

  /// true if action completed with error
  bool get error => _result is Error;

  /// true if action completed successfully
  bool get completed => _result is Ok;

  /// Internal execute implementation
  Future<void> _execute(action) async {
    if (_running) return;

    // Emit running state - e.g. button shows loading state
    _running = true;
    _result = null;
    notifyListeners();

    try {
      _result = await action();
    } finally {
      _running = false;
      notifyListeners();
    }
  }
}
```

Слой View. Вызов команд из ViewModel:
```dart
@override
Widget build(BuildContext context) {
  return Scaffold(
      body: SafeArea(
        child: ListenableBuilder(
          listenable: viewModel,
          builder: (context, _) {
            return CustomScrollView(
              slivers: [
                SliverToBoxAdapter(),
                SliverList.builder(
                  itemCount: viewModel.bookings.length,
                  itemBuilder: (_, index) =>
                      _Booking(
                        key: ValueKey(viewModel.bookings[index].id),
                        booking: viewModel.bookings[index],
                        onTap: () =>
                            context.push(Routes.bookingWithId(
                                viewModel.bookings[index].id)
                            ),
                        onDismissed: (_) =>
                            viewModel.deleteBooking.execute(
                              viewModel.bookings[index].id,
                            ), // Вызов команды
                      ),
                ),
              ],
            );
          }
        )
      )
  );
}
```

## MVI (Model-View-Intent)

![[Pasted image 20241224222344.png]]
![[Pasted image 20241224222309.png]]

- Model - это бизнес-логика
- View - это UI
- Intent - представляет собой действия, которые пользователь может выполнить в приложении

КРАТКО: однонаправленная архитектура

View может не только запрашивать/передавать данные сам, но и сам получать при инициализации состояния. View не является таким ведущим, как в MVVM. Это подход более реактивный. 

Пользователь выполняет действие, которое будет Intent → Intent - это состояние, которое является входом в Model → Model хранит состояние и отправляет запрошенное состояние в View → View загружает состояние из Model → Отображает пользователю.