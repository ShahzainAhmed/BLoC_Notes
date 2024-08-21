## BLoC Theory (Notes)

### What is BLoC?

BLoC (Business Logic Component) is a design pattern in Flutter that helps separate the UI from the business logic, ensuring a clear distinction between the two. With BLoC, you can maintain this separation efficiently.

In the BLoC architecture, the UI and data layers are connected via BLoC, which acts as a mediator between them:

### UI ↔ BLoC ↔ Data

Communication between the UI and data layers occurs through two main concepts: Events and States.

1) Events: These are actions triggered by the user interface (UI). For example, if you're hosting a birthday party (event), you would invite your friends. Similarly, in UI, when you interact with elements like a "Like" button or a "Follow" button, an event is generated. Events represent actions that the user intends to perform, such as clicking a button or scrolling through a list. </br> It’s important to note that events are always initiated from the UI since the UI is the medium through which users interact with the application. Users can’t directly interact with models, databases, or backend services; they can only interact with UI components like buttons, lists, and containers.

2) States: After an event is triggered, the BLoC receives it and determines the necessary action. For example, if you click on a heart icon in a social media app, the event is sent to the BLoC. The BLoC then processes this event, such as updating the post's like status in the database. Once the data layer responds (e.g., with a successful 200 status code), the BLoC updates the UI state to reflect this change. <br> The updated state is then sent back to the UI, which could mean changing the heart icon from an outline to a filled heart, indicating that the post has been liked.

In summary, the BLoC pattern ensures that the UI is only responsible for displaying data, while the business logic and data management are handled by BLoC, maintaining a clean separation of concerns.

<p align="center" width="30%">
    <img width="60%" src="https://github.com/user-attachments/assets/94ab2d63-cd1d-46a5-a48e-7b0baf343935">
</p>

### UI ↔ BLoC Interaction

When using GestureDetector or InkWell, pressing a button (e.g., a heart button) triggers an event that the UI sends to the BLoC. An event in BLoC is represented by a class, and each event class should have a unique name. BLoCs can handle multiple types of states.

### Handling Events and States
When the heart button is pressed, the UI doesn’t update immediately. There's a delay because the action must be processed: the data must be sent to the database, the database must update, and a response must be received. During this time, a loading state can be shown, such as a bottom sheet, a snackbar, or an alert box, to indicate to the user that the process is ongoing.

Once the BLoC receives a success response from the database, it will emit a success state. The UI will then update accordingly, possibly showing a success message in a bottom sheet or alert box, indicating that the post has been liked or deleted successfully.

### BLoC Structure

Typically, you’ll have three main files in your BLoC structure:

1) home_bloc_events.dart: This file contains all the event classes. For example, on Instagram’s home screen, you might have events like ClickStoryEvent, LikePostEvent, FollowUserEvent, etc. Each of these events represents a user action in the UI, such as clicking on a story, liking a post, or following a user.

2) home_bloc_state.dart: This file contains all the state classes that the BLoC will emit in response to events. For instance, if a user clicks on a profile picture, a ProfilePictureClickedState could be emitted. If a user tries to report a post, a ReportPostState might be emitted, leading to the display of a bottom sheet asking for the reason for the report. After the report is confirmed, a ReportConfirmedEvent would be sent to the BLoC, which could then emit a ReportSuccessState.

3) home_bloc.dart: This file contains the logic for handling events and emitting states. It defines how the BLoC listens for specific events and what states to emit in response. This is also where API functions are called, handling the core logic of the application. The flutter_bloc package provides several widgets to interact with the BLoC.

### Core BLoC Widgets 

The flutter_bloc package provides three primary widgets:

1. BlocListener: This widget listens to state changes in the BLoC but does not build any UI. It’s used to execute side effects in response to state changes, like showing a bottom sheet or a snackbar. For example, you might wrap a Container with a BlocListener that listens for specific states and performs actions like showing a notification when a post is liked.

```
BlocListener<HomeBloc, HomeState>(
  listener: (context, state) {
    if (state is PostLikedState) {
      // Show a bottom sheet or snackbar
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Post liked!')),
      );
    }
  },
  child: Container(
    // Your UI here
  ),
);
```
2. BlocBuilder: This widget is used to build the UI based on the current state. It rebuilds the UI whenever the BLoC emits a new state.

3. BlocProvider: This widget provides a BLoC instance to the widget tree, making it available to other widgets in the subtree.
   
By following this structure, you can efficiently manage the flow of events and states in your Flutter app, ensuring that the UI reacts appropriately to user interactions and data updates.
