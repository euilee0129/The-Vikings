# The-Vikings
Ap computer science final Greenfoot game

https://www.google.com/url?q=https://www.greenfoot.org/files/javadoc/&sa=D&ust=1527110096544000&usg=AFQjCNF1y1sWydbsJbhfEpWY5SDWRernxA
https://www.greenfoot.org/doc/tut-3
https://www.youtube.com/watch?v=RkVIVipKpfk


Axe:

Viking player1 = new Viking();
    Valkyrie player2 = new Valkyrie();
    ArrayList<Life> p2= new ArrayList<Life>();
    private int countDamage = 0 ;
    public Axe()
    {
        Life life1 = new Life();
        Life life2 = new Life();
        Life life3 = new Life();
        p2.add(life1);
        p2.add(life2);
        p2.add(life3);
    }

    /**
     * Act - do whatever the Axe wants to do. This method is called whenever
     * the 'Act' or 'Run' button gets pressed in the environment.
     */
    public void act() 
    {
        //throwAxe();
        arch();
        if( thrown == true )
        {
            Greenfoot.stop();
        }
        Greenfoot.start();
    } 

    private int takeDamage()
    { 
        int count = 0;
        if(isTouching(Valkyrie.class) )
        {
            count++;
        }
        return count;
    }

    public void location()
    {
        setLocation(player1.getX(), player1.getY() );
    }

    public void damage()
    {
        if(takeDamage() != 0)
        {
            while( countDamage > 0 )
            {
                World world = getWorld();
                world.removeObject(p2.get(countDamage) );
                countDamage--;

            }

        }
    }
    private boolean thrown = false;
    public void throwAxe()
    {
        if( this.isTouching( Valkyrie.class ) == false )
        {
            if( Greenfoot.isKeyDown("l") )
            {
                if( thrown == false )
                {
                    setRotation(90);
                    setLocation(player2.getX(), player2.getY());
                    thrown = true;
                }
                else
                {
                    setLocation( player1.getX(), player1.getY() );
                }
            }
        }
        else
        {
            damage();
        }

    }

    private double posx, posy, vx, vy, ax, ay, dt=0.1;
    public void addedToWorld( World MyWorld )
    {
        posx = getX();
        posy = getY();

        ax= 0;
        ay= 10;

        vx= 100;
        vy=-60;

    }

    public void arch()
    {
        if( Greenfoot.isKeyDown("l") )
        {
            posx += vx*dt+0.5*ax*dt*dt;
            posy += vy*dt+0.5*ay*dt*dt;

            vx+= ax*dt;
            vy+= ay*dt;

            setLocation((int)posx, (int)posy );
            if( isAtEdge() == true && isTouching( Valkyrie.class) == false )
            {
                setLocation(player1.getX()+10, player1.getY() );
            }
            else
            {
                if(isTouching(Valkyrie.class) )
                {
                    damage();
                    Greenfoot.stop();

                }
                else
                {
                    Greenfoot.start();
                }
            }
        }
    }
    
  Viking:
     public class Viking extends Actor
      {
      private int verticalSpeed = 0;
       private int speed = 3;
        private int countJump;
        private boolean jumping = false;
     /**
      * Act - do whatever the Viking wants to do. This method is called whenever
      * the 'Act' or 'Run' button gets pressed in the environment.
      */
     public void act() 
     {
        checkFall();
        checkKeys();
     }  
      public void jump() 
      {
        verticalSpeed = -15;
        fall();
      }
      public void fall()
      {
        setLocation(getX(), getY() + verticalSpeed);
        verticalSpeed++;
      }
      public boolean onGround()
      {
        Actor under = getOneObjectAtOffset( 0, getImage().getHeight()/2, Ship.class );
        return under != null;
        }
     public void checkFall()
     {
        if(onGround() )
        {
            verticalSpeed = 0;
        }
        else 
        {
            fall();
        }
     }
     public void moveForward()
     {
        setLocation(getX() + speed, getY() );
     }
     public void moveBack()
        {
        setLocation(getX() - speed, getY() );
        }
     public void checkKeys()
     {
        if( Greenfoot.isKeyDown("up") )
        {
            doubleJump();
        }
        if( Greenfoot.isKeyDown("left") )
        {
            moveBack();
        }
        if( Greenfoot.isKeyDown("Right") )
        {
            moveForward();
        }
      }
      public void doubleJump()
     {
        if(countJump >= 2 && onGround() )
        {
           countJump = 0;
           jumping = false;
        }
        if(Greenfoot.isKeyDown("up") && jumping == false )
        {
            countJump++;
            jumping = true;
            jump();
        }
        if(jumping  == true && countJump == 1 )
        {
            jump();
            countJump++;
        }
      }
    }
    Ship:


    public class Ship extends Actor
    {
      private int speed = 3;
       /**
       * Act - do whatever the Ship wants to do. This method is called whenever
       * the 'Act' or 'Run' button gets pressed in the environment.
       */
       public void act() 
      {
         move();
         playerOn();
     }    
       public void move()
     {
         if(isAtEdge() )
         {
                speed = -speed;
        }
        setLocation(getX() + speed, getY() );
     }
        public void playerOn() //WORKS 
     {
        Actor player = getOneIntersectingObject(Viking.class);
        if(this.isTouching(Viking.class) && player != null)
        {
            player.setLocation(player.getX() + speed, player.getY() );
        }
    }
}

public class MyWorld extends World
{

