# The-Vikings
Ap computer science final Greenfoot game

https://www.google.com/url?q=https://www.greenfoot.org/files/javadoc/&sa=D&ust=1527110096544000&usg=AFQjCNF1y1sWydbsJbhfEpWY5SDWRernxA
https://www.greenfoot.org/doc/tut-3
https://www.youtube.com/watch?v=RkVIVipKpfk


MyWorld:

super(1150, 600, 1);
        
Actor life = new Life();//first heart
this.addObject( life, 50, 50 );



Life:
private int heart;
    
    public Life()
    {
        heart = 1;//1 Hit point
    }

something turning:

int rotationalSpeed = 1;
int radius = 200; // adjust as needed
 
public void orbitWorldCenter()
{
    setLocation(getWorld().getWidth()/2, getWorld().getHeight()/2);
    turn(rotationalSpeed-90);
    move(radius);
    turn(90);
}



public void act() 
   {
       int cx = getWorld().getWidth()/2;
       int cy = getWorld().getHeight()/2;
       int speed= 5;
        
       turnTowards(cx,cy);
       int rotation = getRotation();
        
       setRotation(rotation + 89);
       move (speed);
        
   }




