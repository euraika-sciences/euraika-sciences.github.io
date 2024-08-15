---
layout: post
title:  "eurAIka Architecture Design"
date:   2024-08-15 15:06:00 -0500
categories: eurAIka update
---

This project involves building a desktop application called **eurAIka** using **Svelte/SvelteKit** for the frontend and integrating multiple applications within dynamic panels using **Tauri** and **Rust**. The application will allow users to create, manage, and interact with different panel-based layouts containing various embedded apps.

## Background and Requirements

EurAIka is a desktop application designed to enhance productivity by integrating multiple tools into a single, customizable interface. The core idea is to allow users to work with various applications, such as markdown editors, web browsers, code editors, and collaboration tools, within a unified environment. This setup minimizes the need for switching between different applications and provides a fluid workflow where users can focus on their tasks with reduced friction.

The application leverages Svelte/SvelteKit for a responsive frontend, while Tauri and Rust are used to integrate and manage external applications within the eurAIka environment. This combination allows eurAIka to deliver a lightweight, fast, and secure user experience.

**Requirements**

Below are the requirements for eurAIka, categorized based on the [MoSCoW prioritization method](https://www.productplan.com/glossary/moscow-prioritization/#:~:text=MoSCoW%20prioritization%2C%20also%20known%20as,will%20not%20have%20right%20now.):

**Must Have**

- A single-page interface that supports 1-4 panels visible simultaneously.
- Panels that can contain various embedded applications:
  - "Whiteboard" panel with the Milkdown markdown editor.
  - "Librarian" panel with a web browser for accessing sites like SciSpace.
  - "Coder" panel with VSCodium, JupyterLab, or browser-based coding/science apps.
  - "Other" panel with apps such as Zotero, video conferencing tools, or other productivity apps.
- User functionality to:
  - Resize panels as desired.
  - Move panels relative to each other.
  - Open or close panels.
  - Toggle full-screen mode for the currently focused panel using a hotkey.
- Ability to save and load panel layouts, including positions, sizes, and the specific apps in use.
- A global menu accessible from the top of the page to set overall application options.

**Should Have**

- Customizable panel themes to allow users to match their workspace aesthetics.

**Could Have**

- Support for multiple simultaneous workspaces with quick switching capabilities.
- Integration of additional collaborative features, like shared whiteboards or chat.

**Won't Have (Yet)**

- Advanced AI-based features for workspace optimization or suggestions.
- Native support for platforms other than desktop (e.g., mobile or web-based).

## Method

The technical implementation of eurAIka involves several key components: the frontend built with Svelte/SvelteKit, backend integration using Tauri and Rust, panel management, layout persistence, and app embedding. Below is a breakdown of the architectural design.

**Architecture Overview**

The eurAIka application is structured into the following primary components:

![euraika-frontend](/assets/img/2024-08-15-euraika-architecture-design\euraika-frontend.png)

- **Frontend (Svelte/SvelteKit)**
  - **Main Interface**: The core user interface where panels and the global menu are displayed.
  - **Panel Manager**: Responsible for rendering, resizing, moving, and managing the state of panels.
  - **Layout Manager**: Handles the saving and loading of panel layouts.
  - **Global Menu**: A persistent menu at the top of the application for setting global options, such as theme, preferences, and layout management.
- **Backend (Tauri/Rust)**
  - **App Integration**: Manages the embedding of external applications within panels, interfacing with the system to launch and control apps like VSCodium, JupyterLab, and browsers.
  - **System Management**: Provides low-level access to the operating system for tasks such as window management, hotkey listening, and resource management.
  - **Layout Persistence**: Manages the storage and retrieval of layout configurations, saving them to a local database or file system.

**Panel Management**

The Panel Manager component is responsible for the dynamic arrangement of panels within the eurAIka interface. Key functionalities include:

- **Dynamic Resizing and Movement**: Implemented using the `svelte-dnd-action` library for drag-and-drop functionality, allowing users to resize and move panels within the interface. Each panel is a Svelte component that dynamically adjusts to user interactions.
- **Full-Screen Mode**: Triggered by a hotkey (e.g., F11), this feature expands the focused panel to full screen. The `svelte:window` directive will be used to listen for key events and adjust the panel size accordingly.
- **Opening/Closing Panels**: Panels can be added or removed dynamically, with the state managed by the Panel Manager. The visibility of each panel is controlled by Svelte stores, which maintain the application's state.

**Layout Persistence**

Layout persistence is crucial for providing users with a seamless experience where they can save and restore their preferred setups. The Layout Manager component interacts with Tauri to store layout configurations, including:

- **Layout Data Structure**: Each layout is represented as a JSON object, containing details about panel sizes, positions, and the applications embedded within each panel.
- **Saving Layouts**: When a user saves a layout, the JSON representation is sent to the Tauri backend, which writes it to the local file system or a database.
- **Loading Layouts**: Upon loading, the JSON layout is parsed, and the Panel Manager reconfigures the interface to match the saved state.

Example of a layout JSON object:

```json
{
  "layout_name": "Two Panel - Whiteboard & Librarian",
  "panels": [
    {
      "name": "Whiteboard",
      "size": { "width": "50%", "height": "100%" },
      "position": { "x": 0, "y": 0 }
    },
    {
      "name": "Librarian",
      "size": { "width": "50%", "height": "100%" },
      "position": { "x": "50%", "y": 0 }
    }
  ]
}

```

**Integration with External Applications**

Tauri and Rust are used to embed and manage external applications within the eurAIka panels. This involves:

- **Application Embedding**: Each panel that requires an external app (e.g., VSCodium or JupyterLab) communicates with the Tauri backend. Tauri, leveraging Rust, launches the application in a separate process and embeds it into the panel using a webview or native window.
- **Menu Integration**: Each embedded application retains its native menu, but the eurAIka global menu is implemented using Svelte and overlays the interface, providing access to global settings and layout options.
- **Security and Sandboxing**: Tauri’s security model ensures that embedded applications are sandboxed, reducing the risk of malicious code execution. The Rust backend handles system-level operations while maintaining a secure environment for the frontend.

**Component Diagram**

Here is a more detailed component diagram showing how the pieces interact:

![euraika-external-apps](/assets/img/2024-08-15-euraika-architecture-design\euraika-external-apps.png)

## Implementation

The implementation of eurAIka involves several stages, from setting up the development environment to integrating specific features and external applications. Below is a step-by-step guide to building the application.

**Step 1.1: Initialize the Svelte/SvelteKit Project**

- Install SvelteKit using the following commands:

  ```bash
  npm create svelte@latest eurAIka
  cd eurAIka
  npm install
  ```

Configure the project structure:

- Create directories for `components`, `stores`, and `layouts` to organize the codebase.
- Add the necessary SvelteKit configuration in `svelte.config.js`.

**Step 1.2: Set Up Tauri Integration**

- Initialize Tauri within the existing SvelteKit project:

  ```bash
  npm install @tauri-apps/cli @tauri-apps/api
  npx tauri init
  ```

- Configure Tauri in `src-tauri/tauri.conf.json` to handle the desktop environment:

  - Set `window` properties for the main application window.
  - Define any additional processes or permissions needed for embedding external applications.

**Step 1.3: Set Up Rust Backend**

- Install Rust (if not already installed):

  ```bash
  curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
  ```

- Modify `src-tauri/src/main.rs` to include basic system management, app integration, and layout persistence logic.

**Step 2.1: Implement the Main Interface**

- Create a `MainLayout.svelte` file to define the overall structure of the application:
  - Include the global menu component.
  - Set up a grid or flex-based system to allow dynamic panel management.

**Step 2.2: Develop the Panel Manager**

- Implement the `PanelManager.svelte` component:
  - Use `svelte-dnd-action` for drag-and-drop and resizing functionality.
  - Manage the state of panels (open/close, resize, move) using Svelte stores.
  - Bind keyboard events for hotkey functionality (e.g., toggling full-screen mode).

**Step 2.3: Create Layout Manager**

- Create a `LayoutManager.svelte` component to handle saving/loading layouts:
  - Define Svelte stores to manage layout data (e.g., panel positions, sizes).
  - Implement functions to save layouts as JSON and send them to the Rust backend for persistence.
  - Implement functions to load layouts from the backend and reconfigure the interface.

**Step 2.4: Build the Global Menu**

- Implement a `GlobalMenu.svelte` component:
  - Create options for saving/loading layouts, switching themes, and other global settings.
  - Use SvelteKit’s built-in routing and state management to handle menu interactions.

**Step 3.1: Embed Applications**

- Use Tauri to launch and embed external applications:

  - Modify `src-tauri/src/main.rs` to include commands for launching apps like VSCodium, JupyterLab, and web browsers.

  - Embed these apps into the eurAIka interface using Tauri's Webview API or native window embedding.

  - Example of embedding a web app:

    ```rust
    use tauri::api::process::Command;
    
    #[tauri::command]
    fn open_web_browser(url: &str) {
        Command::new("path_to_browser")
            .arg(url)
            .spawn()
            .expect("Failed to open browser");
    }
    ```

**Step 3.2: Manage App State**

- Ensure that the state of each embedded app (e.g., opened files in VSCodium, notebooks in JupyterLab) is managed by communicating with the app via its API or native interfaces.
- Implement controls in the Panel Manager to interact with embedded apps (e.g., refresh, close, or maximize).

**Step 4.1: Save Layouts**

- Implement a Rust function to save layout data to the file system or a local database:

```rust
use std::fs;
use serde_json::json;

fn save_layout(layout_name: &str, layout_data: &str) {
    let path = format!("./layouts/{}.json", layout_name);
    fs::write(path, layout_data).expect("Unable to save layout");
}
```

**Step 4.2: Load Layouts**

- Implement a Rust function to load layout data and send it back to the frontend:

```rust
use std::fs;

fn load_layout(layout_name: &str) -> String {
    let path = format!("./layouts/{}.json", layout_name);
    fs::read_to_string(path).expect("Unable to load layout")
}
```

Integrate these functions with the Svelte frontend to enable saving and loading layouts through the Global Menu.

**Step 5.1: Frontend Testing**

- Test the interface in various scenarios to ensure panels can be resized, moved, and managed as expected.
- Use SvelteKit’s built-in testing utilities for unit and integration testing.

**Step 5.2: Backend Testing**

- Write Rust unit tests for functions managing layout persistence and application embedding.
- Test Tauri’s integration with different OS environments to ensure cross-platform compatibility.

**Step 5.3: User Testing**

- Conduct user testing sessions to gather feedback on usability, especially focusing on panel management and layout saving/loading.

**Step 6.1: Build the Application**

- Use Tauri to bundle the application for different operating systems:

```bash
npm run tauri build
```

**Step 6.2: Distribute eurAIka**

- Prepare the application for distribution by creating installers or portable packages for Windows, macOS, and Linux.
- Set up a versioning system and update mechanism within the Tauri configuration.

## Milestones

These milestones provide a roadmap for developing the eurAIka application, guiding the project from initial setup through to deployment. Each milestone marks a significant phase in the development process, helping to keep the project on track and focused on delivering a high-quality product.

The milestones for the eurAIka project are broken down into phases that align with the key stages of development outlined in the Implementation section. Each milestone includes specific deliverables and a rough timeline for completion.

**Milestone 1: Project Initialization and Environment Setup**

**Timeline:** Week 1-2

**Deliverables:**

- Svelte/SvelteKit project initialized with a well-structured directory layout.
- Tauri integrated into the project.
- Rust backend set up with basic system management capabilities.
- Initial commit to the version control system (e.g., GitHub).

**Objectives:**

- Establish a solid foundation for the project, ensuring all necessary tools and environments are configured correctly.
- Confirm that the SvelteKit frontend can communicate with the Tauri backend.

**Milestone 2: Core Frontend Development**

**Timeline:** Week 3-4

**Deliverables:**

- Main Interface (`MainLayout.svelte`) implemented with basic grid or flex layout.
- Panel Manager developed with resizing, movement, and visibility toggle functionalities.
- Initial implementation of the Global Menu with basic options available.

**Objectives:**

- Create the core user interface elements and ensure that the panels can be managed dynamically.
- Establish the basic structure of the application, allowing for further integration of more complex features.

**Milestone 3: Integration of External Applications**

**Timeline:** Week 5-6

**Deliverables:**

- Tauri commands for launching and embedding external applications (e.g., VSCodium, JupyterLab, web browsers).
- Functional integration of at least two panels (e.g., Whiteboard with Milkdown and Librarian with a web browser).
- Interaction between SvelteKit frontend and embedded applications, allowing users to control these apps within eurAIka.

**Objectives:**

- Successfully embed and interact with external applications, ensuring they work seamlessly within the eurAIka interface.
- Ensure the applications are sandboxed and securely managed by the Tauri backend.

**Milestone 4: Layout Management and Persistence**

**Timeline:** Week 7-8

**Deliverables:**

- Layout Manager component fully functional with the ability to save and load layouts.
- Backend logic for storing and retrieving layout configurations implemented in Rust.
- User interface in the Global Menu for managing layouts.

**Objectives:**

- Enable users to save their preferred panel configurations and restore them later.
- Test the reliability and performance of the layout persistence system.

**Milestone 5: Final Frontend Features and Refinements**

**Timeline:** Week 9-10

**Deliverables:**

- Full-screen mode for panels implemented and tested.
- Theming options for panels and the global interface.
- Polished Global Menu with all intended features implemented.

**Objectives:**

- Complete the user interface by adding final features and ensuring a smooth user experience.
- Refine the application based on initial user feedback and internal testing.

**Milestone 6: Testing and Debugging**

**Timeline:** Week 11-12

**Deliverables:**

- Comprehensive unit and integration tests for both the frontend (SvelteKit) and backend (Rust/Tauri).
- Bug fixes and performance optimizations based on testing results.
- User testing sessions conducted, with feedback incorporated into the application.

**Objectives:**

- Ensure the application is stable, performant, and free of critical bugs.
- Validate that all features work as intended and meet user needs.

**Milestone 7: Deployment and Release**

**Timeline:** Week 13-14

**Deliverables:**

- Build and packaging of eurAIka for Windows, macOS, and Linux.
- Creation of installation packages (e.g., .exe, .dmg, .deb).
- Release of version 1.0.0 on the selected distribution platforms (e.g., GitHub Releases).

**Objectives:**

- Successfully deploy the eurAIka application, making it available to users on all major desktop platforms.
- Ensure the deployment process is smooth, with clear documentation provided for installation and usage.

## Gathering Results

This section will outline how to evaluate the success of the eurAIka application post-deployment, focusing on user feedback, performance metrics, and the overall fulfillment of the requirements.

After the deployment of eurAIka, it is essential to evaluate the effectiveness and success of the application. The following steps outline how to gather and assess results to ensure that eurAIka meets the defined requirements and provides a satisfactory user experience.

**1. User Feedback and Adoption**

**Objective:** Measure user satisfaction and adoption rates to determine how well eurAIka meets user needs.

**Actions:**

- **Collect User Feedback:** Set up channels for users to provide feedback, such as surveys, user forums, or integrated feedback forms within the application.
  - Focus on usability, panel management, integration with external applications, and layout saving/loading features.
  - Encourage users to report any issues or suggest improvements.
- **Analyze Feedback:** Regularly review user feedback to identify common themes, issues, and requests for enhancements.
  - Prioritize feedback based on frequency and impact to guide future updates or bug fixes.
- **Monitor Adoption Metrics:** Track the number of downloads, active users, and session lengths to gauge how widely and effectively the application is being used.

**2. Performance Monitoring**

**Objective:** Ensure that eurAIka performs well across different systems and that all features function smoothly.

**Actions:**

- **Monitor Application Performance:** Use tools like `tauri-plugin-log` to log performance metrics (e.g., memory usage, CPU load) and detect potential bottlenecks.
  - Pay attention to the performance of embedded applications and the responsiveness of the panel management system.
- **Track Error Reports:** Collect and analyze error reports to identify and fix crashes or bugs that may degrade the user experience.
  - Implement a system for automatic error reporting with user consent, allowing for quicker detection and resolution of issues.
- **Test Across Platforms:** Continuously test eurAIka on different operating systems (Windows, macOS, Linux) to ensure consistent performance and functionality.

**3. Fulfillment of Requirements**

**Objective:** Confirm that the eurAIka application meets all the specified requirements as outlined in the initial design document.

**Actions:**

- **Requirement Verification:** Review the final product against the **Requirements** section to ensure all "Must Have" and "Should Have" features are fully implemented.
  - Verify that each panel (Whiteboard, Librarian, Coder, Other) functions as intended, and that layout management works flawlessly.
  - Check that the global menu provides the necessary options and that panel management features (resizing, moving, full-screen mode) are intuitive and reliable.
- **User Satisfaction Correlation:** Compare user feedback with the initially defined requirements to see if the application meets or exceeds user expectations.
  - Identify any gaps between the delivered features and user expectations that might require future updates.
- **Iteration Plan:** Based on feedback and performance data, create a plan for iterative improvements and feature expansions in subsequent versions of eurAIka.

**4. Continuous Improvement**

**Objective:** Establish a process for continuous improvement of eurAIka based on ongoing user feedback and technological advancements.

**Actions:**

- **Scheduled Updates:** Plan regular updates to address bug fixes, performance enhancements, and new feature requests.
  - Implement a versioning system to manage and communicate changes clearly to users.
- **Community Engagement:** Foster a community around eurAIka, encouraging users to share tips, create tutorials, and contribute to the application’s improvement.
  - Consider open-sourcing certain components to encourage community contributions.
- **Future Development Roadmap:** Based on the gathered results, develop a roadmap for future versions of eurAIka, prioritizing the most requested features and addressing any identified shortcomings.
