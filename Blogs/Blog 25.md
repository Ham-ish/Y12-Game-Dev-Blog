# Game Dev Blog 

## 6/11/23: D-day

### Overveiw

The moment this Blog is due is the same as when the project is due so I am writing this the night before. I have culminated a bunch of ideas into an actual game, sort of. 

### What does sort of mean...

I began by adding a movement system to the rotating square, which mostly meant yoinking the code from the icy movement thing, and then a wall system like in the maze making code. I also changed the colour of everything to match an office asthetic as that is what I plan the first couple of tutorial levels to look like. It currently looks like this:

```
import pygame as py
from math import atan
from math import pi

# define constants
WIDTH = 600
HEIGHT = 600
FPS = 30
SPEED = 2
SLIDE = 5

# define colors
BLACK = (0, 0, 0)
GREEN = (0, 255, 0)
WALL_COLOR = (250, 190, 110)  # Red for walls
FLOOR_COLOUR = (190, 170, 150)
MAN_COLOUR =(255,224,189)
GOAL_COLOR = (255, 255, 0)

# initialize pygame and create screen
py.init()
screen = py.display.set_mode((WIDTH, HEIGHT))
# for setting FPS
clock = py.time.Clock()

levels = [
    {
        'player_spawn': [WIDTH // 2 - 10, HEIGHT // 2 - 5],
        'walls': [(200, 200, 200, 10), (500, 300, 10, 200), (400, 200, 10, 150)],
        'goal_position': (150, 150),  # Example: Define a goal to complete the level
        'start_money': 10
    },
    {
        'player_spawn': [50, 50],  # Example: Different player spawn point for level 2
        'walls': [(100, 200, 300, 10), (450, 300, 10, 200), (100, 450, 200, 10)],
        'goal_position': (200, 200),  # Example: Different goal position for level 2]
        'start_money': 20
    }
]

level_index = 0  # Start with the first level

rot = 0
rect_size = (10, 10)
x_velocity = 0
y_velocity = 0

# Set the initial player position and walls based on the first level
rect_pos = levels[level_index]['player_spawn']
walls = levels[level_index]['walls']
goal_position = levels[level_index]['goal_position']
money = levels[level_index]['start_money']
goal_size = 10, 10

# define a surface (RECTANGLE)
image_orig = py.Surface(rect_size)
# for making transparent background while rotating an image
image_orig.set_colorkey(BLACK)
# fill the rectangle / surface with green color
image_orig.fill(MAN_COLOUR)
# creating a copy of the original image for smooth rotation
image = image_orig.copy()
image.set_colorkey(BLACK)
# define rect for placing the rectangle at the desired position
rect = image.get_rect()
rect.center = (WIDTH // 2, HEIGHT // 2)

level_completed = False

font = py.font.Font(None, 36)

money_display = font.render("Money: "+str(money), True, GOAL_COLOR)
text_width = money_display.get_width() //2
text_height = money_display.get_height() // 2
screen.blit(money_display, (WIDTH // 2 - text_width, HEIGHT // 2 - text_height))

# keep rotating the rectangle until running is set to False
running = True
while running:
    # set FPS
    clock.tick(FPS)
    # clear the screen every time before drawing new objects
    screen.fill(FLOOR_COLOUR)
    # check for the exit
    for event in py.event.get():
        if event.type == py.QUIT:
            running = False

    # Find mouse position
    mouse = py.mouse.get_pos()

    # find angle to mouse
    mouse_angle = (mouse[0] - rect.center[0], rect.center[1] - mouse[1])
    if not mouse_angle[0] == 0:
        m = (mouse_angle[1]) / (mouse_angle[0])
    linedeg = atan(m) * 180 / pi

    keys = py.key.get_pressed()

    # Copy the current position for collision checking
    new_rect_pos = rect_pos.copy()

    if keys[py.K_LEFT]:
        if rect_pos[0] >= 0:
            #new_rect_pos[0] -= SPEED
            if x_velocity > -SLIDE:
                x_velocity -= SPEED
    if keys[py.K_RIGHT]:
        if rect_pos[0] + rect_size[0] <= WIDTH:
            #new_rect_pos[0] += SPEED
            if x_velocity < SLIDE:
                x_velocity += SPEED
    if keys[py.K_UP]:
        if rect_pos[1] >= 0:
            #new_rect_pos[1] -= SPEED
            if y_velocity > -SLIDE:
                y_velocity -= SPEED
    if keys[py.K_DOWN]:
        if rect_pos[1] + rect_size[1] <= HEIGHT:
            #new_rect_pos[1] += SPEED
            if y_velocity < SLIDE:
                y_velocity += SPEED

    # Check for collisions with walls along the X-axis
    x_collision = False
    for wall in walls:
        if (
            new_rect_pos[0] < wall[0] + wall[2] and
            new_rect_pos[0] + rect_size[0] > wall[0] and
            rect_pos[1] < wall[1] + wall[3] and
            rect_pos[1] + rect_size[1] > wall[1]
        ):
            new_rect_pos[0] -= x_velocity * 1.1
            x_velocity = -x_velocity * 0.8
            break

    # Check for collisions with walls along the Y-axis
    y_collision = False
    for wall in walls:
        if (
            rect_pos[0] < wall[0] + wall[2] and
            rect_pos[0] + rect_size[0] > wall[0] and
            new_rect_pos[1] < wall[1] + wall[3] and
            new_rect_pos[1] + rect_size[1] > wall[1]
        ):
            new_rect_pos[1] -= y_velocity * 1.1
            y_velocity = -y_velocity*0.8 
            break

    if not x_collision:
        rect_pos[0] = new_rect_pos[0]
        rect_pos[0] += x_velocity
    print ('x:', x_collision)

    if not y_collision:
        rect_pos[1] = new_rect_pos[1]
        rect_pos[1] += y_velocity
    print ('y:', y_collision)
    # friction
    y_velocity = y_velocity*0.9
    if y_velocity < 0.1 and y_velocity > -0.01:
        y_velocity = 0
    x_velocity = x_velocity*0.9
    if x_velocity < 0.1 and x_velocity > -0.01:
        x_velocity = 0

    # no getting stuck in walls

    if False and x_collision and y_collision:
        if keys[py.K_LEFT]:
            rect_pos[0] += SPEED
        if keys[py.K_RIGHT]:
            rect_pos[0] -= SPEED
        if keys[py.K_UP]:
            rect_pos[1] += SPEED
        if keys[py.K_DOWN]:
            rect_pos[1] -= SPEED
    
    if rect_pos[0] <= 0: 
        rect_pos[0] = 0
        x_velocity = -x_velocity
    if rect_pos[0] + rect_size[0] >= WIDTH:
        rect_pos[0] = WIDTH -rect_size[0]
        x_velocity = -x_velocity

    if rect_pos[1] <= 0: 
        rect_pos[1] = 0
        y_velocity = -y_velocity
    if rect_pos[1] + rect_size[1] >= HEIGHT:
        rect_pos[1] = HEIGHT - rect_size[1]
        y_velocity = -y_velocity

    goal = py.rect.Rect(goal_position[0] - goal_size[0] // 2, goal_position[1] - goal_size[1] // 2, goal_size[0], goal_size[1])

    # Draw the goal indicator
    py.draw.rect(screen, GOAL_COLOR, (goal_position[0] - 5, goal_position[1] - 5, 10, 10))

    # Check for level completion
    if rect.colliderect(goal):
        level_completed = True


    if level_completed:
        if level_index <= len(levels)-1:
            level_index +=1
            level_completed = False
            money = levels[level_index]['start_money']
            x_velocity = 0
            y_velocity = 0
            py.time.wait(500)

    walls = levels[level_index]['walls']
    goal_position = levels[level_index]['goal_position']

    # Defining angle of the rotation
    rot = linedeg
    # Rotating the original image
    new_image = py.transform.rotate(image_orig, rot)
    rect = new_image.get_rect()
    # Set the rotated rectangle to the old center
    rect.center = (rect_pos[0] + rect_size[0] // 2, rect_pos[1] + rect_size[1] // 2)
    # Drawing the rotated rectangle to the screen
    screen.blit(new_image, rect)

    # Draw walls
    for wall in walls:
        py.draw.rect(screen, WALL_COLOR, wall)

    money_display = font.render("Money: "+str(money), True, GOAL_COLOR)
    screen.blit(money_display, (WIDTH // 20 , HEIGHT // 20))

    # Flipping the display after drawing everything
    py.display.flip()
    
py.quit()
```



### Current Plans for the game

The next step in my mind is to add the money mechanics, that increase and decrease money, more levels than just the two that exist

### In Conclusion and plan for Next Week

Be Free, yippee. Not actually, next week is the presentations, so I will be prepping those