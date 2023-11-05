# Game Dev Blog 

## 30/10/23: General Project Overview

### Overveiw

Did some more pygame. worked on some movement systems that my game might use.

### Movement Systems

Currently there are two movement systems that I think will lend themselves well to my game. One is a grid based system where the player moves around tiles, and the other is free movement with a little bit of sliding, because who doesn't like sliding. To try these methods out to see which would be better I made two attempts at things. The first was a grid based maze. it worked by having an array of 1's and 0's which represented wall and free. the plan was that I could expand on this by creating other numbers to signify other tiles later. Here is the code I used:

```
import pygame
import sys

# Define some colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
GREEN = (0, 255, 0)
RED = (255, 0, 0)

# Constants for the grid and maze
WIDTH, HEIGHT = 800, 600
GRID_SIZE = 20
GRID_WIDTH = 10
GRID_HEIGHT = 10
MOVE_DELAY = 0

# Maze layout (0 represents an open path, 1 represents a wall)
maze = [
    [0, 1, 0, 0, 0, 1, 0, 1, 0, 0],
    [0, 1, 0, 1, 0, 1, 0, 1, 0, 1],
    [0, 0, 0, 1, 0, 0, 0, 1, 0, 1],
    [1, 1, 0, 1, 0, 1, 0, 1, 0, 1],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
    [0, 1, 0, 1, 1, 1, 1, 1, 0, 0],
    [0, 1, 0, 0, 0, 0, 0, 0, 1, 0],
    [0, 1, 0, 1, 0, 1, 1, 1, 0, 0],
    [0, 0, 0, 1, 0, 0, 0, 0, 0, 0],
    [1, 1, 0, 1, 1, 0, 1, 1, 1, 0]
]

# Define player position and direction
start_pos = [0,0]
end_pos = [GRID_WIDTH - 1, GRID_HEIGHT - 1]
player_pos = start_pos
new_x = 0
new_y = 0
grid_x = 0
grid_y = 0
player_direction = [0, 0]
player_pos_float = [player_pos[0] * GRID_SIZE, player_pos[1] * GRID_SIZE]

# Initialize Pygame
pygame.init()
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Maze Guy")

# Function to draw the maze as well as the player and end
def draw_maze():
    for x in range(GRID_WIDTH):
        for y in range(GRID_HEIGHT):
            if maze[y][x] == 1:
                pygame.draw.rect(screen, BLACK, (x * GRID_SIZE, y * GRID_SIZE, GRID_SIZE, GRID_SIZE))
            else:
                pygame.draw.rect(screen, WHITE, (x * GRID_SIZE, y * GRID_SIZE, GRID_SIZE, GRID_SIZE))

    pygame.draw.rect(screen, GREEN, (player_pos[0]*GRID_SIZE, player_pos[1]*GRID_SIZE, GRID_SIZE, GRID_SIZE))
    pygame.draw.rect(screen, RED, (end_pos[0] * GRID_SIZE, end_pos[1] * GRID_SIZE, GRID_SIZE, GRID_SIZE))


last_move_time = 0
player_in_motion = False
clock = pygame.time.Clock()
FPS = 120
move_dist = 0.25
# Game loop
running = True
while running:
    current_time = pygame.time.get_ticks()

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
          

    keys = pygame.key.get_pressed()
    

    if not player_in_motion:
        if keys[pygame.K_UP]:
            player_direction = [0, -1]
            player_in_motion = True
        elif keys[pygame.K_DOWN]:
            player_direction = [0, 1]
            player_in_motion = True
        elif keys[pygame.K_LEFT]:
            player_direction = [-1, 0]
            player_in_motion = True
        elif keys[pygame.K_RIGHT]:
            player_direction = [1, 0]
            player_in_motion = True
        
        print(player_direction, player_in_motion)

        if not any(keys):
          player_direction = [0, 0]


  
    if player_in_motion:
        print("motion")
        print(new_x, new_y)

        new_x = player_pos_float[0] + player_direction[0] * move_dist
        new_y = player_pos_float[1] + player_direction[1] * move_dist
        print(new_x, new_y)

        grid_x = int(new_x / GRID_SIZE)
        grid_y = int(new_y / GRID_SIZE)
    
    if 0 <= int(new_x) < GRID_WIDTH and 0 <= int(new_y) < GRID_HEIGHT and maze[int(new_y)][int(new_x)] == 0:
        player_pos_float = [new_x, new_y]
  
  # Check if player should stop moving
    if player_in_motion and not any(keys):
        player_in_motion = False
      
    if grid_x != int(player_pos_float[0] / GRID_SIZE) or grid_y != int(player_pos_float[1] / GRID_SIZE):
        player_pos = [int(player_pos_float[0] / GRID_SIZE), int(player_pos_float[1] / GRID_SIZE)]
        player_in_motion = False


    screen.fill(BLACK)
    draw_maze()
    pygame.display.flip()

    if player_pos == end_pos:
        print("Maze solved!")
        running = False
 
    clock.tick(FPS)

pygame.quit()
sys.exit()
```
Except this code would, every so often, completely restart my computer, so I decided it was haunted and mover away from it.

The other movement system I tried worked significanly better

