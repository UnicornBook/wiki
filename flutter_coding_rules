# Comprehensive Flutter Coding Rules & Guides

These rules are created for the Flutter team to maintain a consistent coding style and provide guidance on improving code quality.

## Naming Conventions

1. **Functions**: Should be named starting with a verb, e.g., `doSomething()`.
2. **Classes**: Should be nouns, e.g., `SomethingWidget`.
3. **Localization**: For one word verbs add `to` before the verb, e.g. `toDo`. 

### BLoC Naming
Should start with a noun and end with "Bloc".
  - For BLoCs that fetch data, use names like `NotesListBloc`.
  - For BLoCs that create or edit, use names like `NoteOperationBloc`.
  - For BLoCs that manage details of a model, use names like `NoteDetailsBloc`.

### Widget Naming
If related to a feature, start with the feature name, then the type of widget, otherwise same without a feature name.
  - Avoid general names like `Widget` as it does not provide any information.
  - For a widget that displays a list of items, use `NotesListView`.
  - For a widget that displays the content of a specific items, use `NoteDetailsCard`.

### Models
Use descriptive names that provide clear information about the model.
  - Avoid general names like `Model` or `Data`.
  - Instead, use specific names like `NoteDetails` or `NotesList`.

### Code Style

- Try to keep `StatelessWidget` under 100 lines of code, `StatefuleWidgets` under 200 lines. Large files are hard to read. We should understand the functionality of a class just by reading its name.

- Leave empty space before `return`, after declaring variables and to logically separate the code. This
  makes code more readable.

## Project Architecture
### Layered Structure
The architecture follows a layered approach:

* Data Layer: Handles API interactions using libraries like Retrofit and Dio.
* Repository Layer: Acts as a bridge between data and UI layers.
* State Management Layer: Manages app state using BLoC.
* UI Layer: Defines the user interface.

### Project Folder Structure
```
core/                   Contains routers, constants, and core utilities.
feature/
    ├── data/           Backend interaction logic (e.g., API calls).
    ├── repository/     Repository implementations.
    ├── model/          Domain and data model classes.
    ├── di/             Dependency injection (e.g., AppModule).
    ├── screen/         Feature-specific UI logic.
    ├── widget/         Reusable feature-specific widgets.
    ├── bloc/           BLoC implementations.
    ├── other/          Additional resources specific to the feature.
```
### For large features, organize further:

```
feature/
    ├── core/            Feature-specific data components.
    ├── presentation_a/  UI components for one part of the feature.
    ├── presentation_b/  UI components for another part of the feature.
```

## Toolkit
The toolkit is a shared resource for reusable components across projects.

### Toolkit Folder Structure
```
lib/
├── core/              Contains exceptions, interceptors, constants, and core interfaces.
├── external/          Modified external libraries tailored to project needs.
├── features/          Shared features and associated files.
├────  models/         Common data models.
├────  repository/     Interfaces for repositories.
├────  widget/         Common widgets.
├── theme/             Shared themes, colors, and text styles.
├── uikit/             Data-independent UI widgets.
├── utils/             Utility functions and non-UI classes.
```

### Implementing Common Logic
For shared logic like authentication:

1. **Interface Development**: Develop an interface for the repository in the toolkit. This interface defines a contract for interacting with the common API.

2. **Implementation in applications**: In applications, implement a data layer that conforms to the common logic defined in the toolkit. This implementation must conform to the interface developed in the toolkit.

With this approach, the common logic can be used in multiple projects without introducing a high coupling between them. This encourages code reuse and maintenance.

## Development Practices

1. **Linting**:
   - Address all linter warnings to maintain code quality.
   - If a particular linter warning seems unnecessary, discuss its deactivation with the team before proceeding.

2. **Data Classes**:
   - Always use data classes over `Map<String, dynamic>`.
   - Avoid using the `dynamic` keyword for better type safety and clarity.

3. **Abstract Classes**:
   - Use abstract classes where appropriate to define common functionality.
   - For scenarios where two applications use the same data classes with slight differences, create an abstract class and move it to the toolkit where the common logic resides.

### Code Smells

