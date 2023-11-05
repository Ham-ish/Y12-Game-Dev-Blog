# Game Dev Blog 

## 23/10/23: Random Pygame Blog

### Overveiw

Heeeeyyyy, the 23rd blog this year is due on the 23rd.

I have gone and done *none* of the things that I said that I would do last week. However, I made Pygame work and created a simple game to try and get my brain back into a game capable state, and so that I would have something to present in this blog.

### That Pygame

I created a game where the player must dodge and shoot men falling from the sky.

It is a very simple space-invader-like that I made to see if I remember how to code in Pygame, and to see if it would be a better system to use for this game.

there is the code for that game:

```
import pygame
import sys
import random

# Initialize Pygame
pygame.init()

# Constants
WIDTH, HEIGHT = 600, 400
PLAYER_SIZE = 40
BULLET_SIZE = 10
PLAYER_SPEED = 5
BULLET_SPEED = 10

# Colors
WHITE = (255, 255, 255)
RED = (255, 0, 0)
GREEN = (0, 255, 0)
BLUE = (0, 0, 255)

# Create the screen
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Bullet Rain")


# Player
player = pygame.Rect(WIDTH // 2 - PLAYER_SIZE // 2, HEIGHT - PLAYER_SIZE, PLAYER_SIZE, PLAYER_SIZE)

# Bullets
bullets = []

# Enemies
enemies = []




# Level system
score = 0

def get_current_level(score):
    return abs(score // 20)

current_level = get_current_level(score)

def update_level_data(current_level):
    enemy_size = 30 + 10 * current_level
    enemy_speed = 2 + 0.5 * current_level
    player_speed = 5 + 0.5 * current_level
    return enemy_size, enemy_speed, player_speed

enemy_size, enemy_speed, PLAYER_SPEED = update_level_data(current_level)

# Create Font
font = pygame.font.Font(None, 36)

# Make the enemy rectangles Evil Guys
enemy_surface = pygame.image.load('Screenshot 2023-10-19 at 3.05.42 pm.png')
enemy_image = pygame.transform.scale(enemy_surface, (enemy_size, enemy_size))

def spawn_enemy():
  x = random.randint(0, WIDTH - enemy_size)
  y = 0
  enemy = pygame.Rect(x, y, enemy_size, enemy_size)
  return enemy

# Game Intro
pygame.event.get()
screen.fill(WHITE)
intro_text = font.render("Welcome to Bullet Rain", True, BLUE)
text_width = intro_text.get_width() //2
text_height = intro_text.get_height() // 2
screen.blit(intro_text, (WIDTH // 2 - text_width, HEIGHT // 2 - text_height))
pygame.display.flip()
pygame.time.wait(3000)

# Game loop
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    keys = pygame.key.get_pressed()
  
    if keys[pygame.K_LEFT] and player.left > 0:
        player.x -= PLAYER_SPEED
    if keys[pygame.K_RIGHT] and player.right < WIDTH:
        player.x += PLAYER_SPEED

    # Shoot bullets
    if keys[pygame.K_UP]:
        bullet = pygame.Rect(player.centerx - BULLET_SIZE // 2, player.top, BULLET_SIZE, BULLET_SIZE)
        bullets.append(bullet)
    
    # Reset creen
    screen.fill(WHITE)
  
    # Move bullets
    for bullet in bullets:
        bullet.y -= BULLET_SPEED
        if bullet.y < 0:
            bullets.remove(bullet)
        pygame.draw.rect(screen, RED, bullet)

    # Random enemy spawning
    if random.random() < 0.2:
        enemy = spawn_enemy()
        enemies.append(enemy)


    for enemy in enemies:
        # Move enemies
        enemy.y += enemy_speed
        # Draw enemies
        if enemy.y > HEIGHT:
            # Remove enemies below screen
            enemies.remove(enemy)
        if player.colliderect(enemy):
          # Game over
          running = False
          break
        for bullet in bullets:
          if bullet.colliderect(enemy):
              bullets.remove(bullet)
              if enemy in enemies:
                  enemies.remove(enemy)
              score += 1
        pygame.draw.rect(screen, WHITE, enemy)
        screen.blit(enemy_image, enemy)
    enemy_surface = pygame.image.load('Screenshot 2023-10-19 at 3.05.42 pm.png')
    enemy_image = pygame.transform.scale(enemy_surface, (enemy_size, enemy_size))

    # Check for level progression
    new_level = get_current_level(score)
    if new_level > current_level:
        current_level = new_level
        enemy_size, enemy_speed, PLAYER_SPEED = update_level_data(current_level)


    # Draw player
    pygame.draw.rect(screen, BLUE, player)

    # Draw level counter in the corner
    level_text = font.render(f"Level {current_level + 1} Next level {score%20}/{20}", True, BLUE)
    screen.blit(level_text, (10, 10))

    pygame.display.flip()

pygame.time.wait(300)



screen.fill(RED)

game_over_text = font.render(f"Game Over - Score: {score}", True, BLUE)
text_width = game_over_text.get_width() //2
text_height = game_over_text.get_height() // 2
screen.blit(game_over_text, (WIDTH // 2 - text_width, HEIGHT // 2 - text_height))
pygame.display.flip()
pygame.time.wait(3000)


pygame.quit()
sys.exit()
```

The player moves around and shoots bullets whish get added to a bullet list, enemies spawn randomly and are added to an enemy list, if the bullet hits an enemy they both get destroyed, if the player touches an enemy the game ends and the game over screen appears. Every cycle this happens, the objects information is saved, the screen is cleared and everything is drawn again.

As the player kills more enemies, their score goes higher, and as their score goes higher the enemies get bigger and faster.

THe part of this code that I think is the coolest though is the surface code, which is the code that allows the enemy rectangles to have the image of the not kingpin that I made in Dall-E

### How this will affect the game

I have come to the conclusion that, with the amount of time that I have left, it will be better to use pygame than Godot, as it makes more sense to me.


### In Conclusion and plan for Next Week

From here I will be playing arou- `*`cough cough`*` working on pygame projects to get closer to a completed game
