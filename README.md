# Tyra Documentation

Tyra Documentation

Introduction to Tyra

In this tutorial, you will learn how to get started with Tyra. We will cover:

- Setting up a basic Tyra project.
- Using logging and debugging functions.

### Project Setup

To begin working with Tyra, follow these steps:

1. Include Tyra headers in your project.
   ```cpp
   #include <tyra>
   #include "tutorial_01.hpp"
   ```

2. Initialize an instance of the `Engine` class:
   ```cpp
   Tyra::Engine engine;
   ```

3. Create an instance of your game class (e.g., `Tutorial01`) and pass the `Engine` instance to it:
   ```cpp
   Tyra::Tutorial01 game(&engine);
   ```

4. Run the game by passing the instance of your game class to the `Engine`:
   ```cpp
   engine.run(&game);
   ```

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

- Load an image (texture) and create a sprite from it.
- Clear the screen and render sprites.

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

- Render 3D objects.
- Use a sprite texture on 3D models.

### Initialization

To start rendering 3D objects, follow these steps:

1. Initialize an instance of the `Engine` class and your game class (e.g., `Tutorial03`).
2. Define 3D camera information, such as position and look direction.
3. Load the texture for the sprite atlas.

```cpp
int main() {
  Tyra::Engine engine;
  Tyra::Tutorial03 game(&engine);
  engine.run(&game);
  return 0;
}
```

```cpp
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

```cpp
void Tutorial03::loadTexture() {
  auto filepath = FileUtils::fromCwd("bricks.png");
  textureAtlas = textureRepository.add(filepath);
}
```

### Rendering 3D Objects

To render 3D objects, follow these steps:

1. Load a 3D model and add it to the scene.

```cpp
void Tutorial03::loadMesh() {
  auto modelPath = FileUtils::fromCwd("box.fbx");
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
init(): Initializes resources and configures the environment.

update(): Implements the game's update logic.

render(): Renders objects on the screen.

exit(): Releases resources and properly terminates the game.

Log and Debugging Functions:

TYRA_LOG(...): Logs messages.

TYRA_WARN(...): Logs warning messages.

TYRA_ERROR(...): Logs error messages.

TYRA_ASSERT(condition, ...): Checks conditions and logs error messages.

TYRA_BREAKPOINT(): Freezes program execution for debugging purposes.

Important Functions:

loadTexture(): Loads a texture from the "tyra.png" file and adds it to the texture repository.

configureSprite(): Configures a sprite to display the loaded texture.

render(): Renders the sprite on the screen.

Important Functions and Classes:

loadTexture() Loads the texture from the "bricks.png" file and adds it to the texture repository.

configureMaterial() Configures the material of a 3D model with the loaded texture.

loadMesh() Loads a 3D model (e.g., "box.obj") and adds it to the scene.

setupCamera() Configures the 3D camera.

setupLights() Configures the scene's lights.

render() Renders the 3D scene on the screen.

This is a basic guide to start rendering 3D objects with Tyra.
