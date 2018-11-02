# Simple paint

# Import statements
import simplegui
# Initialize global variables 
List = []
Current_thickness = 3
Current_color = 'white'
# Definition of Dot class
# Should have thickness, color and location attributes
class Dot:
       def __init__(self,x,y,thickness,color):
            self.x=x
            self.y=y
            self.thickness=thickness
            self.color=color

    
# Handler for clear button
def clear_handler():
    while List:
        del List[0]
    
    
# Handler to change thickness    
def thickness_handler(text_input):
    global Current_thickness
    try:
        if int(text_input)>0:
            Current_thickness=int(text_input)
    except ValueError:
        pass  # it was a string, not an int.

# Handler to change color    
def color_handler(text_input):
    global Current_color
    Current_color = text_input

    
# Mouse click handler
# Adds a dot at that position using the current 
# thickness and color settings

def mouse_handler(position):
    dots=Dot(position[0],position[1],Current_thickness,Current_color)
    List.append(dots)

    
    
# Handler to draw on canvas
def draw(canvas):
    if List:
        for Dot in List:
            canvas.draw_circle((Dot.x,Dot.y), Dot.thickness, 1, Dot.color, Dot.color)



        
# Create a frame 
frame = simplegui.create_frame('Doodle', 300, 300)
# Create buttons & text inputs

frame.add_button('Clear', clear_handler, 50)

frame.add_input('Thickness', thickness_handler, 50)

frame.add_input('Color', color_handler, 50)
# Assign callbacks to handler functions
frame.set_mouseclick_handler(mouse_handler)
frame.set_mousedrag_handler(mouse_handler)
frame.set_draw_handler(draw)
frame.add_label("Hit enter to change color or thickness")
# Start the frame
frame.start()
