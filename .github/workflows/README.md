# Build Configuration

This directory contains GitHub Actions workflows for building the game.

## Build Game Workflow

The `build-game.yml` workflow automatically builds the game for Linux and Windows platforms.

### Features

- **Multi-platform builds**: Automatically builds for both Linux and Windows
- **Dependency management**: Downloads and installs SFML library
- **Artifact creation**: Saves compiled executables and assets
- **Easy distribution**: Each build includes run scripts for easy execution

### Workflow Triggers

The workflow runs on:
- Push to `main`, `feature/**`, or `copilot/**` branches
- Pull requests to `main` or `feature/**` branches
- Manual trigger via workflow_dispatch

### Build Process

#### Linux Build
1. Installs SFML and development dependencies via apt
2. Configures CMake project
3. Compiles the game
4. Packages executable with assets and run script
5. Uploads as artifact "MyGame-Linux"

#### Windows Build
1. Downloads SFML 2.6.1 for Windows
2. Configures CMake with Visual Studio 2022
3. Compiles the game
4. Copies SFML DLLs and assets
5. Creates run.bat script
6. Uploads as artifact "MyGame-Windows"

### Download Artifacts

After a successful build:
1. Go to the Actions tab in GitHub
2. Click on the latest "Build Game" workflow run
3. Download artifacts:
   - `MyGame-Linux` - Linux executable
   - `MyGame-Windows` - Windows executable with DLLs

### Running the Game

**Linux:**
```bash
chmod +x run.sh
./run.sh
```

**Windows:**
```cmd
run.bat
```

### Dependencies

The game requires:
- **SFML 2.6+** (System, Window, Graphics, Audio modules)
- **C++17** compatible compiler
- **CMake 3.16+**

### Local Build

To build locally:

```bash
mkdir build
cd build
cmake .. -DCMAKE_BUILD_TYPE=Release
cmake --build . --config Release
```

## CMakeLists.txt

The project uses CMake for cross-platform building. Key features:
- Auto-detection of platform (Linux/Windows)
- Automatic copying of assets
- Windows: Automatic DLL copying
- Support for both Debug and Release configurations
