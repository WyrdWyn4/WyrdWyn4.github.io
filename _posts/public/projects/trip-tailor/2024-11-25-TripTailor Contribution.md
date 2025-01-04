---
title: My Contribution at TripTailor
description: A platform designed to revolutionize travel planning by offering a seamless and personalized experience for users to create, share, and explore travel itineraries.
date: 2024-11-26 00:00:00 +0800
categories: [Projects, TripTailor]
tags: [triptailor, software design, microservices, golang, react.js, postgresql, gin, docker, jwt authorization]
pin: false
math: true
mermaid: true
image: assets/public/img/covers/trip-tailor/triptailor-contribution-cover.png
author: wmksherwani
permalink: /triptailor/contribution
hidden: false
---

## Introduction

This report provides a comprehensive overview of my contribution to a full-stack development project that spans database architecture, backend service development, and frontend implementation. The project was an ambitious undertaking aimed at creating a seamless, user-friendly application with robust functionality, scalable design, and an intuitive interface.

Throughout the project, I encountered and addressed challenges such as managing inter-service synchronization, handling nested data relationships, and optimizing frontend responsiveness. Solutions included iterative code refinements, temporary synchronization fixes (e.g., window reloads for frontend consistency), and modularization of services for scalability.

This report is structured to detail my specific contribution, challenges faced, and the solutions implemented in each layer of development. Additionally, it includes an evaluation of the collaborative process, key pull requests, and a reflection on the experience. The aim is to provide a thorough account of my technical involvement while highlighting the skills and knowledge gained during the project.

## Database

The Database architecture was meticulously designed and implemented from the ground up to serve as the backbone for the system. The work spanned across multiple services, emphasizing robust functionality, maintainability, and scalability. Every aspect of the database, from its core CRUD operations to its integration with services and Docker containers, was thoughtfully developed to align with the system's requirements and objectives.

### Core Database Architecture

At the heart of the database is the Models package, where the foundation for database operations resides. The database design was as follows:

Based on this, we created a table-db.go file for every table (user-db.go, post-db.go, etc.), along with a database.go file, which has several helper functions for implementing fundamental CRUD operations that serve as the backbone for all subsequent database functionalities. This modular approach allows for easy scaling and integration of additional features, ensuring clean separation of concerns.

### Database-Driven Microservices

To enable microservices to interact seamlessly with the database, dedicated folders were created for each microservice. This structured approach ensures consistent integration of database functionalities across all services, fostering a scalable and maintainable system architecture. The initial database review was facilitated by the UserHandler, accessible at http://localhost:8080/users.

The integration of PostgreSQL as the primary database was a deliberate choice, reflecting a preference for its reliability and advanced features. Dependencies for github.com/lib/pq were meticulously managed through go.sum and go.mod files. Instead of adopting ORMs like GORM, I opted for database/sql to avoid unnecessary complexity and maintain control over database interactions. This decision aligns with broader community sentiments and simplifies debugging and optimization for the team.

### Docker Integration

The Docker configuration underwent significant refinement to streamline containerized deployment. The Dockerfile was tailored to align with the backend container's structure, incorporating COPY commands to include necessary files. The RUN command was adjusted to ensure the container starts directly at the program's entry point. In the Docker Compose configuration, obsolete fields like version were removed, and synchronization issues between database and backend containers were mitigated with a temporary 3-second delay. While functional, this solution calls for further refinement to ensure robust synchronization.

### Advanced Table and Relationship Management

The database schema was designed to encapsulate complex relationships while maintaining performance and clarity. Each entity—User, Board, Post, Itinerary, and Event—was modeled with detailed attributes and interdependencies. Recursive logic was implemented to handle many-to-many relationships, ensuring that updates to one table propagate correctly to related tables. For instance, adding a Post to a Board automatically updates the Post's list of associated Boards and vice versa. This functionality avoids inconsistencies but required careful handling to prevent infinite recursion, solved by introducing a recursion flag in the logic.

Relationships between tables were carefully considered. For example, the Itinerary and Post tables have a one-to-one relationship, where an Itinerary can exist independently, but a Post always references an Itinerary. Similarly, Posts and Boards share a many-to-many relationship, allowing versatile data organization. Linking columns were introduced to simplify relational operations, bypassing the complexity of traditional join tables. These columns, however, required auxiliary functions like IntsToStrings and StringsToInts to address limitations in PostgreSQL's array handling.

### Image Storage Implementation

A dedicated Images table was introduced to manage media assets. This table stores image data in a bytea format along with metadata describing its usage (e.g., profile photo, cover image). Helper functions like ImageToByte and ByteToImage were developed to facilitate conversions between image files and database-compatible formats. Additionally, web-based image storage and retrieval were implemented, with endpoints like http://localhost:8080/images/{id} enabling seamless access to stored images.

