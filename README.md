# Dylan's DonkeyCar Project

![DonkeyCar Logo](https://docs.donkeycar.com/assets/images/logo_white.png)

This repository contains the configuration and code for my custom DonkeyCar autonomous vehicle project. Built on the open-source [Donkey Car](https://www.donkeycar.com/) platform, this project implements a self-driving RC car using computer vision and machine learning.

## 🚗 Project Overview

This project transforms a standard RC car into an autonomous vehicle capable of:
- Self-driving using computer vision and machine learning
- Manual control via web interface or game controller
- Data collection for training custom driving models
- Real-time inference with TensorFlow/Keras models

## 🛠️ Hardware Configuration

### Core Components
- **Controller**: Raspberry Pi 4 (recommended) or similar single-board computer
- **Motor Controller**: Custom PWM-based motor control
- **Camera**: Standard Raspberry Pi Camera Module (compatible with CVCAM)
- **Chassis**: Custom RC car platform

### Pin Configuration
- **Drive Train**: PWM-based two-wheel differential drive
- **Motor Channels**:
  - Right Motor: 
    - Speed: Channel 8
    - Forward: Channel 9
    - Backward: Channel 10
  - Left Motor:
    - Speed: Channel 13
    - Forward: Channel 11
    - Backward: Channel 12

## 🚀 Getting Started

### Prerequisites
- Python 3.6+
- DonkeyCar framework installed
- Raspberry Pi OS (or compatible Linux distribution)
- Required Python packages (see requirements.txt)

### Installation

1. **Clone this repository**:
   ```bash
   git clone https://github.com/yourusername/dylanDonkeyCar.git
   cd dylanDonkeyCar
   ```

2. **Install DonkeyCar and dependencies**:
   ```bash
   pip install -e .
   pip install -r requirements.txt
   ```

3. **Configure your vehicle**:
   - Copy `config.py` to `myconfig.py` and modify as needed
   - Update motor and camera settings in `myconfig.py`

## 🎮 Usage

### Starting the Car
```bash
# Start in user (manual) mode
python manage.py drive

# Start with web controller (default port 8887)
python manage.py drive --js

# Start with specific model
python manage.py drive --model models/mypilot.h5
```

### Training a Model
1. **Collect data**:
   ```bash
   python manage.py drive --js
   ```
   - Press `R` to start/stop recording
   - Drive the car manually to collect training data

2. **Train the model**:
   ```bash
   python train.py --model models/mypilot.h5 --tub data/tub_* --type linear
   ```

### Calibration
Calibrate your car's steering and throttle:
```bash
python calibrate.py
```

## 📁 Project Structure

```
dylanDonkeyCar/
├── data/                 # Training data (tub files)
├── models/               # Trained models
├── calibrate.py          # Calibration utility
├── config.py             # Default configuration
├── myconfig.py          # Your custom configuration
├── manage.py            # Main control script
└── train.py             # Training script
```

## ⚙️ Configuration

Key configuration parameters (in `myconfig.py`):
- `DRIVE_TRAIN_TYPE = "PWM_TWO_WHEEL"` - Differential drive configuration
- `CAMERA_TYPE = "CVCAM"` - Camera interface
- `DEFAULT_AI_FRAMEWORK = 'tensorflow'` - AI framework
- `DEFAULT_MODEL_TYPE = 'linear'` - Default model architecture

## 🤖 Autonomous Driving

The car uses a neural network to process camera input and predict steering angles and throttle values. The model is trained using behavioral cloning from human driving data.

## 📶 Web Interface

Access the web controller at `http://<your-pi-ip>:8887` to:
- View the camera feed
- Control the car manually
- Start/stop recording data
- Enable autonomous mode

## 🖥️ Development Workflow (WSL to Raspberry Pi)

This project supports a development workflow where you can edit code in WSL (Windows Subsystem for Linux) and deploy to a Raspberry Pi for execution.

### Prerequisites
- Windows 10/11 with WSL2 installed
- Ubuntu 20.04/22.04 on WSL2
- Raspberry Pi with SSH access
- Shared folder or Git repository for code sync

### Setting Up Development Environment

1. **Install WSL and Ubuntu**
   ```bash
   # In PowerShell as Administrator
   wsl --install -d Ubuntu
   ```

2. **Clone the repository in WSL**
   ```bash
   # In WSL Ubuntu terminal
   git clone https://github.com/yourusername/dylanDonkeyCar.git
   cd dylanDonkeyCar
   ```

3. **Set up Python environment**
   ```bash
   # Create and activate virtual environment
   python -m venv venv
   source venv/bin/activate
   
   # Install dependencies
   pip install -e .
   pip install -r requirements.txt
   ```

### Deployment to Raspberry Pi

1. **Sync code changes**
   Option 1: Using Git (recommended)
   ```bash
   # On WSL after making changes
   git add .
   git commit -m "Your changes"
   git push
   
   # On Raspberry Pi
   git pull
   ```

   Option 2: Using rsync (for direct file transfer)
   ```bash
   # From WSL to Pi
   rsync -avz --exclude='venv' --exclude='.git' --exclude='data' --exclude='models' ./ pi@raspberrypi.local:~/dylanDonkeyCar/
   ```

2. **Install dependencies on Pi**
   ```bash
   # SSH into your Pi
   ssh pi@raspberrypi.local
   
   # Navigate to project directory
   cd ~/dylanDonkeyCar
   
   # Set up Python environment (if not done already)
   python -m venv venv
   source venv/bin/activate
   pip install -e .
   pip install -r requirements.txt
   ```

3. **Run the car**
   ```bash
   # On Raspberry Pi
   source venv/bin/activate
   python manage.py drive
   ```

### Development Tips

- **Testing Locally**: You can test basic functionality in WSL, but hardware-specific features require the Pi
- **VS Code Integration**: Use the Remote - WSL extension for seamless development
- **File Watching**: If using rsync, consider using `entr` to automatically sync on file changes:
  ```bash
  # Install entr
  sudo apt install entr
  
  # Watch for changes and sync
  ls -d * | entr -r rsync -avz --exclude='venv' --exclude='.git' --exclude='data' --exclude='models' ./ pi@raspberrypi.local:~/dylanDonkeyCar/
  ```

## 📚 Resources

- [DonkeyCar Documentation](https://docs.donkeycar.com/)
- [DonkeyCar GitHub](https://github.com/autorope/donkeycar)
- [DonkeyCar Community](https://donkeycar.com/community/)

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

## 🙏 Acknowledgments

- The DonkeyCar team and community for their amazing open-source platform
- All contributors who have helped improve the project

---

*Last updated: August 2023*

[![DonkeyCar](https://img.shields.io/badge/DonkeyCar-Open%20Source-brightgreen)](https://www.donkeycar.com/)
