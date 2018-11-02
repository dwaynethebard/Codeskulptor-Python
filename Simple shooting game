import simplegui, math ,random

# Canvas dimensions
CANVAS_WIDTH = 800
CANVAS_HEIGHT = 600

# Global variables
time = 0
player_bullets = []
enemy_bullets = []
enemy_ships = []
playing = True
shooting = False
# Load players ship and information
player_image = simplegui.load_image("https://res.cloudinary.com/teepublic/image/private/s--30ToqKwE--/t_Preview/b_rgb:191919,c_limit,f_jpg,h_630,q_90,w_630/v1446229133/production/designs/219162_1.jpg")
PLAYER_IMG_WIDTH = player_image.get_width()
PLAYER_IMG_HEIGHT = player_image.get_height()

# Load enemy ship and information

enemy_image = simplegui.load_image("https://storage.pixteller.com/designs/designs-images/2017-08-02/05/8-bit-spaceship-1-598134c98aa6e.png")
ENEMY_IMG_WIDTH = enemy_image.get_width()
ENEMY_IMG_HEIGHT = enemy_image.get_height()

# Calculate the distance between two points
def dist(pos1, pos2):
    a = pos2[1]-pos1[1]
    b = pos2[0]-pos1[0]
    dis = math.sqrt(a**2+b**2)
    return dis

# The basic Ship class for both player and enemy
class Ship:
    def __init__(self, image, position, velocity,height,width):
        self.image = image
        self.pos = position
        self.vel = velocity
        self.turn = 0
        self.turn_speed = 0
        self.rad = height
        self.height = height
        self.width = width
    # Draw the ship on the screen    
    def draw(self, canvas):    
        canvas.draw_image(self.image, 
                          (self.width/2, self.height/2),
                          (self.width, self.height),
                          self.pos,
                          [30,30],
                          self.turn)
    # Update the position of the ship
    def update(self):
        self.turn+=self.turn_speed
        for i in range(2):
            self.pos[i] += self.vel[i]
    # Has the ship collided with a bullet
    def has_collided(self, other):
        dis = dist(self.pos, other.pos)
        # Returns true or false
        return dis <= 40

# Enemy ships
class Enemy(Ship):
    # Create a seperate list of bullets for the enemys              
    def shoot(self):
        enemy_bullets.append(Bullet([self.pos[0],self.pos[1]],6,self.turn))
        
    def player_angle(self,other):
        x_dist = self.pos[0]-other.pos[0]
        y_dist = self.pos[1]-other.pos[1]
        self.turn = -math.atan2(x_dist, y_dist)
        
        
# Player ship        
class Player(Ship):
    # Create a seperate list of bullets for the enemys                            
    def shoot(self):
        player_bullets.append(Bullet([self.pos[0],self.pos[1]],6,self.turn))
         
    # Has the ship hit the boarder    
    def hit_wall(self):
        # if you are moving away from the boarder
        if (self.pos[0]==80 and self.vel[0]>0) or (self.pos[0]==716 and self.vel[0]==-3):
            pass
        # If you hit the boarder you need to stop moving
        elif self.pos[0]<=80 or self.pos[0]>=716 :
            self.vel[0]=0
        # if you are moving away from the boarder    
        if (self.pos[1]==78 and self.vel[1]==3) or (self.pos[1]==519 and self.vel[1]==-3):
            pass
        # If you hit the boarder you need to stop moving
        elif self.pos[1]<=80 or self.pos[1]>=519 :
            self.vel[1]=0

# Bullet class
class Bullet:
    def __init__(self, position, velocity,angle):
        self.pos = position
        self.vel = [-velocity*math.sin(-angle),-velocity*math.cos(-angle)]
        self.angle=angle
        
        
    def draw(self, canvas):
        canvas.draw_circle(self.pos, 5, 1, 'white', 'white')

    def update(self):
        for i in range(2):
            self.pos[i] += self.vel[i]

# Handler for keystrokes
def key_down(key):
    global shooting
    if simplegui.KEY_MAP['space'] == key:
        shooting = True
    if simplegui.KEY_MAP['left'] == key:
        ship.vel[0]=-3
    if simplegui.KEY_MAP['right'] == key:
        ship.vel[0]= 3
    if simplegui.KEY_MAP['up'] == key:
        ship.vel[1]=-3
    if simplegui.KEY_MAP['down'] == key:
        ship.vel[1]=3
    if simplegui.KEY_MAP['a'] == key:
        ship.turn_speed=-math.pi/50   
    if simplegui.KEY_MAP['d'] == key:
        ship.turn_speed=math.pi/50  
        
