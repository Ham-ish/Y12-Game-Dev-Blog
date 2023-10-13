# Game Dev Blog 

## 13/10/23: Beginning of ICAW things

### Overveiw

This Blog is the overview for the last week of last term, as well as any important that happened over the Holidays (Lots). 

### Stuff From the Holidays

I have made a problematic amount of progress on my game dev assignment. Prior to the holidays, I had done nothing on the game dev assignment except the GDD. During the holidays I started getting some actual progress on what I am going to be handing in, in **4 weeks**. 

I began by looking into a game engine that was suggested to me called Godot. In the first week of the holidays I downloaded Godot and began expenrimenting with a hex grid system that made a flooring grid, which looked like this:

<img src="../Images/Godot tiles.png" width=400px alt="Tiles">
<img src="../Images/Godot Tiles settings.png" width=400px alt="Tile settings">


I then realised that I had no idea how any of that worked, so I began on a tutorial to create a simple 2D game over the second week. 

I began with by creating a scene (A sort of referencable object) of the player character. I gave it a collision and animation node as well as some images to define its looks and animations. Using these I created a movement and animation script.

Here is the player setup:

<img src="../Images/Godot player setup.png" width=400px alt="Player setup">

And here is the script:

```GDScript
extends Area2D
signal hit
@export var speed = 400 # How fast the player will move (pixels/sec).
var screen_size # Size of the game window.

# Called when the node enters the scene tree for the first time.
func _ready():
	screen_size = get_viewport_rect().size
	hide()
	pass


# Called every frame. 'delta' is the elapsed time since the previous frame.
func _process(delta):
	var velocity = Vector2.ZERO # The player's movemnent vector.
	if Input.is_action_pressed("move_up"): # Movement Controls
		velocity.y -= 1
	if Input.is_action_pressed("move_down"):
		velocity.y += 1
	if Input.is_action_pressed("move_left"):
		velocity.x -= 1
	if Input.is_action_pressed("move_right"):
		velocity.x += 1
		
	if velocity.length() > 0:
		velocity = velocity.normalized() *speed
		$AnimatedSprite2D.play()
	else:
		$AnimatedSprite2D.stop()
		
	position += velocity * delta
	position = position.clamp(Vector2.ZERO, screen_size)
	
	if velocity.x != 0: # Play animations based on direction velocity
		$AnimatedSprite2D.animation = "Walk"
		$AnimatedSprite2D.flip_v = false
		$AnimatedSprite2D.flip_h = velocity.x < 0
	elif velocity.y != 0:
		$AnimatedSprite2D.animation = "up"
		$AnimatedSprite2D.flip_v = velocity.y > 0
	pass


func _on_body_entered(body): # a function to emit a signal when the player touches an enemy
	hide() 
	hit.emit() # Emits a signal called hit, which can be used in other scenes
	$CollisionShape2D.set_deferred("disabled", true)
	pass 
func start(pos): # Function for startig the game
	position = pos
	show()
	$CollisionShape2D.disabled = false

```

That is where I got up to in the holidays

### Current Plans for the game

Godot seems like a good place to make a game. I will progress further in it to creater the game. 

Last time I created a game I severely underestimated how long it would take to make a game. I may have already made that mistake again this time, but I will see what happens.

### In Conclusion and plan for Next Week

To do:

- See What Happens

- Work on Godot