    /**
     * Constructor for objects of class MyWorld.
     * 
     */
    public MyWorld()
    {    
        // Create a new world with 600x400 cells with a cell size of 1x1 pixels.
        super(1150, 600, 1);
        Actor vikinglife = new Life(); //1st heart
        this.addObject( vikinglife, 40, 40 );//differences of 50
        
        Actor vikinglife1 = new Life(); //2nd heart
        this.addObject( vikinglife1, 90, 40 );
        
        Actor vikinglife2 = new Life(); //3rd heart
        this.addObject( vikinglife2, 140, 40 );

        
        Actor valkyrielife = new Life(); //1st heart
        this.addObject( valkyrielife, 1010, 40 );//differences of 50
        
        Actor valkyrielife1 = new Life(); //2nd heart
        this.addObject( valkyrielife1, 1060, 40 );
        
        Actor valkyrielife2 = new Life(); //3rd heart
        this.addObject( valkyrielife2, 1110, 40 );
        
        Axe axe = new Axe();
        addObject( axe, 100, getHeight()-100 );
        addObject( new Viking(), 200, getHeight()-200 );
        addObject( new Valkyrie(), 300, getHeight()-200 );
        //addObject( new Ship(), getObjectsAt( 200, getHeight()-200, Viking.class).getX(), getObjectsAt( 200, getHeight()-200, Viking.class).getY(), Ship.class );
        addObject( new Ship(), 150, getHeight()-150 );
        
    }
    
    
}
    public class ScoreBoard extends Actor
    {
        public static final float FONT_SIZE = 48.0f;
    public static final int WIDTH = 400;
    public static final int HEIGHT = 300;
    
    /**
     * Create a score board with dummy result for testing.
     */
    public ScoreBoard()
    {
        this(100);
    }

    /**
     * Create a score board for the final result.
     */
    public ScoreBoard(int score)
    {
        makeImage("Game Over", "Score: ", score);
    }

    /**
     * Make the score board image.
     */
    private void makeImage(String title, String prefix, int score)
    {
        GreenfootImage image = new GreenfootImage(WIDTH, HEIGHT);

        image.setColor(new Color(255,255,255, 128));
        image.fillRect(0, 0, WIDTH, HEIGHT);
        image.setColor(new Color(0, 0, 0, 128));
        image.fillRect(5, 5, WIDTH-10, HEIGHT-10);
        Font font = image.getFont();
        font = font.deriveFont(FONT_SIZE);
        image.setFont(font);
        image.setColor(Color.WHITE);
        image.drawString(title, 60, 100);
        image.drawString(prefix + score, 60, 200);
        setImage(image);
    }
}
public class Asteroid extends SmoothMover
{
    /** Size of this asteroid */
    private int size;

    /** When the stability reaches 0 the asteroid will explode */
    private int stability;

    
    public Asteroid()
    {
        this(50);
    }
    
    public Asteroid(int size)
    {
        super(new Vector(Greenfoot.getRandomNumber(360), 2));
        setSize(size);
    }
    
    public Asteroid(int size, Vector speed)
    {
        super(speed);
        setSize(size);
    }
    
    public void act()
    {         
        move();
    }

    /**
     * Set the size of this asteroid. Note that stability is directly
     * related to size. Smaller asteroids are less stable.
     */
    public void setSize(int size) 
    {
        stability = size;
        this.size = size;
        GreenfootImage image = getImage();
        image.scale(size, size);
    }

    /**
     * Return the current stability of this asteroid. (If it goes down to 
     * zero, it breaks up.)
     */
    public int getStability() 
    {
        return stability;
    }
    
    /**
     * Hit this asteroid dealing the given amount of damage.
     */
    public void hit(int damage) 
    {
        stability = stability - damage;
        if(stability <= 0) 
            breakUp ();         
    }
    
    /**
     * Break up this asteroid. If we are still big enough, this will create two
     * smaller asteroids. If we are small already, just disappear.
     */
    private void breakUp() 
    {
        Greenfoot.playSound("Explosion.wav");
        
        if(size <= 16) 
        {
            getWorld().removeObject(this);
        }
        else 
        {
            int r = getMovement().getDirection() + Greenfoot.getRandomNumber(45);
            double l = getMovement().getLength();
            Vector speed1 = new Vector(r + 60, l * 1.2);
            Vector speed2 = new Vector(r - 60, l * 1.2);        
            Asteroid a1 = new Asteroid(size/2, speed1);
            Asteroid a2 = new Asteroid(size/2, speed2);
            getWorld().addObject(a1, getX(), getY());
            getWorld().addObject(a2, getX(), getY());        
            a1.move();
            a2.move();
        
            getWorld().removeObject(this);
        }
    }
}
public class Bullet extends SmoothMover
{
    /** The damage this bullet will deal */
    private static final int damage = 16;
    
    /** A bullet looses one life each act, and will disappear when life = 0 */
    private int life = 30;
    
    public Bullet()
    {
    }
    
    public Bullet(Vector speed, int rotation)
    {
        super(speed);
        setRotation(rotation);
        addForce(new Vector(rotation, 15));
        Greenfoot.playSound("EnergyGun.wav");
    }
    
    /**
     * The bullet will damage asteroids if it hits them.
     */
    public void act()
    {
        if(life <= 0) {
            getWorld().removeObject(this);
        } 
        else {
            life--;
            move();
            checkAsteroidHit();
        }
    }
    
    /**
     * Check whether we have hit an asteroid.
     */
    private void checkAsteroidHit()
    {
        Asteroid asteroid = (Asteroid) getOneIntersectingObject(Asteroid.class);
        if (asteroid != null){
            getWorld().removeObject(this);
            asteroid.hit(damage);
        }
    }
}
