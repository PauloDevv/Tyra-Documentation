# Tyra Documentation

## Introduction to Tyra

In this tutorial, you will learn how to get started with Tyra. We will cover:

### Project Setup
To begin working with Tyra, follow these steps:

1. Include Tyra headers in your project:

```cpp
#include <tyra>
#include "tutorial_01.hpp"
```

2. Initialize an instance of the Engine class:

```cpp
Tyra::Engine engine;
```

3. Create an instance of your game class (e.g., Tutorial01) and pass the Engine instance to it:

```cpp
Tyra::Tutorial01 game(&engine);
```

4. Run the game by passing the instance of your game class to the Engine:

```cpp
engine.run(&game);
```

## Camera Class

The Camera class is responsible for controlling the camera in your 3D game. It includes the following constructors and methods:

### Constructors

- `Camera(const RendererSettings& t_screen)`: Constructor for the Camera class that takes renderer settings as input.

- `~Camera()`: Destructor for the Camera class.

### Methods

- `void update()`: Updates the camera's position based on polar coordinates (yaw, pitch) and the camera type (first person, third person, inverted).

- `void setPositionByMesh(Mesh* t_mesh)`: Sets the camera's position based on a Mesh object. The position varies depending on the camera type.

- `void setLookDirectionByPad(Pad* t_pad, const float deltatime)`: Sets the camera's look direction based on control inputs (Pad) and elapsed time (deltatime).

- `void reset()`: Resets the yaw and pitch angles of the camera to their initial values.

- `void setFirstPerson()`: Sets the camera type to first person.

- `void setThirdPerson()`: Sets the camera type to third person.

- `void setThirdPersonInverted()`: Sets the camera type to inverted third person.

- `void calculatePitch(Pad* t_pad, const float deltatime)`: Calculates the camera's pitch angle based on control inputs (Pad) and elapsed time (deltatime).

- `void calculateYaw(Pad* t_pad, const float deltatime)`: Calculates the camera's yaw angle based on control inputs (Pad) and elapsed time (deltatime).

- `float calculateHorizontalDistance()`: Calculates the horizontal distance of the camera from the observed object based on the pitch angle.

- `float calculateVerticalDistance()`: Calculates the vertical distance of the camera from the observed object based on the pitch angle.

- `void calculateCameraPosition(Mesh* t_mesh, const float horizontalDistance, const float verticalDistance)`: Calculates the camera's position based on a Mesh object, the provided horizontal and vertical distances.

## Example Class

The Example class is your game class that interacts with the Tyra engine. It includes the following constructors and methods:

### Constructors

- `Example(Engine* t_engine)`: Constructor for the Example class that takes an instance of the Tyra engine.

- `~Example()`: Destructor for the Example class.

### Methods

- `void init()`: Initializes the game by loading settings, checking necessary directories, and configuring renderer settings.

- `void loop()`: Manages the game's main loop, updating the game state based on elapsed time.

- `void loadSavedSettings()`: Loads saved game settings if they exist.

- `void checkNeededDirectories()`: Checks if the necessary directories for the game exist, including the save directory.

- `void checkSavesDir()`: Checks the existence of the save directory and creates it if it doesn't exist.

## StateLoadingGame Class

The StateLoadingGame class represents the game's loading state. It controls the loading of entities, item repositories, the user interface, and the game world. It includes methods for initialization and updating the loading process.

## StateSplashScreen Class

The StateSplashScreen class represents the game's splash screen state. It manages the display of splash screens and transitions to the next game state. It includes methods for initialization, updating, and rendering the splash screens.

### Logging and Debugging Functions
Tyra provides logging and debugging functions to assist in your game development. Here are some of the available functions:

- `TYRA_LOG(...)`: Use this function to print log messages.
- `TYRA_WARN(...)`: Print warning messages.
- `TYRA_ERROR(...)`: Log error messages.
- `TYRA_ASSERT(condition, ...)`: Check a condition and log an error message if the condition is false.
- `TYRA_BREAKPOINT()`: Freeze the program's execution at the current location.

Example of using these functions:

