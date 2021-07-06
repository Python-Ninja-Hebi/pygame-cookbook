# Pygame Cookbook - Recipes for Mastering Pygame

Hi,

in the last years I used **Python** and the module **Pygame** to create some games. I found many information about **Pygame** on many different places in the web. I would prefer one book where I can look for solutions. So I started to write this `Pygame Cookbook`.  

**Pygame** (https://www.pygame.org) is a create library for making your own games with python. **Pygame** uses the **SDL** library.

This `Pygame Cookbook` tries to explain the building blocks of a game simple and in detail.

The official documentation of **Pygame** is available at https://www.pygame.org/docs/.

If you like `Pygame Cookbook`, use it. If you have some suggestions, tell me (hebi@python-ninja.com).

All game assets that I use in recipes are from https://www.kenney.nl. Thank you.

Have fun.

Hebi, on the way to Python Ninja

`Edition 0.1, July 2021`

## install pygame

Prerequisite for using the `Pygame Cookbook` is that the Python library **Pygame** is installed on your computer.
$ pip install pygame
More about installing pygame https://www.pygame.org/wiki/GettingStarted#Pygame%20Installation

You can use **Pygame** with different programming environments. A very convenient way to try the recipes of this Cookbook is to copy the **Jupyter Notebook** version (file *pygame-cookbook.ipynb*) to your computer and evaluate the cells your interested in.

## pygame and jupyter notebooks

#### Task: Using pygame with Jupyter Notebooks

**Solution:**

Open a window that shows the drawing of pygame.  

You can 


```python
%gui qt
import pygame
 
pygame.init() #start the pygame module

screen = pygame.display.set_mode((400, 300)) #get access to the display

pygame.display.update() #update the display
```

    pygame 2.0.1 (SDL 2.0.14, Python 3.8.8)
    Hello from the pygame community. https://www.pygame.org/contribute.html


Now you can use another jupyter cell to draw a rectangle.


```python
pygame.draw.rect(screen, (0, 128, 255), pygame.Rect(30, 30, 60, 60)) #draw a rectangle

pygame.display.update() #update display
```

At least close the pygame window.


```python
pygame.quit() #stop the pygame module
```

<img src="img/jupyter_pygame.png" width="280" align="left">

**Explanation:**

With the magic command`%gui qt` **Jupyter** can work together with **Pygame**. This enables the use of the Jupyter GUI and the entries in the Pygame window at the same time. Therefore every recipe in this `Pygame Cookbook` has the magic command`%gui qt` in the first line.

You don't need that, if you want to try a recipe in a different Python environment.

**More:**

https://ipython.readthedocs.io/en/stable/config/eventloops.html

## simple gameloop

#### Task: simple program structure that works with every game

**Solution:**

Opens a window that shows the drawing of pygame.


```python
%gui qt
import pygame

# ---- Initialize ----

pygame.init()

SIZE = WIDTH, HEIGHT = 320, 240
BLACK = 0, 0, 0
BLUE = 0, 0, 255

running = True
screen = pygame.display.set_mode(SIZE)

# ---- Game loop ----

while running:
    
    # ---- input ----
    for event in pygame.event.get():
        if event.type == pygame.QUIT: 
            running = False
    
    # ---- update ---- 
    
    # ---- draw ----
    screen.fill(BLACK)
    pygame.draw.rect(screen, BLUE, pygame.Rect(30, 30, 60, 60)) #draw a rectangle
    pygame.display.flip()

# ---- Quit ----

pygame.quit()
```

    pygame 2.0.1 (SDL 2.0.14, Python 3.8.8)
    Hello from the pygame community. https://www.pygame.org/contribute.html


<img src="img/simple_gameloop.png" width="280" align="left">

**Explanation:**

Every game consists of the same building blocks:
    
* **Input**: Read input from player. Which keys are pressed? Did the  mouse move? Any other input device lika a joystick?
* **Update**: Do the changes in the game world
* **Draw**: Show the changes on the screen  
    
* and start all over again

This is called **game loop** or **event loop**.

`import pygame` .. import the Pygame module  
`init() -> (numpass, numfail)` .. initialize all imported Pygame modules. Some Pygame modules needs to be initialized. Return value *numfail* shows how many modules could not be initialized by Pygame.

`pygame.display.set_mode()` .. return a *Surface* object on which python can draw. The first parameters define the size. The created display will be the best supported by the system. 

There are many additional flags:

`pygame.FULLSCREEN` .. fullscreen no window  
`pygame.HWSURFACE` .. hardware accelerated (only fullscreen). 
`pygame.OPENGL` .. create an OpenGL-renderable display  
`pygame.RESIZABLE` .. resizable window  
`pygame.NOFRAME` .. window without border or controls  

`pygame.event.get(eventtype=None) -> Eventlist` .. get next event from Pygame.   
`if event.type == pygame.QUIT: ` .. user clicked on the close control of the window.

`screen.fill(BLACK)` .. fill the complete display with color   
`pygame.draw.rect` .. draw a rectangle on the display

`pygame.display.flip()` .. show changed display on the screen

`pygame.quit() -> None` .. uninitialize all imported Pygame modules

This **loop** will be used in the most recipes.

**more**  
* Pygame documentaion https://www.pygame.org/docs/

## gameloop with timing

#### Task: integrating time into the game loop

**Solution:**

using class **pygame.time.clock** to get a good timing


```python
%gui qt
import pygame

# ---- Initialize ----

pygame.init()

SIZE = WIDTH, HEIGHT = 320, 240
BLACK = 0, 0, 0
BLUE = 0, 0, 255

running = True
screen = pygame.display.set_mode(SIZE)

clock = pygame.time.Clock() #create clock object

FRAMES_PER_SECOND = 30      #who many pictures per second should pygame generate?

x_position = 60             #position of the blue rectangel
PIXELS_PER_SECOND = 40      #how many pixels per second should the rectangele be moved?

# ---- Game loop ----

while running:
    
    # ---- input ----
    for event in pygame.event.get():
        if event.type == pygame.QUIT: 
            running = False
    
    # ---- update ---- 
    delta_time = clock.tick(FRAMES_PER_SECOND)     # time since last frame
    
    x_position = x_position + delta_time/1000 * PIXELS_PER_SECOND #next position of the rectanel
 
    if x_position > WIDTH: #rectangele vanishes right start from left again
        x_position = 0
    
    # ---- draw ----
    screen.fill(BLACK)
    pygame.draw.rect(screen, BLUE, pygame.Rect(int(x_position), 30, 60, 60)) #draw a rectangle
    pygame.display.flip()

# ---- Quit ----

pygame.quit()
```

**Explanation:**

**Pygame** comes with an integrated class for timing: **pygame.time.Clock**

**-Initialize-**

First you have to create your own **clock** object.  

`clock = pygame.time.Clock()`

`FRAMES_PER_SECOND = 30` .. you have to define how many frames (pictures) should be maximal drawn by **Pygame** in a second

`x_position = 60` .. the blue rectangle will move from left to right. In every frame the script will use the time since the last frame to calculate the new position.  

`PIXELS_PER_SECOND = 40` .. the speed (velocity) of the rectangle will be 40 pixels in one second.

**-Game loop update()-**

`delta_time = clock.tick(FRAMES_PER_SECOND)` .. returns the milliseconds since the last frame.   
The parameter *FRAMES_PER_SECOND* defines the maximal number of frames per second. It limits the runtime. **Pygame** will not draw more frames per second.

`x_position = x_position + delta_time/1000 * PIXELS_PER_SECOND` .. to get the distance covered since the last frame you have to multiply the elapsed time by the velocity. *delta_time* is in milliseconds but you need the time in seconds. So you have to divide with thousand.
 
`if x_position > WIDTH: x_position = 0`.. before the rectangel is vanishing from the screen you have to start from the left side again


```python

```

## draw


```python

```

## rectangle


```python

```

## image

## text

## transparent

## mouse

## keyboard

## events

## collision


```python

```
