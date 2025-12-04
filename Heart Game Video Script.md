1## Introduction (approx. 1 minute)


"Hello, and welcome to this walkthrough of my implementation of the Heart Game Project. My name is Aditya Poudel and my University ID is 2216027. In this video, I will break down the core engineering decisions behind this application, focusing on four key themes: Software Design Principles, Event-Driven Programming, interoperability, and Virtual Identity."

But before that, I want to show a short gameplay of what the game actually looks like. 


## 1. Software Design Principles (approx. 2.5 minutes)

**Concept:**

"For the software design, my primary goal was to achieve **low coupling and high cohesion**. I wanted to ensure that different parts of the application could evolve independently without breaking each other."
  

**Implementation - Factory Pattern:**

"A prime example of this is my use of the **Factory Pattern** in the frontend. I implemented a `GameModeFactory` class."

*(Show code: `Frontend/src/factories/gameModeFactory.js`)*

"Instead of cluttering the main Game component with complex `if-else` statements to handle 'Easy' and 'Hard' modes, I delegated this responsibility to the factory.

- The `GameModeFactory.createGameMode(mode)` method returns a specific class instance (`EasyGameMode` or `HardGameMode`).

- Each mode class encapsulates its own rules, like `timerBonus` and `allowIncorrect` logic.

- **Justification:** I considered simply putting this logic inside the Game component, but that would have violated the Single Responsibility Principle. By using a Factory, the Game component doesn't need to know *how* a mode works, just that it exists. This makes adding a future 'Medium' mode incredibly easy—I just add a class and update the factory, touching no other code."

  

**Implementation - Service Layer:**

"On the backend, I separated the business logic from the API routes using a **Service Layer** pattern."

*(Show code: `Backend/app/services/auth_service.py` vs `Backend/app/api/v1/auth.py`)*

"The API endpoints only handle HTTP requests and responses, while the `AuthService` handles the actual logic of registering and verifying users. This ensures high cohesion—related logic stays together."


## 2. Event-Driven Programming (approx. 2.5 minutes)
  

**Concept:**

"Next, let's talk about how the app responds to user actions. I adopted an **Event-Driven** architecture to decouple the components even further."

  

**Implementation - Custom Event System:**

"In the frontend, I implemented a custom `EventContext` using a publish-subscribe model."

*(Show code: `Frontend/src/context/EventContext.jsx`)*

"I created an `eventEmitter` that allows any component to broadcast events like `GAME_OVER` or `SCORE_UPDATED` without needing to pass callback functions down through five layers of components (prop drilling)."

  

**Justification:**

"**Why did I choose this?**

I considered using standard React props for everything. However, as the application grew, passing a 'Game Over' callback from the Timer component up to the Parent and then down to the Scoreboard became unmanageable (Prop Drilling).

By using an event emitter, the Timer simply emits `TIME_UP`, and the Game component—which is listening for that event—reacts immediately. This makes the system reactive and significantly cleaner."


## 3. Interoperability (approx. 2.5 minutes)
  

**Concept:**

"Interoperability is about how the frontend communicates with the backend API. I needed a robust way to handle data exchange."
  

**Implementation - Axios Interceptors:**

"I used the **Axios** library for HTTP requests, but I took it a step further by implementing **Interceptors**."

*(Show code: `Frontend/src/services/api.js`)*

"I have a request interceptor that automatically injects the `Authorization: Bearer [token]` header into every outgoing request.

I also have a response interceptor that listens for `401 Unauthorized` errors. If it catches one, it automatically attempts to refresh the token and retry the original request."

  
**Justification:**

"**Why this approach?**

I could have manually added the auth header to every single `fetch` call in my components. But that would be repetitive and error-prone (High Coupling). If the auth logic ever changed, I'd have to update every file.

With the interceptor, the interoperability logic is centralized. The components just ask for data, and the API service handles the *how* of the communication, ensuring seamless connectivity with the external Heart Game API."
  

**Implementation - Redis Caching & Base64 Optimization:**

"To further improve interoperability and performance, I implemented a **Redis Caching Layer**."

*(Show code: `Backend/app/cache/question_cache.py`)*

"The external Heart Game API can be slow, which would normally cause a delay for the user waiting for a question. To solve this, I use Redis to pre-fetch and cache questions.

Additionally, I specifically use the **Base64 version** of the external API."

*(Show code: `Backend/app/external/heart_api_client.py`)*


**Justification:**

"**Why Redis and Base64?**

I noticed that fetching questions in real-time created a poor user experience due to network latency.

- **Redis:** By caching questions, I ensure that when a user asks for the next question, it's served instantly from memory (microseconds) rather than waiting for an external HTTP request (seconds).

- **Base64:** Although the Base64 response body is slightly larger, my tests showed it renders significantly faster on the client side because it avoids additional round-trips to fetch image URLs. This trade-off of slightly larger payload for much faster rendering—was a deliberate choice to optimize the player's experience."

## 4. Virtual Identity (approx. 2.5 minutes)

**Concept:**

"Finally, Virtual Identity. Security was a top priority, specifically to identify who is playing."
  

**Implementation - JWT with Access & Refresh Tokens:**

"I implemented a stateless authentication system using **JSON Web Tokens (JWT)**."

*(Show code: `Backend/app/api/v1/auth.py` and `Frontend/src/services/api.js`)*

"When a user logs in, they receive two tokens:

1. **Access Token:** Short-lived (e.g., 15 minutes), used to access protected resources.

2. **Refresh Token:** Long-lived, used to get a new access token when the old one expires."


**Justification:**

"**I considered using Session Cookies**, which is the traditional way. However, session cookies require the server to store state for every active user, which can be hard to scale.

**I chose JWTs because:**

- **Scalability:** The server doesn't need to store session data; it just verifies the signature.

- **User Experience:** As I mentioned in my design, users visit the game frequently. It doesn't make sense to force them to log in every time. By using a Refresh Token flow, I can keep the user 'logged in' securely for days or weeks without compromising security. If the access token is stolen, it expires quickly. The refresh token is stored securely and can be revoked if needed."

## Conclusion

"In summary, by leveraging the Factory Pattern, Event-Driven Architecture, centralized API handling, and JWT Authentication, I've built a Heart Game that is not only functional but also modular, scalable, and secure. Thank you."