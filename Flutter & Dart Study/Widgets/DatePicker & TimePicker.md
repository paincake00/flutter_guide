
The pretty widget of calendar:

```dart
final pickedDate = await showDatePicker(
	context: context,
	initialDate: DateTime.now(),
	firstDate: ...
	lastDate: ...
);
```

The pretty widget of time picker:

```dart
final pickedDate = await showTimePicker(
	context: context,
	initialEntryMode: TimePickerEntryMode.input,
	initialTime: TimeOfDay.now(),
	builder: (context, child) {
        return MediaQuery(
          data: MediaQuery.of(context).copyWith(
            alwaysUse24HourFormat: true,
          ),
          child: child!,
        );
    },
);
```