Learn about all code smells here: [Refactoring Guru - Code Smells](https://refactoring.guru/refactoring/smells)

### Common Code Smells in Flutter Projects

1. **Switch Statements**
   - **Issue**: Violates the Open-Closed Principle.
   - **Solution**: Replace switch statements with polymorphism to enhance maintainability.

2. **Shotgun Surgery**
   - **Issue**: Makes the code less maintainable due to scattered changes.
   - **Solution**: Consolidate related changes into a common class to simplify maintenance.

3. **Divergent Change**
   - **Issue**: Reduces maintainability by grouping unrelated responsibilities in a single class.
   - **Solution**: Separate methods with different responsibilities into distinct classes.

# Common Flutter Issues & Debugging Guide

## Common Flutter Issues

1. **Excessive Widget Rebuilds**:
   - Avoid calling `setState` indiscriminately. Only call `setState` when the state changes impact the UI. If a state variable change does not affect the UI, avoid rebuilding the widget tree.

2. **Caching Images**:
   - When using image assets, pre-cache them in the `initState` method to improve performance and avoid delays during image loading.

3. **Assets Management**:
   - When adding assets, ensure to specify the `package` property to indicate the package to which the asset belongs.

## Debugging

1. **Running in Debug Mode**:
   - Always run your project in debug mode during development to leverage features like breakpoints and hot reload.

2. **Using Breakpoints**:
   - Use breakpoints instead of print statements for debugging, as they provide more detailed information and help track the execution flow more effectively.

3. **Debugging Network Requests**:
   - Use the Flutter DevTools for debugging network requests. If the issue is with the backend, generate a `curl` command and share it with the backend developers for further investigation.

## Example: Adding a Notes Feature
1. Create a Feature Folder
Add a note folder with subfolders:

```
note/
├── cmodel/
├── cdata/
├── crepository/
├── cdi/
├── cbloc/
├── cwidget/
├── cscreen/
```

2. Define Models

```
@freezed
class Note with _$Note {
  const factory Note({
    required String title,
    required String content,
  }) = _Note;

  factory Note.fromJson(Map<String, dynamic> json) => _$NoteFromJson(json);
}
```
3. Create Data Source

```
@RestApi()
abstract class NotesApi {
  factory NotesApi(Dio dio, {required String baseUrl}) = _NotesApi;

  @GET('notes')
  Future<ApiResponse<List<Note>>> getNotes();

  @GET('notes/{id}')
  Future<ApiResponse<Note>> getNote(@Path('id') String id);

  @POST('notes')
  Future<ApiResponse<Note>> createNote(@Body() PostNoteBody postNoteBody);
}
```
4. Implement Repository:

```
@lazySingleton
class NotesRepository {
  final RepositoryExecutor _repositoryExecutor;
  final NotesApi _notesApi;

  NotesRepository(this._repositoryExecutor, this._notesApi);

  Future<List<Note>> getNotes() async {
    final response = await _repositoryExecutor.execute(_notesApi.getNotes);
    return response.data;
  }

  Future<Note> getNote(String id) async {
    final response = await _repositoryExecutor.execute(() => _notesApi.getNote(id));
    return response.data;
  }

  Future<Note> createNote(PostNoteBody postNoteBody) async {
    final response = await _repositoryExecutor.execute(() => _notesApi.createNote(postNoteBody));
    return response.data;
  }
}
```
5. Develop BLoC:

```
@injectable
class NotesBloc extends Bloc<NotesEvent, NotesState> {
  final NotesRepository _notesRepository;

  NotesBloc(this._notesRepository) : super(const NotesState.initial()) {
    on<NotesEvent>((event, emit) async {
      if (event is RequestedNotesEvent) {
        try {
          final notes = await _notesRepository.getNotes();
          emit(NotesState.success(notes: notes));
        } catch (e) {
          emit(NotesState.failure(e as AppException));
        }
      }
    });
  }
}
```

6. Create Widgets & Screens.

7. Update Routing: Add the screen to app_router.dart.

8. Add DI:
```
class NotesModule extends AppModule {
  @override
  void registerDependencies() {
    getIt.registerSingleton<NotesApi>(
      NotesApi(getIt<Dio>(), baseUrl: getIt<String>(instanceName: 'url')),
    );
  }
}
```
## Useful Commands
1. Generate Classes:

```
fvm flutter pub run build_runner build --delete-conflicting-outputs
```

2. Fix iOS Dependencies:

```
fvm flutter clean
fvm flutter pub get
fvm flutter precache --ios
rm -rf ios/Podfile.lock ios/Pods ios/.symlinks
cd ios && pod deintegrate && pod install --repo-update
cd ..
```

3.  Fix flutterfire Errors:

```
dart pub global activate flutterfire_cli
fvm flutter clean
fvm flutter pub get
fvm flutter precache --ios
cd ios && pod deintegrate && pod install --repo-update
cd ..
fvm flutter pub get
fvm flutter run
```