# Handler for keystrokes        
def key_up(key):
    global shooting
    if simplegui.KEY_MAP['space'] == key:
        shooting = False
    if simplegui.KEY_MAP['left'] == key:
        ship.vel[0]=0
    if simplegui.KEY_MAP['right'] == key:
        ship.vel[0]= 0
    if simplegui.KEY_MAP['up'] == key:
        ship.vel[1]=0
    if simplegui.KEY_MAP['down'] == key:
        ship.vel[1]=0
    if simplegui.KEY_MAP['a'] == key:
        ship.turn_speed=0   
    if simplegui.KEY_MAP['d'] == key:
        ship.turn_speed=0 
        
# Handler to draw on canvas
def draw(canvas):
    # Draw the boarder
    global playing , ship
    canvas.draw_polyline([[50, 50], [750, 50], [750, 550],[50,550],[50,50]], 20, 'Blue')
    if playing:
        global player_bullets, time,enemy_bullets
        # Is the ship hitting the wall
        ship.hit_wall()
        
        # Draw the enemy ships
        for enemy in enemy_ships:
            enemy.draw(canvas)
            
            
        # Check to see if your bullets has hit the enemy ships
        for enemy in enemy_ships:
            for bullet in player_bullets:
                if enemy.has_collided(bullet):
                    enemy_ships.remove(enemy)
                    player_bullets.remove(bullet)
                    
        # Check to see if enemy bullets has hit your ship
        for bullet in enemy_bullets:
            if ship.has_collided(bullet):
                playing = False
                    
                    
        # Spawn enemy ships. Screen refreshes 60/sec.        
        if time % 90==0:
            # Set random position
            x = random.randrange(70,CANVAS_WIDTH-70)
            y = random.randrange(70,CANVAS_HEIGHT-70)
            enemy_ships.append(Enemy(enemy_image,[x,y],[0,0],ENEMY_IMG_HEIGHT,ENEMY_IMG_WIDTH))
        
        # Draw and update player ship
        ship.draw(canvas)
        ship.update()
        
        # Allow continiuous shooting
        if shooting and time % 8==0:
            ship.shoot()
        # Draw and update player bullets
        for bullet in player_bullets:
            bullet.draw(canvas)
            bullet.update()
        # Draw and update enemy ship    
        for bullet in enemy_bullets:
            bullet.draw(canvas)
            bullet.update()
        time+=1
        
        # timer for how often enemy shoots
        if time%60==0:
            for enemy in enemy_ships:
                    enemy.shoot()
                    pass
        
        for enemy in enemy_ships:
            enemy.player_angle(ship)
        # Remove bullets from list once they are out of the screen
        player_bullets = filter(lambda bullet: (bullet.pos[1]<535 and bullet.pos[1]>65 and bullet.pos[0]<735 and bullet.pos[0]>65), player_bullets)
        enemy_bullets = filter(lambda bullet: (bullet.pos[1]<535 and bullet.pos[1]>65 and bullet.pos[0]<735 and bullet.pos[0]>65), enemy_bullets)

        
    else:
        canvas.draw_text('GAME OVER', (300, 300),48, 'Red')

def start():
    global playing , enemy_ships,player_bullets,enemy_bullets , ship
    enemy_bullets = []
    player_bullets = []
    enemy_ships = []
    ship.pos[0]=410
    ship.pos[1]=450
    playing = True
    
ship = Player(player_image,[410,450],[0,0],PLAYER_IMG_HEIGHT,PLAYER_IMG_WIDTH)

# Create a frame and assign callbacks to event handlers
frame = simplegui.create_frame("Home", CANVAS_WIDTH, CANVAS_HEIGHT)
frame.set_draw_handler(draw)
frame.set_keydown_handler(key_down)
frame.set_keyup_handler(key_up)
frame.add_button('Start', start)
frame.add_label(' press "A" to turn left')
frame.add_label(' press "D" to turn left')
frame.add_label(' press "Space" to fire')
frame.add_label(' directional pad to move')

frame.set_canvas_background("#191919")
# Start the frame animation
frame.start()