```cpp
void Tutorial01::init() {
  short positiveNumber = 1;

  TYRA_LOG("Hello world!");

  TYRA_WARN("Something weird happened! ", "Number should be positive, but is: ", -1);

  TYRA_ERROR("Something really bad happened!");

  TYRA_ASSERT(positiveNumber > 0, "Number should be positive!", "Please check the code!", "Current value:", (int)positiveNumber);
}
```

Make sure to use these functions to facilitate debugging and game development.

## Tutorial 02: Loading Textures and Rendering Sprites

In this tutorial, you will learn how to:

### Loading Textures and Creating Sprites
To load a texture and create a sprite from it, follow these steps:

1. Set up a texture in the "res" directory and load it:

```cpp
auto filepath = FileUtils::fromCwd("tyra.png");
auto* texture = textureRepository.add(filepath);
```

2. Create a sprite and set its mode, size, and position:

```cpp
sprite.mode = SpriteMode::MODE_STRETCH;
sprite.size = Vec2(256.0F, 128.0F);
sprite.position = Vec2(screenSettings.getWidth() / 2.0F - sprite.size.x / 2.0F, screenSettings.getHeight() / 2.0F - sprite.size.y / 2.0F);
```

3. Add the texture to the sprite:

```cpp
texture->addLink(sprite.id);
```

4. Render the sprite in the game's main loop:

```cpp
renderer.beginFrame();
renderer.renderer2D.render(sprite);
renderer.endFrame();
```

## Tutorial 03: 3D Object Rendering

In this tutorial, you will learn how to:

### Initialization
To start rendering 3D objects, follow these steps:

1. Initialize an instance of the Engine class and your game class (e.g., Tutorial03).

```cpp
int main() {
  Tyra::Engine engine;
  Tyra::Tutorial03 game(&engine);
  engine.run(&game);
  return 0;
}

Tutorial03::Tutorial03(Engine* t_engine) : engine(t_engine) {
  textureAtlas = nullptr;
}

void Tutorial03::init() {
  loadTexture();
  cameraPosition = Vec4(0.0F, 0.0F, -10.0F, 1.0F);
  cameraLookDirection = Vec4(0.0F, 0.0F, 1.0F, 0.0F);
  setupCamera();
 

 setupLights();
}
```

### Rendering 3D Objects
To render 3D objects, follow these steps:

1. Load a 3D model and add it to the scene.

```cpp
void Tutorial03::loadMesh() {
  auto modelPath = FileUtils::fromCwd("box.obj");
  auto mesh = meshRepository.add(modelPath);
  scene.link(mesh->id);
}
```

2. Configure the material for the model.

```cpp
void Tutorial03::configureMaterial() {
  auto& material = mesh->getMaterial();
  material->setDiffuseTextureAtlas(textureAtlas);
  material->setDiffuseUvMin({0.0F, 0.0F});
  material->setDiffuseUvMax({1.0F, 1.0F});
}
```

3. Render the scene in the game's main loop.

```cpp
void Tutorial03::render() {
  renderer.beginFrame();
  renderer.render3D(scene, camera);
  renderer.endFrame();
}
```

- `init()`: Initializes resources and configures the environment.
- `update()`: Implements the game's update logic.
- `render()`: Renders objects on the screen.
- `exit()`: Releases resources and properly terminates the game.

## Log and Debugging Functions

- `TYRA_LOG(...)`: Logs messages.
- `TYRA_WARN(...)`: Logs warning messages.
- `TYRA_ERROR(...)`: Logs error messages.
- `TYRA_ASSERT(condition, ...)`: Checks conditions and logs error messages.
- `TYRA_BREAKPOINT()`: Freezes program execution for debugging purposes.

## Important Functions

- `loadTexture()`: Loads a texture from the "tyra.png" file and adds it to the texture repository.
- `configureSprite()`: Configures a sprite to display the loaded texture.
- `render()`: Renders the sprite on the screen.

## Important Functions and Classes

- `loadTexture()`: Loads the texture from the "bricks.png" file and adds it to the texture repository.
- `configureMaterial()`: Configures the material of a 3D model with the loaded texture.
- `loadMesh()`: Loads a 3D model (e.g., "box.obj") and adds it to the scene.
- `setupCamera()`: Configures the 3D camera.
- `setupLights()`: Configures the scene's lights.
- `render()`: Renders the 3D scene on the screen.

This is a basic guide to start rendering 3D objects with Tyra. Good luck with your game development!