```
import pygame
import sys
import random

# Initialize Pygame
pygame.init()

# Constants
WIDTH, HEIGHT = 1000, 800
PLAYER_SIZE = 20
PLAYER_SPEED = 5


# Colors
WHITE = (255, 255, 255)
RED = (255, 0, 0)
GREEN = (0, 255, 0)
BLUE = (0, 0, 255)

# Create the screen
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Icy Floor")

# Player
player = pygame.Rect(WIDTH // 2 - PLAYER_SIZE // 2, HEIGHT // 2 - PLAYER_SIZE // 2, PLAYER_SIZE, PLAYER_SIZE)

player_x_velocity = 0
player_y_velocity = 0

font = pygame.font.Font(None, 36)

# Game Intro
intro_text = font.render("player ice man guy game", True, BLUE)
screen.blit(intro_text, (WIDTH // 2 - 150, HEIGHT // 2 - 18))
pygame.display.flip()
pygame.time.wait(3000)



# Game loop
clock = pygame.time.Clock()
running = True

while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and player.left > 0:
        player.x -= PLAYER_SPEED
        if player_x_velocity > -3:
          player_x_velocity -= 1
    if keys[pygame.K_RIGHT] and player.right < WIDTH:
        player.x += PLAYER_SPEED
        if player_x_velocity < 3:
          player_x_velocity += 1
    if keys[pygame.K_UP] and player.left > 0:
      player.y -= PLAYER_SPEED
      if player_y_velocity > -3:
        player_y_velocity -= 1
    if keys[pygame.K_DOWN] and player.right < WIDTH:
      player.y += PLAYER_SPEED
      if player_y_velocity < 3:
        player_y_velocity += 1

    player.x += player_x_velocity
    player.y += player_y_velocity
    player_x_velocity *= 0.9
    player_y_velocity *= 0.9
    # Clear screen

    # Clear the screen
    screen.fill(WHITE)

    # Draw player
    pygame.draw.rect(screen, BLUE, player)
  

    pygame.display.flip()
    clock.tick(60)


pygame.quit()
sys.exit()
```

This worked much better, and soft corners are my favorite thing. 

The code works by moving the player and giving the player velocity, then moving the player based on that velocity

### Current Plans for the game

I will be using the slidy ice movement in my game, as it is not making my computer restart.

### Bonus Cool Thing

This is one of the things that I got distracted with, but I think I will actually add this to my game. I found someone else's code on how to rotate a rectangle in pygamea constant amount every loop, and changed the code so that it follows the cursor around. 

Code:

```
import pygame as py  
from math import atan
from math import pi

# define constants  
WIDTH = 500  
HEIGHT = 500  
FPS = 30  

# define colors  
BLACK = (0 , 0 , 0)  
GREEN = (0 , 255 , 0)  

# initialize pygame and create screen  
py.init()  
screen = py.display.set_mode((WIDTH , HEIGHT))  
# for setting FPS  
clock = py.time.Clock()  

rot = 0  
# find angle to mouse
mouse = py.mouse.get_pos()
grid_value = (mouse[0]-HEIGHT//2, WIDTH//2-mouse[1])
if not grid_value[0] == 0:
    m = (grid_value[1]-0)/(grid_value[0]-0)
linedeg = atan(m)*180/pi  

# define a surface (RECTANGLE)  
image_orig = py.Surface((200 , 100))  
# for making transparent background while rotating an image  
image_orig.set_colorkey(BLACK)  
# fill the rectangle / surface with green color  
image_orig.fill(GREEN)  
# creating a copy of orignal image for smooth rotation  
image = image_orig.copy()  
image.set_colorkey(BLACK)  
# define rect for placing the rectangle at the desired position  
rect = image.get_rect()  
rect.center = (WIDTH // 2 , HEIGHT // 2)  
# keep rotating the rectangle until running is set to False  
running = True  
while running:  
    # set FPS  
    clock.tick(FPS)  
    # clear the screen every time before drawing new objects  
    screen.fill(BLACK)  
    # check for the exit  
    for event in py.event.get():  
        if event.type == py.QUIT:  
            running = False  
    
    mouse = py.mouse.get_pos()
    grid_value = (mouse[0]-HEIGHT//2, WIDTH//2-mouse[1])
    if not grid_value[0] == 0:
        m = (grid_value[1]-0)/(grid_value[0]-0)
    linedeg = atan(m)*180/pi 

    # making a copy of the old center of the rectangle  
    old_center = rect.center  
    # defining angle of the rotation  
    rot = (linedeg) 
    # rotating the orignal image  
    new_image = py.transform.rotate(image_orig , rot)  
    rect = new_image.get_rect()  
    # set the rotated rectangle to the old center  
    rect.center = old_center  
    # drawing the rotated rectangle to the screen  
    screen.blit(new_image , rect)  
    # flipping the display after drawing everything  
    py.display.flip()  

py.quit()  
```

I think this effect looks cool, and I wat to use it in the actual game

### In Conclusion and plan for Next Week

In the next week I will work on making an actual game as the project is due fairly soon (Like less than 7 days). All project for me. Workedy worky work. Why is werk spelled work anyway?

I think I have a good basis to work off of.
