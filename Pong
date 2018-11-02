# Implementation of classic arcade game Pong

import simplegui
import random

# initialize globals - pos and vel encode vertical info for paddles
#width and height of screen
WIDTH = 600
HEIGHT = 400
# ball radius
BALL_RADIUS = 20
#pads dimensions
PAD_WIDTH = 8
PAD_HEIGHT = 80
HALF_PAD_WIDTH = PAD_WIDTH / 2
HALF_PAD_HEIGHT = PAD_HEIGHT / 2
# ball position and velocity
ball_pos = [0, 0]
ball_vel = [0, 0]
#which way the ball moves at the start
right = True
#starting position of the paddles, centers the paddles, only the y coordinates
paddle1_pos = HEIGHT / 2 - HALF_PAD_HEIGHT
paddle2_pos = HEIGHT / 2 - HALF_PAD_HEIGHT
#paddle velocity
paddle1_vel = 0
paddle2_vel = 0
#score
score1 = 0
score2 = 0
total_score=score1+score2
#temporary velocity
tmp_vel = [0, 0]
# is game paused
pause_check = False

# helper function that spawns a ball, returns a position vector and a velocity vector
# if right is True, spawn to the right, else spawn to the left
def ball_init(right):
    global ball_pos, ball_vel , total_score # these are vectors stored as lists
    total_score=score1+score2
    ## ball positon is halve the dimensions, then round down to nearest int
    ball_pos = [WIDTH // 2, HEIGHT // 2]
    # Determin the velocity
    min_score= min(score1,score2)
    ball_vel[0] = random.randrange(min(score1,score2)+1, max(score1,score2)+2)
    ball_vel[1] = -1 * random.randrange(1, 3)
    if not right:
        ball_vel[0] *= -1
    pass

def check_collision():
    global ball_pos, right, score1, score2
    # check if the ball is at the end of the screen and could score of bounce of paddle
    # Checking the paddle on the left side
    # ball_pos[0] - BALL_RADIUS is the left most point on the ball.
    # the can only go as far as the pad width
    if ball_pos[0] - BALL_RADIUS <= PAD_WIDTH: 
        # if the y positions match then the ball bounces
        # paddle1_pos is the y corridnate of the top of the paddle
        # So if ball is between the top and bottom coordinates we reflect the ball
        if paddle1_pos <= ball_pos[1] and paddle1_pos + PAD_HEIGHT >= ball_pos[1]:
            reflect()
        # otherwise the ball scores    
        else:
            score2 += 1
            # the ball goes towards whoever just scored
            right = True
            ball_init(right)
            
    if ball_pos[0] + BALL_RADIUS >= WIDTH - PAD_WIDTH:
        if paddle2_pos <= ball_pos[1] and paddle2_pos + PAD_HEIGHT >= ball_pos[1]:
            reflect()
        else:
            score1 += 1
            right = False
            ball_init(right)
# reflect the ball, just mult the x velocity by -1
# and increase the speed of the ball slightly
def reflect():
    global ball_vel
    ball_vel[0] = - ball_vel[0]
    ball_vel[0] *= 1.1
    ball_vel[1] *= 1.1
# new game
def begin_new_game():
    global right
    n = random.randrange(0, 2)
    if n == 0:
        right = False
    else:
        right = True
    ball_init(right)

# define event handlers
def init():
    global pause_check, paddle1_pos, paddle2_pos, paddle1_vel, paddle2_vel, right  # these are floats
    global score1, score2  # these are ints
    score1 = 0
    score2 = 0
    paddle1_pos = HEIGHT / 2 - HALF_PAD_HEIGHT
    paddle2_pos = HEIGHT / 2 - HALF_PAD_HEIGHT
    begin_new_game()
    pause_check = False
    pass

def draw(c):
    global right, score1, score2, paddle1_pos, paddle2_pos, ball_pos, ball_vel, paddle1_vel, paddle2_vel
    
    # check collision with the gutters
    check_collision()
    
    # update paddle's vertical position
    paddle1_pos += paddle1_vel
    paddle2_pos += paddle2_vel
    
    # keep paddle on the screen of player 1
    if paddle1_pos + PAD_HEIGHT >= HEIGHT:
        paddle1_pos = HEIGHT - PAD_HEIGHT
    elif paddle1_pos <= 0:
        paddle1_pos = 0
    # keep paddle on the screen of player 2
    if paddle2_pos + PAD_HEIGHT >= HEIGHT:
        paddle2_pos = HEIGHT - PAD_HEIGHT
    elif paddle2_pos <= 0:
        paddle2_pos = 0

    # draw mid line and gutters
    c.draw_line([WIDTH / 2, 0],[WIDTH / 2, HEIGHT], 1, "White")
    c.draw_line([PAD_WIDTH, 0],[PAD_WIDTH, HEIGHT], 1, "White")
    c.draw_line([WIDTH - PAD_WIDTH, 0],[WIDTH - PAD_WIDTH, HEIGHT], 1, "White")
    
    # draw paddles
    c.draw_line([HALF_PAD_WIDTH, paddle1_pos], [HALF_PAD_WIDTH, paddle1_pos + PAD_HEIGHT], PAD_WIDTH, "White")
    c.draw_line([WIDTH - HALF_PAD_WIDTH, paddle2_pos], [WIDTH - HALF_PAD_WIDTH, paddle2_pos + PAD_HEIGHT], PAD_WIDTH, "White")
    
    # update ball, if the ball touches the top or the bottom of the screen
    if ball_pos[1] <= BALL_RADIUS or ball_pos[1] >= HEIGHT - 1 - BALL_RADIUS:
        ball_vel[1] = - ball_vel[1]
        
    ball_pos[0] += ball_vel[0]
    ball_pos[1] += ball_vel[1]
    
    # draw ball and scores
    
    c.draw_circle(ball_pos, BALL_RADIUS, 1, "White", "White")
    c.draw_text(str(score1), [100, 100], 50, "Green")
    c.draw_text(str(score2), [500, 100], 50, "Green")
    
# define the up and down keys for player one and player two
def keydown(key):
    global paddle1_vel, pause_check, paddle2_vel
    if not pause_check:
        if key == simplegui.KEY_MAP["up"]:
            paddle2_vel = -6
        elif key == simplegui.KEY_MAP["down"]:
            paddle2_vel = 6
        
        if key == simplegui.KEY_MAP["w"]:
            paddle1_vel = -6
        elif key == simplegui.KEY_MAP["s"]:
            paddle1_vel = 6

def keyup(key):
    global paddle1_vel, paddle2_vel
    paddle2_vel = 0
    paddle1_vel = 0
    
def pause():
    global tmp_vel, ball_vel, pause_check
    if not pause_check:
        pause_check = True
        tmp_vel = ball_vel
        ball_vel = [0, 0]
    pass

def play():
    global pause_check, tmp_vel, ball_vel
    if pause_check:
        pause_check = False
        ball_vel = tmp_vel
    pass


# create frame
frame = simplegui.create_frame("Pong", WIDTH, HEIGHT)
frame.set_draw_handler(draw)
frame.set_keydown_handler(keydown)
frame.set_keyup_handler(keyup)
frame.add_button("Restart", init, 100)
frame.add_button("Pause", pause, 100)
frame.add_button("Play", play, 100)
frame.add_label(' Left player Up: w')
frame.add_label(' Left player Down: s')
frame.add_label(' Right player Up: up directional pad')
frame.add_label(' Right player Down: down directional pad')
# start frame
init()
frame.start()
begin_new_game()
