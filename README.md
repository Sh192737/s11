### Report: Using `ros1_bridge` to Print Topics from ROS1 to ROS2 on a MacBook with an i3 Processor

This report outlines the process I followed to set up `ros1_bridge` for bridging topics between ROS1 (Noetic) and ROS2 (Foxy) on my MacBook with an i3 processor.

#### Tools and Requirements
- **Operating System**: macOS
- **Homebrew**: Package manager for macOS
- **Docker**: For running ROS1 (Noetic)
- **ROS Noetic** (via Docker)
- **ROS2 Foxy**
- **`ros1_bridge`**: For bridging ROS1 and ROS2

### Part 1: Setting Up ROS1 (Noetic) with Docker

1. **Install Homebrew**:
   To manage packages on macOS, I installed Homebrew by running:
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

2. **Install Docker**:
   I used Homebrew to install Docker:
   ```bash
   brew install --cask docker
   ```

3. **Start Docker**:
   I opened Docker from the Applications folder and ensured it was running.

4. **Pull the ROS Noetic Docker Image**:
   To get the ROS Noetic image, I ran:
   ```bash
   docker pull osrf/ros:noetic-desktop-full
   ```

5. **Run the ROS Noetic Docker Container**:
   I started a new container with the ROS Noetic image:
   ```bash
   docker run -it --rm osrf/ros:noetic-desktop-full
   ```

6. **Install `turtlesim` in ROS1 Docker Container**:
   Inside the Docker container, I updated the package list and installed `turtlesim`:
   ```bash
   apt-get update
   apt-get install ros-noetic-turtlesim
   ```

7. **Start ROS Core and `turtlesim` Node**:
   - I started `roscore` in one terminal:
     ```bash
     roscore
     ```
   - In another terminal, I ran the `turtlesim` node:
     ```bash
     rosrun turtlesim turtlesim_node
     ```

### Part 2: Setting Up ROS2 (Foxy)

1. **Install Homebrew**:
   Since Homebrew was already installed, I proceeded with the next steps. If needed, the installation command is:
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

2. **Install Dependencies**:
   I installed the necessary dependencies using Homebrew:
   ```bash
   brew install python3 cmake tinyxml2 poco
   ```

3. **Install ROS2 Foxy**:
   I added the ROS2 tap and installed ROS2 Foxy:
   ```bash
   brew tap ros2/ros2
   brew install ros-foxy-desktop
   ```

4. **Source the ROS2 Setup Script**:
   To configure the ROS2 environment, I sourced the setup script:
   ```bash
   source /opt/ros/foxy/setup.bash
   ```

5. **Install `ros1_bridge` in ROS2**:
   I installed `ros1_bridge`:
   ```bash
   sudo apt update
   sudo apt install ros-foxy-ros1-bridge
   ```

### Part 3: Bridging ROS1 and ROS2

1. **Source the ROS1 and ROS2 Setup Scripts**:
   - In the Docker container for ROS1:
     ```bash
     source /opt/ros/noetic/setup.bash
     ```
   - On macOS for ROS2:
     ```bash
     source /opt/ros/foxy/setup.bash
     ```

2. **Start `ros1_bridge` Node**:
   I ran the `ros1_bridge` node to enable communication between ROS1 and ROS2:
   ```bash
   ros2 run ros1_bridge dynamic_bridge
   ```

3. **Verify Bridged Topics**:
   - I listed the topics in ROS2:
     ```bash
     ros2 topic list
     ```
   - I checked for topics from ROS1, such as `/turtle1/cmd_vel`.

4. **Print Topics from ROS1 to ROS2**:
   - I viewed the data from ROS1 topics in ROS2:
     ```bash
     ros2 topic echo /turtle1/cmd_vel
     ```

### Conclusion
By following these steps, I successfully set up `ros1_bridge` to enable topic communication between ROS1 and ROS2 on my MacBook with an i3 processor. Using Docker for ROS1 and Homebrew for ROS2 facilitated a smooth setup, allowing me to bridge and view topics from ROS1 in the ROS2 environment.
