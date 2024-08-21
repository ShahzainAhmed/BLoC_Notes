## BLoC Theory (Notes)

### What is BLoC?

BLoC (Business Logic Component) is a design pattern in Flutter that helps separate the UI from the business logic, ensuring a clear distinction between the two. With BLoC, you can maintain this separation efficiently.

In the BLoC architecture, the UI and data layers are connected via BLoC, which acts as a mediator between them:

### UI ↔ BLoC ↔ Data

Communication between the UI and data layers occurs through two main concepts: Events and States.

1) Events: These are actions triggered by the user interface (UI). For example, if you're hosting a birthday party (event), you would invite your friends. Similarly, in UI, when you interact with elements like a "Like" button or a "Follow" button, an event is generated. Events represent actions that the user intends to perform, such as clicking a button or scrolling through a list. </br> It’s important to note that events are always initiated from the UI since the UI is the medium through which users interact with the application. Users can’t directly interact with models, databases, or backend services; they can only interact with UI components like buttons, lists, and containers.

2) States: After an event is triggered, the BLoC receives it and determines the necessary action. For example, if you click on a heart icon in a social media app, the event is sent to the BLoC. The BLoC then processes this event, such as updating the post's like status in the database. Once the data layer responds (e.g., with a successful 200 status code), the BLoC updates the UI state to reflect this change. <br> The updated state is then sent back to the UI, which could mean changing the heart icon from an outline to a filled heart, indicating that the post has been liked.

In summary, the BLoC pattern ensures that the UI is only responsible for displaying data, while the business logic and data management are handled by BLoC, maintaining a clean separation of concerns.

![BLoC](https://github.com/user-attachments/assets/94ab2d63-cd1d-46a5-a48e-7b0baf343935)
