
## Default

![[Pasted image 20240801162607.png|280]]
Entry point: `main.dart`. All code in `src` directory.
## Types

![[Pasted image 20240801162827.png|280]]
Code (also in `src` dir) divided into types (controllers, models, services, widgets and etc).

## Layer

![[Pasted image 20240801165329.png|280]]

## Layer-by-Type

![[Pasted image 20240801165725.png|280]]
dir `use_case` - it is a set of callable classes, which if it were initialize fulfill `call` method with some action. 
For example, class `SignInUser`, which claim repository (in interface type), with `call()` method, which fulfill method `SignIn` of repository. After initializing of class `SignInUser` execute the method for sign in user. 
Use cases help to avoid the tight coupling between controllers and repositories.
## Feature

![[Pasted image 20240801174722.png|280]]

## Feature-by-Layer

![[Pasted image 20240801180405.png|280]]
dir `core` contain common different classes for general using by features.
`constant.dart` has a constant, `injector.dart` uses for dependencies injection and etc.
## Feature-by-Layer-by-Type

![[Pasted image 20240801180656.png|280]]
