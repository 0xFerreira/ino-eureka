Phase 0: Environment & Toolchain Setup

This phase is about creating a professional, stable, and strict development environment. Do not proceed until you have a zero-warning build with all tools fully configured.

    [ ] 1. Install Core Toolchain:

        [ ] Install a modern C++ compiler. Clang (16+) is primary.

        [ ] Install CMake (3.28+).

        [ ] Install Ninja (1.11+).

    [ ] 2. Establish Initial CMakeLists.txt:

        [ ] Set C++ standard to 23.

        [ ] Enable Ninja as the generator (-G "Ninja").

        [ ] Configure compiler flags:

            -Wall -Wextra -Wpedantic -Wshadow -Wconversion

            -Werror (Treat warnings as errors).

        [ ] Configure build-type flags:

            Debug: -g -D_GLIBCXX_ASSERTIONS and sanitizers (-fsanitize=address,undefined).

            Release: -O3 (or -O2, profile later).

        [ ] Enable compile_commands.json generation (set(CMAKE_EXPORT_COMPILE_COMMANDS ON)).

    [ ] 3. Configure Static Analysis & Formatting:

        [ ] Create a .clang-format file. Enforce a consistent style project-wide.

        [ ] Create a .clang-tidy configuration file. Enable bugprone-*, performance-*, and modernize-* checks.

        [ ] Integrate clang-tidy into the build process or IDE.

    [ ] 4. Validate the Environment:

        [ ] Create a "Hello, World" main.cpp.

        [ ] Confirm the project compiles with zero warnings or errors.

        [ ] Confirm clang-tidy runs and reports no issues.

        [ ] Confirm clang-format works on the file.

        [ ] Confirm VS Code or your chosen editor correctly uses compile_commands.json for IntelliSense.

Phase 1: Core Architecture & Abstractions

With a stable environment, we now define the core architectural skeleton. The focus is on interfaces and data flow, not implementation details.

    [ ] 1. Establish Project Structure:

        [ ] Create the src/ directory.

        [ ] Organize code into subdirectories for logical units (simulation/, logic/, rendering/, core/).

    [ ] 2. Implement Initial Modules:

        [ ] Convert the "Hello, World" into a basic C++20 module structure as previously discussed.

        [ ] Ensure CMake correctly builds the modules before their dependents.

    [ ] 3. Create the Renderer Abstraction Layer:

        [ ] Define a pure abstract Renderer interface (renderer.h or a module).

        [ ] The interface must be completely independent of any third-party library (no SDL_ types in the function signatures).

        [ ] Create the first concrete implementation (SDLSimpleRenderer) using the basic SDL3 2D renderer.

        [ ] All game code must interact only with the Renderer interface, not the concrete class.

    [ ] 4. Integrate Initial Dependencies:

        [ ] Add SDL3 as a dependency.

        [ ] Add taskflow. Create a precompiled header (pch.h) containing taskflow.hpp and other heavy standard library headers to manage compile times. Configure target_precompile_headers in CMake.

Phase 2: Initial Systems & Simulation Stub

Now we breathe life into the architecture with the first data-driven systems.

    [ ] 1. Design the ECS Core:

        [ ] Define what an Entity is (it should just be an ID, like a uint32_t).

        [ ] Define the first few Components as plain-old-data structs (PositionComponent, SpriteComponent).

        [ ] Design the storage for components (e.g., std::vector of each component type).

    [ ] 2. Implement the First Systems:

        [ ] Create a basic RenderSystem that queries for entities with PositionComponent and SpriteComponent.

        [ ] The RenderSystem will use the abstract Renderer interface to issue draw calls.

        [ ] Create a stub TemperatureSystem module. It should contain an exported function that, for now, does nothing but print a message.

    [ ] 3. Implement the Job System:

        [ ] Integrate taskflow to create a global thread pool and job scheduler service.

        [ ] Refactor the main game loop to be driven by jobs. Create jobs for Input, Simulation, Physics, Rendering, etc., respecting their dependencies.
