# projects

"""
graphics_interface.py
Graphical interface for the Lunar Lander game
Originally by Andy Exley; modified by Cary Gray and Janet Davis
"""

from graphics import *
from button import Button 
import turtle
import random

class LanderDrawing:
    """A drawing of the lunar lander."""
    
    def __init__(self, win):
        """Create the shape, initially undrawn, at 0,0.
        
        Parameters:
            win: the GraphicsWindow where this will be drawn
        """
        
        self.win = win
        self.x = 0
        self.y = 0
        self.drawn = False
        self.polygon = Polygon(Point(-10, 0),
                Point(-3, 10),
                Point(3, 10),
                Point(10, 0))
        self.polygon.setFill("blue")

    def drawAt(self, newX, newY):
        """Move and draw the shape at newX, newY."""
        dx = newX - self.x
        dy = newY - self.y
        self.polygon.move(dx, dy)
        if not self.drawn:
            self.drawn = True
            self.polygon.draw(self.win)
        self.x = newX
        self.y = newY

class LanderInterface:
    """GraphicLanderInterface class is a graphical interface 
       for your lunar lander game"""

    def __init__(self, alt):
        """Constructor that initializes the graphics window
        and shapes that we will use for drawing things.
        """

        # initialize window
        self.win = GraphWin("Lunar Lander Game", 300, 900)
        # transform coordinates
        self.height = 900
        self.win.setCoords(0, -10, 300, self.height)

        

        self.surface = self.createSurface()
        self.surface.draw(self.win)
        
        self.star = self.createStars(Point(150, 50), 35, "yellow")
        self.star.draw(self.win)
        
        for i in range(300):
            x = random.randint(0, 300)
            y = random.randint(600, 830)
            self.star = self.createStars(Point(x, y), 2, "white")
            self.star.draw(self.win)    
    
    
        
        
        self.drawing = LanderDrawing(self.win)

        self.thrustButton = Button(Point(30, self.height / 2), Point(110, self.height / 2 + 30), "Thrust")
        self.noThrustButton = Button(Point(210, self.height / 2), Point(290, self.height / 2 + 30), "No Thrust") 
        self.thrustButton.draw(self.win)
        self.noThrustButton.draw(self.win)

        self.gameDetails = Text(Point(150, self.height - 30), f"Wait for game to start")
        self.gameDetails.draw(self.win)
        
        self.text = ""
        
        
    def showInfo(self, lander):
        """Get the lander info then (re)draw it.
        
        That's it. It doesn't actually show any information."""
        alt = lander.getAltitude()
        fuel = lander.getFuel() 
        vel = lander.getVelocity() 
        
    
        text = f"\n Altitude: {alt} \n Velocity: {vel} \n Fuel: {fuel}"
        self.gameDetails.setText(text)
        self.drawing.drawAt(self.win.width / 2, alt)

    def getThrust(self):
        """Wait for a user's mouse click then return 0 thrust.
        
        You'll want to fix this to avoid catastrophic incidents.
        """
        pt = self.win.getMouse()
        x, y = pt.getX(), pt.getY()

        if (x >= 30 and x <= 110) and (y >= self.height // 2 and y <= self.height // 2 + 40):
            return 1
        
        elif (x >= 200 and x <= 280) and (y >= self.height // 2 and y <= self.height // 2 + 40):
            return 0

        else: 
            print("Please click on the actual button to apply thrust")
        
        return 0

    def showCrash(self):
        """Crash100 message... change this to graphical message"""
        
        crashingText = Text(Point(130,int(self.height*0.35)), "Crash! Oh noes!")
        crashingText.draw(self.win)
        self.drawing.drawAt(self.win.width / 2, 0)
        print("Crash! Oh noes!")

    def showLanding(self):
        """Landing message... change this to graphical message"""
        
        LandingText = Text(Point(130,int(self.height*0.35)), "Hooray, the Eagle has landed!")
        LandingText.draw(self.win)
        print("Hooray, the Eagle has landed!")

    def close(self):
        self.win.close()

    def createSurface(self):
        """Draws the surface of the moon"""
        
        circ = Circle(Point(150,-300),300)
        circ.setFill("slate gray")
        return circ
    
    def createStars(self, p, radius, color):
        """Draws stars as circles"""
        circle = Circle(p, radius)
        circle.setFill(color)
        return circle
