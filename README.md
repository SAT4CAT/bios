extends KinematicBody2D

const UP = Vector2(0, -1)
const GRAVITY = 20
const ACCELERATION = 25
const MAX_SPEED = 250
const JUMP_HEIGHT = -550
const DIRECTION_RIGHT = 1
const DIRECTION_LEFT = -1
var direction = Vector2(DIRECTION_RIGHT, 1)

var motion = Vector2()

func set_direction(hor_direction):
    if hor_direction == 0:
        hor_direction = DIRECTION_RIGHT
    var hor_dir_mod = hor_direction / abs(hor_direction)
    apply_scale(Vector2(hor_dir_mod * direction.x, 1))
    direction = Vector2(hor_dir_mod, direction.y)

func _physics_process(delta):
    motion.y += GRAVITY
    var friction = false

    if Input.is_action_pressed("ui_right"):
        motion.x = min(motion.x+ACCELERATION, MAX_SPEED)
        set_direction(DIRECTION_RIGHT)
    elif Input.is_action_pressed("ui_left"):
        motion.x = max(motion.x-ACCELERATION, -MAX_SPEED)
        set_direction(DIRECTION_LEFT)
    else:
        friction = true

    if is_on_floor():
        if Input.is_action_pressed("ui_up"):
            motion.y = JUMP_HEIGHT
        if friction == true:
            motion.x = lerp(motion.x, 0, 0.2)
    else:
        if friction == true:
            motion.x = lerp(motion.x, 0, 0.05)

    motion = move_and_slide(motion, UP)
    pass
