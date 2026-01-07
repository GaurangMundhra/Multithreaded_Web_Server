# Multithreaded_Web_Server

Simple educational demos showing different approaches to handling concurrent socket clients in Java.

This repository contains three variants:
- SingleThreaded — (demo) a single-threaded server handling one client at a time. (Please confirm files you want reviewed here.)
- MultiThreaded — server spawns a new Thread for each incoming client connection. Includes a simple client and a JMeter test plan.
- ThreadPool — server uses a fixed-size thread pool (ExecutorService) to handle clients.

This repository is a small demo / teaching project to explore concurrency models for socket servers.

---

Table of contents
- Features
- Contents
- Requirements
- Build & run instructions
  - MultiThreaded (one-thread-per-client)
  - ThreadPool (fixed thread pool)
  - SingleThreaded (if present)
- Quick testing (telnet, curl, JMeter)
- Suggestions and recommended improvements
- Contributing

---

Features
- Minimal socket-based servers demonstrating concurrency approaches.
- A client that can create many client threads to exercise the MultiThreaded server.
- A JMeter test plan (MultiThreaded/TCP Sampler.jmx) for basic load testing.

Repository contents (important files)
- MultiThreaded/
  - Server.java — launches a ServerSocket and spawns a new Thread per connection (port 8080).
  - Client.java — creates 100 client threads that connect to the server and print server response.
  - TCP Sampler.jmx — JMeter test plan for load testing.
  - (Some .class files are present in repo — consider removing them.)
- ThreadPool/
  - Server.java — uses Executors.newFixedThreadPool(poolSize) and listens on port 8010.
- SingleThreaded/
  - (Files may exist here — inspect and add or convert as needed.)

Requirements
- Java JDK 8+ (javac/java from OpenJDK or Oracle JDK)
- JMeter (optional, to open / run .jmx test plan)

---

Build & run instructions (simple, no build tool)

Note: the Java sources in this repo currently use the default package (no `package` declarations). That works for ad-hoc compilation but is not recommended for production code. See "Suggested improvements" below.

1) Compile and run the MultiThreaded server
- Compile:
  - javac MultiThreaded/*.java
- Run server (port 8080):
  - java -cp MultiThreaded Server
- Run the client (in another terminal while server runs):
  - javac MultiThreaded/Client.java
  - java -cp MultiThreaded Client
  - The client will spawn multiple threads and print received lines from the server.

2) Compile and run the ThreadPool server
- Compile:
  - javac ThreadPool/*.java
- Run server (port 8010 by default):
  - java -cp ThreadPool Server

3) Compile and run SingleThreaded server (if present)
- If SingleThreaded/*.java sources exist:
  - javac SingleThreaded/*.java
  - java -cp SingleThreaded Server
  - (Adjust class name as needed if different.)

Notes on classpath & default package
- Because the files have no `package` declaration, compiling them into their directories and running with `-cp <folder>` works:
  - For example: `javac MultiThreaded/*.java` then `java -cp MultiThreaded Server`
- Recommended: introduce packages and a build tool (Maven or Gradle) for clarity.

Quick testing with telnet / netcat
- Start server (MultiThreaded on 8080)
- In other terminal:
  - telnet localhost 8080
  - or: nc localhost 8080
- The server will write a single line (e.g., "Hello from server!") and close the socket.

JMeter
- Open `MultiThreaded/TCP Sampler.jmx` in JMeter to run a basic load test against the server. Update the target host/port in the test plan as needed.

Suggested improvements (prioritized)
1. Add packages and move source to a conventional layout:
   - src/main/java/com/yourorg/servers/multithreaded/Server.java
   - This avoids the default package and enables better builds.
2. Add a build tool (Maven/Gradle) with tasks:
   - mvn compile / mvn exec:java or gradle run per variant.
3. Remove compiled artifacts (.class) and add `.gitignore`.
4. Improve resource handling:
   - Use try-with-resources for Socket/Streams.
   - Ensure thread pools are stopped cleanly (shutdown hooks).
5. Improve protocol:
   - If the goal is an HTTP server, implement proper HTTP responses (status line, headers, content-length).
   - Otherwise document the simple text protocol used.
6. Add logging, tests, and a lightweight CI workflow (GitHub Actions) to compile the code and run smoke tests.
7. Add README (this file), contribution guidelines, and a license.

Contributing
- Feel free to open issues or PRs. If you want, I can:
  - Create a clean project layout (add packages + Maven/Gradle).
  - Remove .class files and add .gitignore.
  - Add unit tests and GitHub Actions for build checks.
  - Improve the server to implement a minimal HTTP response and graceful shutdown.

License
- No license currently provided. Consider adding an open-source license (MIT/Apache-2.0) if you want to allow reuse.

---

If you'd like, I can:
- Push this README into the repository for you.
- Reorganize the project into a standard Maven/Gradle layout.
- Implement graceful shutdown and add try-with-resources to the connection handlers.
- Convert the servers to implement a minimal HTTP response so they can be tested with `curl` (recommended for web-server demos).