### Optimization and Refactoring

Throughout the development process, the codebase was continuously refined to improve efficiency and usability. For example, the AddPost and AddBoard functions were updated to automatically manage relationships with the User table, eliminating the need for redundant function calls. Similarly, a single function was created to handle table creation and deletion, streamlining database initialization.

The schema itself underwent iterative refinement to align with practical requirements. Columns were added, renamed, or removed to reflect the evolving application needs. For instance, the Post table was simplified by removing redundant attributes like Tags and PostImages, while the Itinerary table was enhanced with attributes like Title, Description, and Price.

### Concurrency and Synchronization Challenges

One recurring challenge was synchronizing the backend service with the database container during Docker Compose builds. Despite implementing a temporary delay to mitigate the issue, a long-term solution is necessary to ensure seamless operation, especially as the Docker configuration grows more complex.

### Philosophical and Design Considerations

The development process raised fundamental questions about the application's design and purpose. For instance, the role of Itineraries and their relationship with Users required re-evaluation to clarify the application's logic. Similarly, the utility and definition of Posts were examined to ensure their alignment with user workflows. These considerations informed decisions on schema design and functionality implementation, emphasizing the need for clarity and purpose in system design.

### Pull Requests

- Database Implementation #7
- Tt 13 mdp docker fixes #8
- Database Tables #15
- ConnString Database Function Updates #21
- Image Storage Implementation #27
- Database ReLogic #31
- Add Functions Optimization (Post and Board) #33
- Database Array + Itinerary Cost Fix #39

## Backend

### Save-Service Development

I was responsible for building the Save-Service, a crucial component in the backend architecture designed to manage user-saved content such as boards, posts, itineraries, and events. This service was developed from scratch, encompassing database interaction, API endpoint creation, and Docker integration to ensure seamless functionality and scalability.

#### API Implementation

The Save-Service provides a set of RESTful API endpoints to enable users to retrieve and manipulate saved content efficiently. These endpoints cover key functionalities, ensuring modularity and a logical flow for data retrieval:

**GET Endpoints:**
- `/boards`: Retrieves all boards associated with a user ID, enabling users to view their saved content collections.
- `/posts`: Fetches all posts tied to a specific board ID, supporting dynamic display and interaction with content within a board.
- `/itineraries`: Retrieves itineraries linked to a post ID, allowing for detailed navigation through saved plans.
- `/events`: Returns events associated with an itinerary ID, breaking down saved itineraries into actionable details.

**DELETE Endpoints:**
- `/boards/:boardId/posts/:postId`: Removes a post from a board without deleting the post entirely. This distinction ensures that users maintain ownership of their content while managing how it’s categorized.
- `/boards/:boardId`: Deletes a saved board entirely. In implementing this functionality, I carefully considered future use cases, such as duplicating boards, to ensure that deleting a board would not unintentionally affect its source content.

These routes were built with a focus on modularity, enabling easy extension as additional features or relationships are introduced.

#### Database Integration

The Save-Service interacts with the database to fetch, update, and delete data effectively. I designed the service to leverage existing queries while incorporating new ones as needed, ensuring that the service remained lightweight yet robust. By structuring the endpoints to handle nested relationships—such as boards containing posts, itineraries, and events—I ensured the data was retrieved in a logical hierarchy that aligns with frontend requirements.

#### Dockerization

To integrate the Save-Service into the broader system, I created a dedicated Docker container. The Dockerfile was set up to expose port 8086, and I incorporated a wait-for-it.sh script to address dependency management, ensuring the service only initialized once the database was fully operational. This setup ensured a smooth startup sequence within the multi-service architecture and laid the groundwork for seamless deployment.

### Challenges and Optimizations

1. **Inter-Service Synchronization**
   - Ensuring proper sequencing between services during initialization posed challenges. Temporary fixes, such as adding delays via wait-for-it.sh, were implemented, but I documented the need for more robust solutions like health checks in future updates.

2. **Handling Nested Relationships**
   - One of the more complex aspects of Save-Service was its handling of nested relationships, such as boards containing posts that link to itineraries and events. I developed efficient queries and API logic to ensure that related data could be fetched or modified without redundant calls, balancing performance and accuracy.

3. **Scalability Considerations**
   - I designed the service with extensibility in mind. By modularizing API routes and database interactions, Save-Service can accommodate future additions, such as tagging or sharing functionality, with minimal refactoring.

### Pull Requests

