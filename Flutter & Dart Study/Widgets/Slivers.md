
Slivers are used for creating of custom scroll effects.

Widget with all slivers:

```dart
CustomScrollView(
	slivers: [
		SliverAppBar(
			expandedHeight: 200,
			title: Text('Sliver App Bar'),
			flexibleSpace: FlexibleSpaceBar(
				background: _expandedImage,
			),
			floating: true, // floating bar  
		),
		SliverToBoxAdapter(
			child: ...,
		),
	],
);
```

SliverAppBar:

![[{AA752DAD-F19E-4EBD-81E7-B2B8FA2606D0}.png|500]]