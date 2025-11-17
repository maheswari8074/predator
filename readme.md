# Predator: Multimodal AI for Advanced UAV Control

Predator is a framework for controlling a simulated UAV using multimodal AI. It translates natural language commands and visual queries into flight actions within a high-fidelity PX4/Gazebo environment, leveraging both local (LLaVA) and cloud-based (Gemini) MLLMs.

**Quick Links**

- [Python Downloads](https://www.python.org/downloads/)
- [Flask Documentation](https://flask.palletsprojects.com/)

### Core Features

  - **Natural Language Flight Control**: Execute complex flight maneuvers with simple text commands.
  - **Visual Question Answering (VQA)**: Interrogate the drone's video feed to gain environmental awareness.
  - **Dual-Model Architecture**: Utilizes Ollama/LLaVA for low-latency tasks and the Gemini 1.5 Flash API for advanced reasoning.
  - **Realistic Simulation**: Integrated with the PX4 Autopilot (SITL) and Gazebo for accurate physics and sensor data.

### Technology Stack

  - **Simulation**: PX4, Gazebo, QGroundControl
  - **AI Models**: LLaVA, Gemini 1.5 Flash
  - **Backend**: Python, Flask
  - **Frontend**: HTML, Tailwind CSS
  - **Orchestration**: Ollama

-----

## Installation Guide

A Linux environment (or WSL 2 on Windows) is strongly recommended.

#### 1\. System Prerequisites

First, install the core simulation and AI dependencies on your system.

  - **PX4/Gazebo Toolchain**: Follow the official **[PX4 Gazebo Simulation guide](https://docs.px4.io/main/en/sim_gazebo_gz/)** for a complete setup.

  - **Gazebo Python Bindings**: This project requires a specific transport library.

    ```bash
    sudo apt update && sudo apt install python3-gz-transport13
    ```

  - **Ollama**: Install the Ollama server from the [official website](https://ollama.com/) and ensure the service is running.

    ```bash
    sudo systemctl enable --now ollama
    ```

#### 2\. Project Setup

Clone the repository and configure the environment.

  - **Clone and Enter Directory**
    ```bash
    git clone https://github.com/RAWx18
    cd Predator
    ```
  - **Create and Activate Virtual Environment**
    > The `--system-site-packages` flag is essential to access the Gazebo library installed in the previous step.
    ```bash
    python3 -m venv --system-site-packages venv
    source ./venv/bin/activate
    ```
  - **Install Dependencies**
    ```bash
    pip install -r requirements.txt
    ```

#### 3\. Configuration

Configure the AI models and environment variables.

  - **Set Environment Variables**: Add your API key and the machine's IP address to your shell profile (`~/.bashrc` or `~/.profile`).

    ```bash
    export API_KEY="<YOUR_GEMINI_API_KEY>"
    export RPi_IP="127.0.0.1" # Use localhost or the IP of your machine
    ```

    Then, reload your profile with `source ~/.profile`.

  - **Create Custom LLaVA Model**: Use the provided Modelfile to create a specialized Ollama model.

    ```bash
    ollama pull LLaVA
    ollama create Predator -f ./DroneAI/models/Modelfile
    ```

-----

## Execution

1.  **Launch the Simulator**: Navigate to your PX4-Autopilot directory and run the SITL command.

    ```bash
    make px4_sitl gz_x500_mono_cam_lawn
    ```

2.  **Run the Web Application**: In the project directory (with the venv activated), start the Flask server.

    ```bash
    python web.py
    ```

3.  **Open the Interface**: Access the web UI in your browser at `http://127.0.0.1:5000`.

Optionally, run **QGroundControl** to monitor vehicle status.

-----

## Development

  - **CSS Generation**: To watch for style changes and recompile the CSS, run:
    ```bash
    npx tailwindcss -i ./static/src/input.css -o ./static/dist/css/output.css --watch
    ```
  - **Debug Ollama**: To view real-time logs from the model server, stop the service and run it directly in your terminal:
    ```bash
    sudo systemctl stop ollama
    ollama serve
    ```

## License

This project is licensed under the MIT License. See the `LICENSE` file for details