- My Travels page (My Boards / My Itineraries), Edit Profile page, and Waleed's latest backend stuff #36
- Tt pack the demo data everyone #45
- Save Service Endpoint Implementation #51
- Noah’s DB Packing PR #52
- Query Tags for Feed purposes #53

## Frontend

### Frontend Development Contribution

My contribution to the frontend development involved implementing critical features across multiple components, focusing on user experience, dynamic functionality, and seamless integration with backend services. These efforts revolved around creating intuitive pages, enhancing UI interactions, and ensuring the frontend properly interfaced with the backend APIs.

#### "My Travels" Page

I spearheaded the creation of the My Travels page, which serves as a hub for users to navigate their saved boards and itineraries. This page includes tabs for My Boards and My Itineraries, providing users with a streamlined overview of their saved content. Each tab allows users to explore and manage their collections, ensuring clear navigation through complex relationships like itineraries tied to posts and events. The overall architecture and layout were built to prioritize ease of access while providing space for future scalability.

#### My Boards and Board Posts Pages

For the My Boards page, I implemented functionality to fetch and display all user boards. Each board is presented with a dynamic image, and users can delete boards with a single click via an intuitive UI element—a hoverable "X" button. Clicking on a board directs users to the Board Posts page, which I also developed.

On the Board Posts page, users can view all posts associated with a specific board. Each post dynamically displays an image based on its events, defaulting to a placeholder image when no event image is available. I included a red hoverable "X" button for removing posts from boards without deleting the posts entirely, allowing users to maintain ownership of their content. The fetching logic integrates deeply with the Save-Service, ensuring accurate relationships between boards, posts, itineraries, and events are maintained.

#### User Itineraries Page

I extended functionality on the User Itineraries page, allowing users to view and interact with their saved itineraries. This required creating a fetchevents() function that reuses event-fetching logic from the Save-Service to efficiently retrieve itinerary-related data. I also revamped the display code to properly render images from the database. Itineraries and their corresponding events are stored in a two-dimensional array (itineraryEvents), ensuring each itinerary is visually and contextually connected to its events.

#### Profile Editing Features

I developed the Edit User Profile page, where users can update their personal details, including name, preferred tags, country of residence, and spoken languages. This page introduced a cleaner UI for profile customization while awaiting full backend integration. Similarly, I enhanced the Account Settings page by adding routing to ensure smooth navigation between profile-related functionalities.

#### Save-Service Endpoint Integration

On the frontend, I integrated new Save-Service endpoints to support advanced user interactions:

- **Add and Edit Boards**: Users can create new boards and update existing ones through /addboard and /editboard endpoints. I designed the UI for adding board descriptions and names, with functionality to automatically save itineraries to new boards upon creation.
- **Add and Remove Posts**: The /addboardpost and delete /post endpoints were implemented to enable attaching posts to boards or removing them dynamically. These additions ensure users can manage content relationships directly from the UI.

### UI Enhancements and Optimizations

Across multiple pages, I introduced subtle but impactful UI updates:

- In CreateItinerary.js, I relocated the Add Image button from the itinerary section to individual events, making the design more intuitive.
- On the Navbar.js, I updated the itinerary references to use titles instead of names, aligning with backend changes.
- I fixed display mismatches in the itinerary image logic, ensuring content loaded consistently across components.

### Challenges

One of the notable challenges during development was ensuring data consistency and responsiveness, especially when dealing with dynamic content loaded from the backend. In some instances, data from the Save-Service and other backend APIs did not load as quickly as expected due to asynchronous operations or delays in fetching complex relationships, such as boards, posts, itineraries, and events. This inconsistency occasionally resulted in incomplete or empty displays on the frontend, particularly when users navigated between pages or interacted with elements that relied on newly fetched data.

To address this, we implemented a temporary solution that involved triggering a window reload after certain user actions, such as creating or modifying boards or itineraries. This reload ensured that all components were correctly synchronized with the latest data.

### Pull Requests

- My Travels page (My Boards / My Itineraries), Edit Profile page, and Waleed's latest backend stuff #36
- Fixed Itinerary Image Display #41
- Save Service Endpoint Implementation #51

## Conclusion

In conclusion, working on this project was an immensely fulfilling experience, particularly as I contributed across the all database, backend, and frontend layers. Tackling the backend and database aspects allowed me to design robust solutions that ensured seamless data flow and service functionality. I also enjoyed diving into frontend development, where I could see my work take shape in a user-facing capacity, bridging the gap between the backend logic and the visual elements users interact with.

Collaboration with the team was a highlight of this journey. From brainstorming ideas to implementing features, it was inspiring to work alongside such talented individuals. This project deepened my technical skills, broadened my understanding of full-stack development, and showcased the power of teamwork. I had a lot of fun, and am grateful for the experience.